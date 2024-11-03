---
title: "Build a System Monitor TUI (Terminal UI) in Go"
author: ivan
date: 2024-11-02 12:22:21 +0100
categories: [Programming, Go]
tags: [Go, Fundamentals]
toc: true
mermaid: true
math: true
---

> The goal of this article is to:
> 1. Learn how to access and read system information.
> 2. Learn how to build a text-based user interface in the Terminal.
>
> We will be using two libraries to achieve this goal [shirou/gopsutil](https://github.com/shirou/gopsutil) and [charmbracelet/bubbletea](https://github.com/charmbracelet/bubbletea)
> 
> If you want to skip my ramblings and just see the final solution, the source code is available [here](https://github.com/ivan-penchev/system-monitor-tui).
{: .prompt-info }

## Why a TUI?

As an engineer, I often have to interact with systems. And I have to do it in a manner where it is consistent, meaning if I am creating a vendor account in 2022 or today, I must always use the same procedure. The problem? Human memory is far from flawless, we may have done a procedure over, and over, and over again... and forget to do a small step. You don't have to look further than the case of the [2018 Hawaii false missile alert](https://en.wikipedia.org/wiki/2018_Hawaii_false_missile_alert) to see the potential consequences of such lapses.


So how do we as engineers combat this? We try to automate the interactions so we do not have to rely on our memory. Scripting repetitive tasks is a common approach.
I can't remember how often I have seen, or written myself, a bash script similar to:

```bash
#!/bin/bash

echo "Select an action:"
select action in "Create Vendor Account" "Update Vendor Information" "Deactivate Vendor Account" "Exit"
do
    case $action in
        "Create Vendor Account")
            # Command to create a vendor account
            echo "Creating vendor account..."
            ;;
        "Update Vendor Information")
            # Command to update vendor information
            echo "Updating vendor information..."
            ;;
        "Deactivate Vendor Account")
            # Command to deactivate a vendor account
            echo "Deactivating vendor account..."
            ;;
        "Exit")
            break
            ;;
        *)
            echo "Invalid option. Please try again."
            ;;
    esac
done
```
In layman's terms, this is a 'menu' that allows the user to choose from a list of actions. Once an action is selected, the corresponding command is executed. While this is a step in the right direction, the interface is purely text-based and can be cumbersome to navigate, especially for more complex interactions.

While this script is functional, it lacks the user-friendly interface that a TUI can provide. A TUI provides a graphical interface within a text-only terminal environment, improving user interaction by offering elements like menus, buttons, and forms. It bridges the gap between the ease of use found in Graphical User Interfaces (GUIs) and the power and accessibility of command-line interfaces. TUIs are particularly useful when GUIs are not practical.

Moreover, TUIs can offer a better user experience than simple scripts. They can provide immediate feedback, error handling, and a more intuitive navigation system, which can be essential for complex or critical tasks. By automating procedures through a TUI, engineers can ensure that processes are followed accurately and efficiently, minimizing the risk of human error and increasing productivity.

## Starting up

Let us create a basic skeleton for our project. Usually I would advice you follow the [Standard Go Project Layout](https://github.com/golang-standards/project-layout), but due to the expected size of this project, I will just plug everything into one package.

``` bash
mkdir system-monitor-tui
cd system-monitor-tui
go mod init github.com/ivan-penchev/system-monitor-tui
touch main.go
```

```go
package main

import (
	"fmt"
)

func main() {
	printSystemInfo()
}

func printSystemInfo() {
	// TO-DO: Retrieve system information
	fmt.Println("CPU Percentage    :", "TO-DO")
	fmt.Println("Memory Percentage :", "TO-DO")
	fmt.Println("Running Processes :", "TO-DO")
}

```
{: file='main.go'}

Congrats we did the the equivalent of a "hello world" :)

With our project structure in place, we can now focus on retrieving system resources using the `gopsutil` library.

## Getting System Resources using gopsutil
`gopsutil` is a Go package that provides a set of functions to retrieve system and process metrics from various platforms. It serves as a convenient and cross-platform way to gather information about system resources such as CPU usage, memory usage, disk usage, and more.

In other words, with this package, we can run our code on Windows, Linux, and Mac, and it will still provide us with the system resources.

```bash
touch systeminfo.go
go get github.com/shirou/gopsutil/v4
```

We would create 3 methods:

1. `GetCPUStats` this function retrieves CPU usage statistics and converts them into percentage values. The `cpu.Times(false)` function returns the CPU times, and the function calculates the percentage of time spent in various states (user, system, idle, etc.).
2. `GetMEMStats` this function retrieves memory usage statistics using `mem.VirtualMemory()`. It returns the total, used, free, and available memory, along with the percentage of used memory.
3. `GetProcesses` this function retrieves a list of running processes. It gathers information such as PID, name, username, memory usage, CPU usage percentage, and running time for each process. The processes are then sorted by CPU usage percentage in descending order, and the top n processes are returned.

<details markdown="1">
  <summary><b>View the full systeminfo.go file</b></summary>

```go
package main

import (
	"fmt"
	"sort"
	"time"

	"github.com/shirou/gopsutil/v4/cpu"
	"github.com/shirou/gopsutil/v4/mem"
	"github.com/shirou/gopsutil/v4/process"
)

func GetCPUStats() (cpu.TimesStat, error) {
	stats, err := cpu.Times(false)
	if err != nil {
		return cpu.TimesStat{}, err
	}
	if len(stats) == 0 {
		return cpu.TimesStat{}, nil
	}

	currStats := stats[0]

	total := currStats.User + currStats.System + currStats.Idle + currStats.Nice +
		currStats.Iowait + currStats.Irq + currStats.Softirq + currStats.Steal +
		currStats.Guest

	if total == 0 {
		return cpu.TimesStat{}, nil
	}

	// Overwrite TimesStat fields with percentage values
	currStats.User = (currStats.User / total) * 100
	currStats.System = (currStats.System / total) * 100
	currStats.Idle = (currStats.Idle / total) * 100
	currStats.Nice = (currStats.Nice / total) * 100
	currStats.Iowait = (currStats.Iowait / total) * 100
	currStats.Irq = (currStats.Irq / total) * 100
	currStats.Softirq = (currStats.Softirq / total) * 100
	currStats.Steal = (currStats.Steal / total) * 100
	currStats.Guest = (currStats.Guest / total) * 100

	return currStats, nil
}

func GetMEMStats() (mem.VirtualMemoryStat, error) {
	v, err := mem.VirtualMemory()
	if err != nil {
		return mem.VirtualMemoryStat{}, err
	}

	return mem.VirtualMemoryStat{
		Total:       v.Total,
		Used:        v.Used,
		Free:        v.Free,
		UsedPercent: v.UsedPercent,
		Available:   v.Available,
	}, nil
}

type ProcessInfo struct {
	PID         int32
	Name        string
	Username    string
	Memory      uint64
	CPUPercent  float64 // CPU usage percentage
	RunningTime string
}

func GetProcesses(n int) ([]ProcessInfo, error) {
	procs, err := process.Processes()
	if err != nil {
		return nil, err
	}

	var processInfos []ProcessInfo
	for _, p := range procs {
		pid := p.Pid
		name, err := p.Name()
		if err != nil {
			name = "Unknown"
		}

		createTime, err := p.CreateTime()
		if err != nil {
			createTime = 0
		}

		startTime := time.Unix(createTime/1000, 0)
		runningTime := time.Since(startTime).Truncate(time.Second)

		username, err := p.Username()
		if err != nil {
			name = "Unknown"
		}

		memoryInfo, err := p.MemoryInfo()
		if err != nil {
			processInfos = append(processInfos, ProcessInfo{
				PID:         pid,
				Name:        name,
				RunningTime: runningTime.String(),
				Username:    username,
				Memory:      0,
				CPUPercent:  0,
			})
			continue
		}

		memory := memoryInfo.RSS

		cpuPercent, err := p.CPUPercent()
		if err != nil {
			cpuPercent = 0
		}

		processInfos = append(processInfos, ProcessInfo{
			PID:         pid,
			Name:        name,
			RunningTime: runningTime.String(),
			Username:    username,
			Memory:      memory,
			CPUPercent:  cpuPercent,
		})
	}

	sort.Slice(processInfos, func(i, j int) bool {
		return processInfos[i].CPUPercent > processInfos[j].CPUPercent
	})

	if len(processInfos) > n {
		processInfos = processInfos[:n]
	}

	return processInfos, nil
}

func convertBytes(bytes uint64) (string, string) {
	const (
		KB = 1024
		MB = KB * 1024
		GB = MB * 1024
	)

	switch {
	case bytes >= GB:
		return fmt.Sprintf("%.2f", float64(bytes)/float64(GB)), "GB"
	case bytes >= MB:
		return fmt.Sprintf("%.2f", float64(bytes)/float64(MB)), "MB"
	case bytes >= KB:
		return fmt.Sprintf("%.2f", float64(bytes)/float64(KB)), "KB"
	default:
		return fmt.Sprintf("%d", bytes), "B"
	}
}

```
{: file='systeminfo.go'}
</details>

Now that we have our system information functions ready, let's integrate them into our main program.

```go
package main

import (
	"fmt"
)

func main() {
	printSystemInfo()
}

func printSystemInfo() {
	cpuUsage, _ := GetCPUStats()
	memoryUsage, _ := GetMEMStats()
	runningProcesses, _ := GetProcesses(10) // Get top 10 CPU intensive processes

	fmt.Println("CPU Percentage    :", cpuUsage)
	fmt.Println("Memory Percentage :", memoryUsage)
	fmt.Println("Running Processes :", runningProcesses)
}
```
{: file='main.go'}

### Looking and understanding the output

Now if everything worked as expected you would be able to see the output. 
Lets examine it, to understand it better. this would help us when we have to design our TUI.

1. CPU Percentage output 

```json
{
  "cpu": "cpu-total",      // The CPU identifier.
  "user": 0.2,             // Percentage of CPU time spent in user mode.
  "system": 0.5,           // Percentage of CPU time spent in system mode.
  "idle": 99.2,            // Percentage of CPU time spent idle.
  "nice": 0.0,             // Percentage of CPU time spent on low-priority processes.
  "iowait": 0.0,           // Percentage of CPU time spent waiting for I/O operations.
  "irq": 0.0,              // Percentage of CPU time spent servicing interrupts.
  "softirq": 0.0,          // Percentage of CPU time spent servicing software interrupts.
  "steal": 0.0,            // Percentage of CPU time stolen by other operating systems running in a virtualized environment.
  "guest": 0.0,            // Percentage of CPU time spent running guest operating systems.
  "guestNice": 0.0         // Percentage of CPU time spent running guest operating systems with a low priority.
}
```

2. Memory Percentage output:

```json
{
  "total": 33411727360,    // Total physical memory available.
  "available": 17795051520, // Memory available for use.
  "used": 15616675840,     // Memory currently used.
  "usedPercent": 46,       // Percentage of memory used.
  "free": 17795051520,     // Free memory.
}
```

3. Running Processes output: 

```json
[
  {
    "PID": 7896,                          // Process ID.
    "Name": "system-monitor-tui.exe",     // Name of the process.
    "Username": "NETA\\ivan",             // Username of the process owner.
    "Memory": 7536640,                    // Memory used by the process (in bytes).
    "CPUPercent": 85.02660992784301,      // CPU usage percentage of the process.
    "RunningTime": "0s"                   // How long the process has been running.
  }
  //... omitted
]
```

### Using go routines and channels to refresh information?

> This section is optional if you are already familiar with how goroutines and channels work.
> 
> The reason I am highlighting this, is because when using [bubbletea](https://github.com/charmbracelet/bubbletea) the framework abstract those concepts away.
> Yet it is still a good idea to be familiar with  the "magic" under the hood of the framework.
{: .prompt-info }

Goroutines are distinct from traditional threads in that they are managed by the Go runtime, which is responsible for their scheduling and execution. They are more lightweight than operating system threads, and many goroutines can run concurrently on a small number of operating system threads. This enables efficient parallelism and concurrency without the overhead associated with traditional threading models.

In Go, you can create a new goroutine by using the go keyword followed by a function call. For example:

```go
func main() {
	go worker() // Start a worker goroutine

	// Keep the main goroutine running
	select {}
}

func worker() {
	for {
		fmt.Println("Working...")
		time.Sleep(time.Second)
	}
}
```
In this example, the worker function runs in a separate goroutine, printing "Working..." every second. The select{} statement ensures that the main Goroutine continues running indefinitely, preventing the program from exiting immediately, thus allowing the worker goroutine to continue its work concurrently.

```go
func main() {
	// Start the worker goroutine using an anonymous function
	go func() {
	 for {
		 worker()
		 time.Sleep(time.Second)
	 }
	}()

	// Keep the main goroutine running
	select {}
}

func worker() {
	for {
		fmt.Println("Working...")
		// You may include additional logic here if needed
	}
```

Now that you've learned how goroutines work , update your program to continuously print CPU usage, memory usage, and running processes.

```go
package main

import (
	"fmt"
	"os"
	"os/signal"
	"sync"
	"syscall"
	"time"
)

func main() {
	// Create a channel to listen for OS signals
	stopChan := make(chan os.Signal, 1)
	signal.Notify(stopChan, syscall.SIGINT, syscall.SIGTERM)

	// Create a channel to signal when to print system info
	printChan := make(chan struct{})

	// Use a wait group to wait for all goroutines to finish
	var wg sync.WaitGroup

	// Start a goroutine to handle the printing
	wg.Add(1)
	go func() {
		defer wg.Done()
		printSystemInfo(printChan)
	}()

	// Create a ticker to signal every 10 seconds
	ticker := time.NewTicker(10 * time.Second)
	defer ticker.Stop()

	// Main loop to handle signals and ticker
    // as well as keep the main goroutine running
	for {
		select {
		case <-ticker.C:
			printChan <- struct{}{}
		case <-stopChan:
			fmt.Println("Received stop signal, stopping...")
			close(printChan)
			wg.Wait()
			return
		}
	}
}
func printSystemInfo(printChan chan struct{}) {
	for range printChan {
		cpuUsage, _ := GetCPUStats()

		memoryUsage, _ := GetMEMStats()

		runningProcesses, _ := GetProcesses(10)

		fmt.Println("CPU Percentage    :", cpuUsage)
		fmt.Println("Memory Percentage :", memoryUsage)
		fmt.Println("Running Processes :", runningProcesses)
	}
}

```
{: file='main.go'}


This is a very standard pattern for fetching and refreshing data.
Our framework abstracts and handles all this underneath, so we wouldn't actually be writing this directly.

## Writing TUI using bubbletea

Creating a Text User Interface (TUI) can be a complex task, especially if you aim to build a robust, interactive, and visually appealing interface. Using a framework like Bubble Tea can significantly simplify this process. 
Bubble Tea is a modern TUI framework for Go, inspired by [The Elm Architecture](https://guide.elm-lang.org/architecture/). Here are some specific benefits of using Bubble Tea:

Firstly, it employs a Model-Update-View (MUV) architecture, which ensures a clear separation of concerns. The model is responsible for holding the application state, the update function handles state changes, and the view function renders the state. This structured approach simplifies the management of complex state transitions.

Secondly, Bubble Tea provides a declarative UI, which significantly enhances ease of use. It allows developers to straightforwardly declare what the UI should look like, making it easier to understand and reason about the UI and its changes over time.

Thirdly, Bubble Tea includes a rich library of built-in components, such as text inputs, lists, and tables, which can be used out of the box. These components are also highly customizable, allowing developers to extend and modify them to meet specific needs.

Lastly, Bubble Tea supports concurrency, enabling asynchronous operations. This feature allows background tasks, such as data fetching, to be handled without blocking the UI, ensuring a smooth and responsive user experience.

For our project we would be using the following packages:
1. [charmbracelet/bubbletea](https://github.com/charmbracelet/bubbletea) - is the core framework for building TUIs in Go.  
2. [charmbracelet/bubbles](https://github.com/charmbracelet/bubbles) - a collection of reusable components for building TUIs. It includes various elements like text inputs, lists, tables, and more, which can be easily integrated into your application. 
3. [charmbracelet/lipgloss](https://github.com/charmbracelet/lipgloss) - a package for styling terminal applications. It provides tools to add colors, styles, and layouts to your TUI, allowing you to create visually appealing interfaces. 

```bash
touch tui.go
go get github.com/charmbracelet/lipgloss
go get github.com/charmbracelet/bubbletea
go get github.com/charmbracelet/bubbles
```

With these packages installed, we can start defining our TUI model.

### Define our Model (struct)
In order to create our TUI we must define a model.
For something to be considered a `Model` it must have the following 3 methods:
```go
type Model interface {
    // Init is the first function that will be called. It returns an optional
    // initial command. To not perform an initial command return nil.
    Init() Cmd

    // Update is called when a message is received. Use it to inspect messages
    // and, in response, update the model and/or send a command.
    Update(Msg) (Model, Cmd)

    // View renders the program's UI, which is just a string. The view is
    // rendered after every Update.
    View() string
}
```
We need to store some data for our Model. We will use the following model to store the data for our state:

```go
type model struct {
	width  int
	height int

	processTable table.Model
	tableStyle   table.Styles
	baseStyle    lipgloss.Style
	viewStyle    lipgloss.Style

	CpuUsage cpu.TimesStat
	MemUsage mem.VirtualMemoryStat
}

type TickMsg time.Time
```
{: file='tui.go'}

Next, let's define the view for our model.

### Define our Model (View)

The view defines how the user interface (UI) is rendered and what elements are displayed.

```go
func (m model) View() string {
	// Sets the width of the column to the width of the terminal (m.width) and adds padding of 1 unit on the top.
	// Render is a method from the lipgloss package that applies the defined style and returns a function that can render styled content.
	column := m.baseStyle.Width(m.width).Padding(1, 0, 0, 0).Render
	// Set the content to match the terminal dimensions (m.width and m.height).
	content := m.baseStyle.
		Width(m.width).
		Height(m.height).
		Render(
			// Vertically join multiple elements aligned to the left.
			lipgloss.JoinVertical(lipgloss.Left,
				column(m.ViewHeader()),
				column(m.ViewProcess()),
			),
		)

	return content
}
```
{: file='tui.go'}

<details markdown="1">
  <summary><b>View ViewHeader() function</b></summary>

```go

// Uses lipgloss.JoinVertical and lipgloss.JoinHorizontal to arrange the header content.
// It displays the last update time and various system statistics (CPU and memory usage) in a structured format.
func (m model) ViewHeader() string {
	// defines the style for list items, including borders, border color, height, and padding.
	list := m.baseStyle.
		Border(lipgloss.NormalBorder(), false, true, false, false).
		BorderForeground(Color.Border).
		Height(4).
		Padding(0, 1)

	// applies bold styling to the text.
	listHeader := m.baseStyle.Bold(true).Render

	// helper function that formats a key-value pair with an optional suffix. It aligns the value to the right and renders it with the specified style.
	listItem := func(key string, value string, suffix ...string) string {
		finalSuffix := ""
		if len(suffix) > 0 {
			finalSuffix = suffix[0]
		}

		listItemValue := m.baseStyle.Align(lipgloss.Right).Render(fmt.Sprintf("%s%s", value, finalSuffix))

		listItemKey := func(key string) string {
			return m.baseStyle.Render(key + ":")
		}

		return fmt.Sprintf("%s %s", listItemKey(key), listItemValue)
	}

	return m.viewStyle.Render(
		lipgloss.JoinVertical(lipgloss.Top,
			fmt.Sprintf("Last update: %d milliseconds ago\n", time.Now().Sub(m.lastUpdate).Milliseconds()),
			lipgloss.JoinHorizontal(lipgloss.Top,
				// Progress Bars
				list.Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader("% Usage"),
						listItem("CPU", fmt.Sprintf("%s %.1f", ProgressBar(100-m.CpuUsage.Idle, m.baseStyle), 100-m.CpuUsage.Idle), "%"),
						listItem("MEM", fmt.Sprintf("%s %.1f", ProgressBar(m.MemUsage.UsedPercent, m.baseStyle), m.MemUsage.UsedPercent), "%"),
					),
				),

				// CPU
				list.Border(lipgloss.NormalBorder(), false).Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader("CPU"),
						listItem("user", fmt.Sprintf("%.1f", m.CpuUsage.User), "%"),
						listItem("sys", fmt.Sprintf("%.1f", m.CpuUsage.System), "%"),
						listItem("idle", fmt.Sprintf("%.1f", m.CpuUsage.Idle), "%"),
					),
				),
				list.Border(lipgloss.NormalBorder(), false).Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader(""),
						listItem("nice", fmt.Sprintf("%.1f", m.CpuUsage.Nice), "%"),
						listItem("iowait", fmt.Sprintf("%.1f", m.CpuUsage.Iowait), "%"),
						listItem("irq", fmt.Sprintf("%.1f", m.CpuUsage.Irq), "%"),
					),
				),
				list.Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader(""),
						listItem("softirq", fmt.Sprintf("%.1f", m.CpuUsage.Softirq), "%"),
						listItem("steal", fmt.Sprintf("%.1f", m.CpuUsage.Steal), "%"),
						listItem("guest", fmt.Sprintf("%.1f", m.CpuUsage.Guest), "%"),
					),
				),

				// MEM
				list.Border(lipgloss.NormalBorder(), false).Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader("MEM"),
						func() string {
							value, unit := convertBytes(m.MemUsage.Total)
							return listItem("total", value, unit)
						}(),
						func() string {
							value, unit := convertBytes(m.MemUsage.Used)
							return listItem("used", value, unit)
						}(),
						func() string {
							value, unit := convertBytes(m.MemUsage.Available)
							return listItem("free", value, unit)
						}(),
					),
				),
				list.Render(
					lipgloss.JoinVertical(lipgloss.Left,
						listHeader(""),
						func() string {
							value, unit := convertBytes(m.MemUsage.Active)
							return listItem("active", value, unit)
						}(),
						func() string {
							value, unit := convertBytes(m.MemUsage.Buffers)
							return listItem("buffers", value, unit)
						}(),
						func() string {
							value, unit := convertBytes(m.MemUsage.Cached)
							return listItem("cached", value, unit)
						}(),
					),
				),
			),
		),
	)
}
```
{: file='tui.go'}
</details>

<details markdown="1">
  <summary><b>View ViewProcess() and ProgressBar() functions</b></summary>

```go
// Gets the View from the table component and renders is on our View
func (m model) ViewProcess() string {
	return m.viewStyle.Render(m.processTable.View())
}

// creates a visual representation of a percentage as a progress bar.
func ProgressBar(percentage float64, baseStyle lipgloss.Style) string {
	totalBars := 20
	fillBars := int(percentage / 100 * float64(totalBars))
	// renders the filled part of the progress bar with a green color.
	filled := baseStyle.
		Foreground(Color.Green).
		Render(strings.Repeat("|", fillBars))
	// renders the empty part of the progress bar with a secondary color.
	empty := baseStyle.
		Foreground(Color.Secondary).
		Render(strings.Repeat("|", totalBars-fillBars))

	return baseStyle.Render(fmt.Sprintf("%s%s%s%s", "[", filled, empty, "]"))
}

```
{: file='tui.go'}
</details>

With our view in place, let's move on to initializing our model.

### Define our Model (Init)
The `Init` function is part of the `tea.Model` interface and is called once when the Bubble Tea program starts. 
It is used to initialize the model and set up any initial commands. If we do not wish to perform any initial commands, we simply return nil.

```go
// Calls the tickEvery function to set up a command that sends a TickMsg every second.
// This command will be executed immediately when the program starts, initiating the periodic updates.
func (m model) Init() tea.Cmd {
	return tickEvery()
}

func tickEvery() tea.Cmd {
	// tea.Every function is a helper function from the Bubble Tea framework
	// that schedules a command to run at regular intervals.
	return tea.Every(time.Second,
		// Callback function that takes the current time (t time.Time) as a parameter and returns a message (tea.Msg).
		// This callback is invoked every second.
		func(t time.Time) tea.Msg {
			return TickMsg(t)
		})
}
```
{: file='tui.go'}

### Define our Model (Update)
Once we have initialized our model, and configured how it must look (view), the last thing we must do is handle updates.
This is where the Update function of the model comes into play.

```go

// Takes a tea.Msg as input and uses a type switch to handle different types of messages.
// Each case in the switch statement corresponds to a specific message type.
func (m model) Update(msg tea.Msg) (tea.Model, tea.Cmd) {
	switch msg := msg.(type) {

	// message is sent when the window size changes
	// save to reflect the new dimensions of the terminal window.
	case tea.WindowSizeMsg:
		m.height = msg.Height
		m.width = msg.Width

	// message is sent when a key is pressed.
	case tea.KeyMsg:
		switch msg.String() {
		// Toggles the focus state of the process table
		case "esc":
			if m.processTable.Focused() {
				m.tableStyle.Selected = m.baseStyle
				m.processTable.SetStyles(m.tableStyle)
				m.processTable.Blur()
			} else {
				m.tableStyle.Selected = m.tableStyle.Selected.Background(Color.Highlight)
				m.processTable.SetStyles(m.tableStyle)
				m.processTable.Focus()
			}
		// Moves the focus up in the process table if the table is focused.
		case "up", "k":
			if m.processTable.Focused() {
				m.processTable.MoveUp(1)
			}
		// Moves the focus down in the process table if the table is focused.
		case "down", "j":
			if m.processTable.Focused() {
				m.processTable.MoveDown(1)
			}
		// Quits the program by returning the tea.Quit command.
		case "q", "ctrl+c":
			return m, tea.Quit
		}
	// This custom message is sent periodically by the tickEvery function.
	// The model's lastUpdate field is updated to the current time.
	// Fetching CPU Stats, Memory Stats & Processes
	// Returning Command: The tickEvery command is returned to ensure that the TickMsg continues to be sent periodically.
	case TickMsg:
		m.lastUpdate = time.Time(msg)
		cpuStats, err := GetCPUStats()
		if err != nil {
			slog.Error("Could not get CPU info", "error", err)
		} else {
			m.CpuUsage = cpuStats
		}

		memStats, err := GetMEMStats()
		if err != nil {
			slog.Error("Could not get memory info", "error", err)
		} else {
			m.MemUsage = memStats
		}

		procs, err := GetProcesses(5)
		if err != nil {
			slog.Error("Could not get processes", "error", err)
		} else {
			rows := []table.Row{}
			for _, p := range procs {
				memString, memUnit := convertBytes(p.Memory)
				rows = append(rows, table.Row{
					fmt.Sprintf("%d", p.PID),
					p.Name,
					fmt.Sprintf("%.2f%%", p.CPUPercent),
					fmt.Sprintf("%s %s", memString, memUnit),
					p.Username,
					p.RunningTime,
				})
			}
			m.processTable.SetRows(rows)
		}

		return m, tickEvery()
	}
	// If the message type does not match any of the handled cases, the model is returned unchanged, and no new command is issued.
	return m, nil
}

func (m model) View() string {
	// Sets the width of the column to the width of the terminal (m.width) and adds padding of 1 unit on the top.
	// Render is a method from the lipgloss package that applies the defined style and returns a function that can render styled content.
	column := m.baseStyle.Width(m.width).Padding(1, 0, 0, 0).Render
	// Set the content to match the terminal dimensions (m.width and m.height).
	content := m.baseStyle.
		Width(m.width).
		Height(m.height).
		Render(
			// Vertically join multiple elements aligned to the left.
			lipgloss.JoinVertical(lipgloss.Left,
				column(m.ViewHeader()),
				column(m.ViewProcess()),
			),
		)

	return content
}
```
{: file='tui.go'}

Finally, let's put everything together and run our TUI application.

### Putting it all together

As a final step, after defining our Model, we need to run the application. To do this, we will update our main.go file.

```go

func main() {
	tableStyle := table.DefaultStyles()
	tableStyle.Selected = lipgloss.NewStyle().Background(Color.Highlight)

	// Creates a new table with specified columns and initial empty rows.
	processTable := table.New(
		// We use this to define our table "header"
		table.WithColumns([]table.Column{
			{Title: "PID", Width: 10},
			{Title: "Name", Width: 25},
			{Title: "CPU", Width: 12},
			{Title: "MEM", Width: 12},
			{Title: "Username", Width: 12},
			{Title: "Time", Width: 12},
		}),
		table.WithRows([]table.Row{}),
		table.WithFocused(true),
		table.WithHeight(20),
		table.WithStyles(tableStyle),
	)

	m := model{
		processTable: processTable,
		tableStyle:   tableStyle,
		baseStyle:    lipgloss.NewStyle(),
		viewStyle:    lipgloss.NewStyle(),
	}

	// Create a new Bubble Tea program with the model and enable alternate screen
	p := tea.NewProgram(m, tea.WithAltScreen())

	// Run the program and handle any errors
	if _, err := p.Run(); err != nil {
		log.Fatalf("Error running program: %v", err)
	}
}

```
{: file='tui.go'}

With our main.go file updated, we can now run our application and see the TUI in action.

If you wired everything correctly, you can run the code with ```go run .```
The result should look like:
![create-tui-with-go]({{ "/assets/img/create-tui-with-go.png" | relative_url }})


## Conclusion

While the software we created is practically 'cloning' [htop](https://htop.dev/), it is nonetheless impressive what you can achieve with the available libraries for Go.

We have successfully built a cross-platform TUI that provides real-time system monitoring. And it only took an hour to create.

Keep building, keep coding!

> The following post has been hugely inspired by [World Cup 2022 CLI Dashboard](https://github.com/cedricblondeau/world-cup-2022-cli-dashboard/tree/main)
