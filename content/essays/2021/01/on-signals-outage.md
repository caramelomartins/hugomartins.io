---
title: "On Signal's Outage"
date: 2021-01-26
draft: false
summary: ""
authors: Hugo Martins
categories: ["infrastructure"]
---

Last January 15th, Signal had an [outage](https://www.engadget.com/signal-recovers-from-outage-183256503.html?guccounter=1&guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS91cmw_c2E9dCZyY3Q9aiZxPSZlc3JjPXMmc291cmNlPXdlYiZjZD0mdmVkPTJhaFVLRXdpTzZkUF9sS3J1QWhYRW5Gd0tIYmJPQTJBUUZqQUNlZ1FJQkJBQyZ1cmw9aHR0cHMlM0ElMkYlMkZ3d3cuZW5nYWRnZXQuY29tJTJGc2lnbmFsLXJlY292ZXJzLWZyb20tb3V0YWdlLTE4MzI1NjUwMy5odG1sJnVzZz1BT3ZWYXcySllaUkE3SXdXSHByaUZSRnoteTF6&guce_referrer_sig=AQAAAG8yHMsdyzXFkgVEYT2qmVVC3KH1GZs40JvP0TFMs_TDxh9Y2ctOnUSIs6Tvz3D0HN3GKgdY8RcgnacnwdJVGwtfvUVB3dtxVul4yxcC__eWQxBx_LEPTlX2GoTfEQ2YKTeKNFRNF3jf_uE5kCZAcLprAg16zNa0aai9_cvkZA5f) [of](https://www.androidpolice.com/2021/01/16/signal-interference-messaging-app-struggling-with-downtime-in-wake-of-newfound-popularity/) [epic](https://www.theverge.com/2021/1/17/22235707/signal-back-app-privacy-encrypted-outage) [proportions](https://www.republicworld.com/world-news/rest-of-the-world-news/elon-musk-responds-to-signals-global-outage-tweet-after-millions-of-new-sign-ups.html). WhatsApp's mistakes with the communication of their privacy changes, along with the fact that they are considering making privacy modifications, lead a surge of new users to try and install Signal. This surge of new users was to much for Signal's infrastructure to bear.

Signal's server-side code has pretty much been stable throughout the outage, which indicates they are either running a different server code on their infrastructure, which is highly likely, or they had problems mostly caused by their clients. Or a combination of both of these. 

Although there's no official explanation as to why this outage happened, according to some [recent commits](https://github.com/signalapp/Signal-Android/commit/c95f0fce6ee3b78ff82fde865b2ee49288e1303f) to the Android client, made almost exactly after the outage started, it appears that the clients were actually worsening the situation by constantly retrying after a failure that resulted from the server's rejection of the updates - effectively creating even more load for the server's to bear with. There's also been some work related with session resets and automatic retries, with feature flags. I can only wonder if the clients caused more harm than good to an already bad issue on the server-side.