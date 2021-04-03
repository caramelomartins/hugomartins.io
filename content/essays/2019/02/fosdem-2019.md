---
title: "FOSDEM 2019 - A Review"
date: 2019-02-14T23:00:00+00:00
draft: false
summary: This year I decided to make the trip to Belgium and attend FOSDEM 2019, on February 2 and 3. I have an affection for _open source_ software and I, obviously, had an interest in seeing the conference. I was also curious to understand what the environment would feel like, at a big conference - never have I went to anything larger than 500 people.
aliases: ["/essays/fosdem-2019-a-review/", "/blog/fosdem-2019-a-review/"]
categories:
  - oss
  - conferences
---

## Introduction

This year I decided to make the trip to Belgium and attend FOSDEM 2019, on February 2 and 3. I have an affection for _open source_ software and I, obviously, had an interest in seeing the conference. I was also curious to understand what the environment would feel like, at a big conference - never have I went to anything larger than 500 people.

I had a few talks scheduled, mostly around stuff I'm working currently. Infrastructure-related talks, how to manage infrastructure, how to use specific tools such as Kubernetes and some talks about DNS. More than the talks though, I was interested in wandering through the conference.

I've been present on both days, Saturday and Sunday, and I'll be going over the talks I sat through.

## Saturday

I've gotten to the conference around the beginning of the afternoon and decided to go straight to a talk, leaving the wandering for afterwards.

It started with [Challenges With Building End-to-End Encrypted Applications - Learnings From Etesync](https://fosdem.org/2019/schedule/event/challenges_with_building_end_to_end_encrypted_applications_learnings_from_etesync/). This talk felt a bit like advertising to me, to be honest, but, then again, a lot of talks at FOSDEM are intended to present projects or updates of specific projects. It was interesting to see the problems that _devs_ went through, when building end to end encrypted applications, but I would have liked to hear about some of the solutions. By the end of this talk, I was confronted with the first instance of organization issues, when the organization cut off the speaker, because he was running out of time. There were still about 5 minutes until the end of the scheduled talk - 10 minutes, if you count the time to empty the room - but the organization decided that it was more relevant to do Q&A, than to let the speaker finish the presentation.

I went to [DNS over HTTPS - the good, the bad and the ugly](https://fosdem.org/2019/schedule/event/dns_over_http/) after that by Daniel Stenberg, creator of cURL. I was really excited about this talk, because of the speaker and the topic. I was disappointed, as I couldn't hear most of the talk due to sound issues. The room was a huge auditorium and the sound was clear, which meant that we couldn't hear more than half of what the speaker said. Such a shame.

Saturday was finished by going to [Codifying infrastructure with Terraform for the future](https://fosdem.org/2019/schedule/event/terraform_best_practices/). This was a really entertaining and interesting talk. Anton Babenko, the speaker, was witty and laid out some of the best practices, that one should aim for, when managing Terraform resources and modules. I am a newbie to Terraform and this was very interesting, from my perspective, to get a broader view of the end result of using Terraform.

## Sunday

I got into the conference around the same time as Saturday. I decided to first listen to [Open Source at DuckDuckGo](https://fosdem.org/2019/schedule/event/duckduckgo_open_source/). This talk was given at the same auditorium where, during Saturday, I had sat through the DNS-related talk, without hearing anything. The sound problems persisted and I had to give up and leave.

Saddened, I decided to go to [Computer Games with MicroPython](https://fosdem.org/2019/schedule/event/python_games_with_micropython/). This was unexpected because I didn't plan on going to this talk. Nonetheless, I am glad I went because it was very interesting. This talk explained how one can use MicroPython to build tiny games that are played and programmed in microcontrollers, what challenges exist in that field - mostly because of resource restrictions - and how to overcome them.

I stayed in the same room, to listen to a somewhat more technical talk on [Extending Numba](https://fosdem.org/2019/schedule/event/python_extending_numba/). This talk reviewed how to make Numba, a high-performance Python compiler, accept and process things it isn't able to, originally. The talk was very hands-on, with code snippets being shown and explained. It was interesting to get out of my comfort zone, by listening to a talk on a framework I didn't know, performing computations I didn't understand-

After that, I decided to get out of my comfort zone again and go to [How to build an automatic refactoring and migration toolkit](https://fosdem.org/2019/schedule/event/ml_on_code_automatic_refactoring/). Again, a room without microphones...I couldn't hear anything again for 20 minutes. The room was also so packed, which meant I couldn't leave. _Horrible_.

After another disappointment, I went [Fighting spam for fun and profit](https://fosdem.org/2019/schedule/event/spamassassin/) which was a talk about the new updates on SpamAssassin, which have been baked during the last 3 years. This talk was short, simple and straight to the point - like a good talk should be. This got my hopes up but we were already ending the conference.

For the last talk, I decided to go to [Writing a CNI - as easy as pie](https://fosdem.org/2019/schedule/event/containers_k8s_cni/). This talk was about _Container Network Interfaces_ in Kubernetes. It had the added bonus that, given the technicalities of the talk, it was mostly empty so I was hoping for a good talk. I was not disappointed!! It was the best talk I went to. The speaker walked us through what CNIs are and how they work. He then showed us, with interactive programming, how to code a basic CNI for Kubernetes, from scratch, in about 10 minute - which worked at the end of the demo. The first iteration of the CNI had a small bug and failed to initialize, the time was running out...at the last minute, he fixed the bug and the crowd cheered!

## Conclusions

At the end, it was a nice experience. Nonetheless, I have to say that having everyone volunteer for the organization can sometimes, unwittingly, cause the kinds of organizational issues that I mentioned: technical issues and confusion in some of the rooms. On the other hand, I noticed that a lot of people go to FOSDEM to meet other people and catch up, rather than to listen to talks. I feel like attending with this perspective, different than what I went there to do, might result in a completely different experience of the entire conference.
