---
title: "DevOps Principles of Feedback: Understanding What's Going On"
date: 2021-10-04
draft: false
summary: A summary of the principles behind enabling a “fast and constant” flow of feedback from production all the way back to engineers.
authors: Hugo Martins
categories: [ devops ]
---

A few months ago, I've written at length about DevOps themes, in essays such as [Three Ways of DevOps](https://hugomartins.io/essays/2021/01/three-ways-of-devops/) and [DevOps Principles of Flow: Deliver Faster](https://hugomartins.io/essays/2021/02/devops-principles-and-practices-of-flow/), based on reading [The DevOps Handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002). Since then, I've had this essay half-baked, sitting on my drafts and haven't been able to complete it up until now.

In this essay, I want to focus our attention on _The Principles of Feedback_.  I've previously summarized them as:

> _The Principles of Feedback_ are the guiding principles behind the Second Way. At their core, they enable a “fast and constant” flow of feedback from production all the way back to engineers. These principles focus on getting the necessary ambience and tools in place for engineers to monitor their work and quickly react to any adverse situations. They focus on amplifying feedback from the operational side of software to the development side of software.

In a simple analogy, if _The Principles of Flow_ focus on the  _left-to-right_ flow of information (e.g. from "dev" to "ops"), _The Principles of Feedback_ guide the _right-to-left_ flow of information (e.g. from "ops" back to "dev").

## Principles

> We make our system of work safer by creating fast, frequent, high quality information flow throughout our value stream and our organization, which includes feedback and feedforward loops. This allows us to detect and remediate problems while they are smaller, cheaper and easier to fix; avert problems while they are smaller, cheaper, and easier to fix; avert problems before they cause catastrophe; and create organizational learning that we integrate into future work. _(The DevOps Handbook, pp. 27)_

To achieve what the above quote proposes, _The DevOps Handbook_ outlines the following principles:

1. *Working Safely within Complex Systems*: We must create the necessary environment in which we can work in our systems "without fear, confident that any errors will be detected quickly, long before they cause catastrophic outcomes (...)".
2. *See Problems as They Occur*: We need to ensure we have telemetry to accompany our production environment and be able to effectively monitor any incidents long before they affect customers. We can also use telemetry to validate we are achieving our desired goals.
3. *Swarm and Solve Problems to Build New Knowledge*: Swarming, [from Wikipedia](https://en.wikipedia.org/wiki/Swarm_behaviour),  "is a collective behavior exhibited by entities, particularly animals, of similar size which aggregate together, perhaps milling about the same spot or perhaps moving en masse or migrating in some direction."  In the technological sense, this means that when an issue, even the smallest of issues, occurs, all elements of a team must unite to resolve the problems as quickly as possible.
4. *Keep Pushing Quality Closer to the Source*: Ensure that quality is enforced by our peers, rather than top-down bureaucratic processes.

## Telemetry Everywhere

A direct, but at times complex, way of abiding by the two initial principles, is to ensure that telemetry is available to detect and solve problems in real-time. For this to be a possibility, we should have centralized systems for telemetry infrastructure used by the entire organization. We should also ensure that applications have appropriate logging that can help production issues.

Having this infrastructure in place helps use telemetry to guide problem-solving, rather than relying on semi-blind troubleshooting approaches. Apart from using telemetry to solve problems, we should also push for telemetry to be used for production metrics that are outside the scope of daily work such as, for example, number of successful logins. These metrics can then be used as SLIs that contribute to more effective SLOs. 

Another important aspect of telemetry is that it cannot be a walled garden. We must ensure that telemetry can be self-service,  providing visibility to developers about the telemetry of their applications, extending this feedback loop to everyone inside the organization. This increases transparency and guarantees that everyone has a shared vision of "reality".

With telemetry available, we need to ensure that we use it to better anticipate problems and achieve our goals, this means statistical analysis, usage of anomaly detection techniques and automated alerts.

This fast feedback loop, with telemetry and infrastructure for self-service, allows for deployments to be safer, for developers to follow their work and see how their applications behave, have developers share on-call rotas and even manage their services in production - instead of having to rely on a dedicated operations team.

## Review & Coordinate Close to the Source

We should aim at creating review and coordination processes as close to the source code as possible. For example, avoiding change approval processes entirely, fearlessly cutting on bureaucratic processes, instead relying on peer-review processes, coordination and scheduling of changes, without formal approvals. Code reviews have been proven to improve code readability, a sense of ownership, as well as code transparency and contributions. We should _review_ the review process, to ensure that we are enabling the effectiveness of reviews, rather than creating a process that doesn't ensure the quality of the source code.

Reviewing and coordinating closer to the source also enables teams that _swarm_ whenever an issue occurs to troubleshoot it also contributes to the creation of shared knowledge, that becomes distributed throughout the team, allowing teams to be more effective and efficient.

Whenever possible bottom-up coordination is much better than top-down mandates that are based on bureaucratic processes.

## Conclusions

An organization that has a well set up infrastructure, which provides self-service telemetry, aligned with the autonomy of self-sufficient teams is much better equipped to face the challenges that modern development teams are facing every day.

These practices - outlined throughout this essay - are the core of the underlying practices that enable organizations to ensure there are feedback and feedforward loops in place that allow for errors to be cheaper, smaller and caught earlier.

I hope in the future to write more about the last way of DevOps, _The Principles of Continual Learning_, which are by far my favourite guidelines of the entire book because they are, at this moment, perhaps the lesser-known guidelines and the ones that are less put in practice.
