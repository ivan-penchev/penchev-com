---
title: 12 years on Webhostingpad.com (Honest review)
author: ivan
date: 2022-05-30 08:22:21 +0100
categories: [DevOps]
tags: [Reviews]
toc: true
---

## Introduction

The year is 2010 my familly of three was living with 160$ per month. My daily allowance was 0$, but I really really wanted to have my own website. So after doing some graphic work for the internet businessman with questionable morales, on 31st May 2010 I managed to get enough money to pay for hosting - 71.64$ that was the price for me to have a website for 3 years.


## Hosting options in 2010
Web hosting services evolved throughout the years. Today web hosting is cheap, accessible and convenient for modern users. There are different types of web hosting services you can use and take advantage of depending on your needs, preferences, and requirements. 

But remember this is 2010, and I just paid 71.64$ but what for?
Well, the cloud has been established, with Amazon launching Amazon Web Services in 2006 and Google launching Google Cloud Platform in 2008, but as you could imagine for this amount of money I was far away from the cloud. I was pretty much underground.

Hosting providers (like Hostinger, and Bluehost) were, I suppose also now, kings and they were offering many services best mostly, you could "buy" space inside of a VPS (Virtual Private Server) or you could "buy" space inside of Shared server. With the first (base version) costing 20-30$ per month, and the latter going for anything from 1-3$ per month, so who would like to buy the first? 

So yeah, I have close to no budget, and I can't really afford to go with more reputable providers, so I choose the next best I can afford - webhostingpad.com a US-based company regarded as a good web host with competitive pricing.

![webhostingpad-com-ui-2010]({{ "/assets/img/webhosting-pad-in-2010.png" | relative_url }})

Fig. 01 Webhostingpad.com in 2010

## Review

Let us get the worst out first.

1. Unbelievably Cheap, but only initially
2. Uptime is 99.99%, but in reality, is a bit worse (very critical for startup/hobby businesses)
3. Like other web hosts, WebHostingPad will also help you transfer an existing site over to their services for free. But it is very limited help.
4. Your daily backup options are limited (size must not exceed 6GB), only backup website files, not databases or emails and etc.
5. You need to pay for basic things like - SSH access or even the option to create manual backups.
6. Just as their website they have not innovated over the years (personal view)
7. Email spam filter is paid upgrade (hence you get a lot of spam)

Let us mention some of the pros:

1. EXCELLENT support (for easy things) and response rate
2. Huge ease of signup - just one page proccess
3. Free domain for the first year
4. 30-day money-back guarantee
5. DirectAdmin Admin page (huge discussion on DA vs Cpanel vs Plesk)
6. Quick and easy installation of popular apps and CMSs with Softaculous.
7. Unlimited emails


If I have to be honest I had mostly positive experiences. Beginner-Friendly Client Area and Control Panel. The staff were responsive to my inquiries. However the number of spam emails I received is huge, and the uptime, as well as response time of my small website, leaves much to be desired.

![webhostingpad-com-client-ui-2022]({{ "/assets/img/webhosting-pad-client-area-2022.png" | relative_url }})

Fig. 02 Webhostingpad.com Client Area in 2022


## Migrating

So why migrate, if it is "mostly positive experiences"?  Two reasons:

1. My skills have outgrown the platform.
2. The price has grown, but the service is essentially the same.

Let us look at the price

| Period start  | Period end    | Price paid (USD) |
| ------------- | ------------- | ---------------- |
| 31st May 2010 | 31st May 2013 | 71.64            |
| 31st May 2013 | 31st May 2016 | 71.64            |
| 31st May 2016 | 31st May 2019 | 145.96           |
| 31st May 2019 | 31st May 2022 | 145.36           |
| 31st May 2022 | ( ----- )     | 196.74           |

So what am I using this for? My main website, 6 databases (3 of which are dead), and an email server. Well that is way to much to pay in 2022.

How much traffic do I get on my website:

| Period               | Hits   | GB traffic |
| -------------------- | ------ | ---------- |
| 05.2020 - 12.2020    | 22895  | 0.25       |
| 01.2021- 12.2021     | 114934 | 4.17       |
| 01.2022-  29.05.2022 | 80122  | 2.82       |

With this amount of traffic, and my website being static I can just use [Azure Static Web Apps](https://azure.microsoft.com/en-us/services/app-service/static/), since it is free with bandwidth up to 100 GB per month.

With the databases, due to the nature and velocity of data, I decided to split them between Azure Postgresql and Firestore. It was a bit inconvenient to have to rewrite some of the store logic, especially when using NoSQL, but nothing major.

The major thing was the mail server. Now you can host that yourself...but you probably should not. Not only is there a fair amount of infrastructure involved (even though it is quite simplified nowadays), but just buying the infrastructure in some Cloud provider is expensive. I went with [Zoho mail](https://www.zoho.com/mail/) they had amazing pricing -> 0.9 euro per user. Well... I had multiple emails, but they were all for me, so I just createdthem under my username as aliases. On this price tier there are only 5GBs of space, which worried me a bit,  however, upon further checking for 12 years I have only used 335.34 MB of space for both my emails.

So this is the story of how I went from having to pay 196.74$ to paying 34.77$



## Conclusion

With the current landscape of hosting providers, I would **only** recommend you to choose webhostingpad.com if you are entirely new to the hosting space. And even then, with the insanely low price, do not stay after the initial years.

I have huge respect for all the support staff in webhostingpad.com and I wish them well.

Best wishes
Customer **o30ixzk2**
31/05/**2010**-31/05/**2022**

