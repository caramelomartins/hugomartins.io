---
title: "DevOps Principles of Flow: Deliver Faster"
date: 2021-02-26
draft: false
summary: A summary of the principles behind increasing flow of work, along with some of its practices, such as automating infrastructure, automated testing, deployment pipelines and continuous integration.
authors: Hugo Martins
categories: [ devops ]
---

Last month, I wrote a summary about the [Three Ways of DevOps](http://localhost:1313/essays/2021/01/three-ways-of-devops/), after having read [The DevOps Handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002). In that essay, I summarized the three sets of guiding principles that constitute the essential  structure of DevOps: 

- _The Principles of Flow_;
- _The Principles of Feedback_;
-  _The Principles of Continual Learning_.

In this essay, I'll investigate _The Principles of Flow_, and associated practices, which I previously summarized as:

> The Principles of Flow are the guiding principles that define the First Way. Their focus is on enabling fast flow of work from the conception stage all the way to a completion stage. This means that we ought to focus on ensuring that work can flow, as quickly as possible, between idealization, implementation, testing, quality assurance and deployment. In a traditional manufacturing process, this would be the process of taking work from an initial stage of raw materials, all the way to a complete product at the end of the production line. Enabling this fast flow increases the competitive advantages because software becomes easier to produce, easier to modify and easier to maintain, meaning that organizations can, more quickly, adapt to constant changes in their surroundings.

Understanding these principles and implementing their practices can help organizations deliver faster, being more productive, as well as ensure that their work is carried out securely and according to pre-established standards.

## Principles

Improvement in delivery speed, which is the focus of _The Principles of Flow_, is meant to ensure that we achieve a state of "flow" in our delivery. This stream of work can be easier to achieve if we pursue the following principles (again, from _The DevOps Handbook_):

1. _Make Our Work Visible_: Ensure that all the work taking place is visible, in queues, to all stakeholders to allow for speedy organization and prioritization. Making work visible also eases the focus of teams because everyone will know, at all times, what the priorities are. An example of this is having work displayed in Kanban boards with each column representing the different sections of the value stream (e.g. development, ops and QA).
2. _Limit Work in Progress_: Limiting work-in-progress avoids interruptions throughout the day, increasing productivity. It also sets clear expectations and priorities, avoiding multitasking incentives.
3. _Reduce Batch Sizes_: Traditional approaches, such as _waterfall_, rely on large batches of work having to be complete before starting sequential large batches of work. It has been demonstrated by now that large batches of work reduce flow and decrease quality, with small batches of work being preferable.
4. _Reduce Number of Handoffs_: Each time work passes from one team to another requires a lot of communication and creates potential moments where work will be stopped, waiting for _something_ to be resolved, and reduces knowledge and context that inevitably will not be transferred between teams. Reducing the number of handoffs required between teams improves flow and overall quality of work.
5. _Eliminate Waste in the Value Stream_: Removing any obstacles to making work progress, no matter what they are. Examples of waste are: partially done work, unnecessary processes, manual and non-standard work, context switching and extra features.

Applied on their own, these principles might be ineffective but applied together they form a powerful combination that can increase flow of work, leading to competitive advantages for organizations and a more joyous place of work for all of those that are integrated in this process. 

We'll now see how we can implement some technical practices, in an organization, in order to better fulfill these principles. We can broadly separate them in the following categories:

- Change Definition of Done;
- Automated & Self-Service Infrastructure;
- Deployment Pipelines;
- Automated Testing;
- Continuous Integration;
- Decouple Deployments From Releases.

## Change _Definition of Done_

One quick and easy action to improve the flow of work, is to ensure that our _Definition of Done_ (DoD) includes running (or demoing) in production, or a production-like environment that can be relied upon.

Too often, if the *DoD* isn't set to be "have it in a production-like state", tasks will be left lingering on the background, waiting for deployment, until the moment that the deployment comes and everyone realizes that it hadn't been tested in a production-like setting ever. Afterwards, there's a moment of confusion because no one remembers what the task was meant to do, so it gets increasingly hard to test it again, context has been lost and the process needs to start over again. Or maybe a lot of changes have occurred and no one realized that there was a conflict between this change and another change, creating a deployment nuisance.

By modifying the *DoD* to include running (or demoing) in a production-like environment, we are also making the task visible for a longer time, ensuring that everyone is aware of the production-like state in which the task is at.

Finally, modifying the *DoD* creates incentives to improve deployment speeds and automation of infrastructure, as we will need to deploy more times, either to production or to a production-like environment, so the need will arise of becoming more efficient at it, otherwise it will start to become dull and bothersome.

## Automated & Self-Service Infrastructure

Starting with the foundations of any technological endeavor, an automated infrastructure can go a long way into allowing organizations to reduce batch sizes and limit work in progress. This happens because automated infrastructure, in particular self-service infrastructure, empowers developers to move at their own pace - allowing them to complete work faster and with less dependencies.

Infrastructure should not only be automated but also easier to rebuild than to repair. With existing tools such as Terraform and Ansible, this can easily be true and, by following this suggestion, it can decrease the amount of time and effort it takes to build infrastructure and offer it to developers.

We should be able to easily and quickly create _on demand_ environments, for development, quality assurance and production. Instead of having to manually provide environments for developers, we should aim at having platforms and tooling in place to allow for those environments to be self-service. Examples tools that can help achieve this dream are virtualization, IaaC, containerization and using public cloud services.

When infrastructure is easy to build and deploy, it allows for repeatable creation of systems, easy patching and upgrading, as well as scaling. Essentially creating _immutable infrastructure_ that doesn't allow manual changes in production systems.

Automated and self-service infrastructure also reduces the number of handoffs because teams don't have to keep relying on other teams to complete their work - in this case, to deploy their work.

So, _automated and self-service infrastructure_ can contribute to having less work-in-progress by allowing developers to quickly complete their tasks and move on, have reduced batch sizes because they can more easily complete quick batches than longer batches, and reduce the number of handoffs between teams because it removes dependencies on deploying work.

## Deployment Pipelines

With automated infrastructure, we can then proceed to ensure we can have appropriate automated deployment pipelines that rely on infrastructure built on demand. These pipelines are fundamental to allow developers to reduce batch sizes and limit work-in-progress because they will have their own dedicated environments on demand to execute their pipelines.

These deployment pipelines must be able to run all the time, at any time, granting developers complete freedom to perform on their own schedules. They should be able to segregate builds from tests, so that both can be executed in separate depending on the needs.

Build phases should be able to generate packages automatically with the appropriate configurations and then deploy those packages to environments that can replicate production-like environments. Before deploying, these automated deployment pipelines should ensure that code merged into trunk is always tested.

These directives make building and testing independent of the developer and its environment, as well as operations, which improves the feedback loop but it also ensures, again, that we can support some of the principles of flow with actionable practices. After automating infrastructure, having a deployment pipeline that is as automated as possible, at least to a production-like environment, ensures that work can continuously be done without a lot of halts, handoffs and time waiting for dependencies.

## Automated Testing

After having an appropriate deployment pipeline, we should focus on ensuring that automated testing exists, and it should be fast and reliable. Slow tests that take days to run won't have the same effect on the flow of work as automated testing that runs fast. Equally, if the test suite isn't reliable, it will not be a good foundation for assuring quality of one's work.  

Automated testing, that is fast and reliable, ensures faster flow and reduces the "fear factor" in deploying changes to production. For this to be a reality, we need a fast and reliable validation test suite composed of *unit tests*, *acceptance tests* and *integration tests*, at least.

Automated testing should allow us to catch errors as soon as possible, which is another reason why they should be quick to run. Ideally, tests should allow us to catch errors with the fastest category of testing possible (e.g. unit tests if a function stops behaving as it should or integration tests if we start interaction incorrectly with another component).

With automation of infrastructure and deployment pipelines in place, we should aim at automating "_all the things_", including all tests that are conducted manually. Finally, performance testing and non-functional requirements should also be built into out automation test suite (such as static analysis, dependency analysis, etc).

## Continuous Integration

With an appropriate deployment pipeline, along with an automated test suite, we are now capable of enabling continuous integration. Continuous integration means that we continuously build and automatically test all changes together, enabling developers to quickly close the feedback loop and understand if their code is actually working and performing correctly. 

We can achieve continuous integration by: reducing development batches to the smallest units possible, increasing the rate of code production while reducing the probability of defect introduction; adopting a "trunk-based" development practice where there are frequent commits to trunk and developers can develop their features in separate branches and then test them before committing to trunk.

## Decouple Deployments From Releases

Automated deployments can be better achieved if we decouple deployments from releases: deployments are installations of specified versions of software in a given environment while releases are _a_ moment when we make a feature available to customers. I've been thinking more about this recently because it is easy to get both confused and I have been guilty of using both words interchangeably for a long time. 

As I think more about it, joining the two makes it difficult to create accountability for outcomes, either successful or unsuccessful, while separating them empowers both developers and operations. Some deployment patterns that help decouple deployments from releases are *blue-green deployments*, *canary deployments*, *feature toggles* and *dark launches*. We've seen these patterns in action a lot in some of the most deploy intensive companies, such as Netflix, Amazon or Facebook.

## Conclusions

All of these practices enable us to prepare for automated low-risk releases. We can achieve these automated low-risk releases by automating the deployment process entirely, by implementing fast and reliable automated tests, and relying on ephemeral, consistent and reproducible environments. 

Automating everything enables automated self-service deployments with increased shared transparency, responsibility and accountability between teams. These technical approaches do not ensure that we can fulfill the prophecies that are put forth by the principles of "flow" but at least give us a fair chance of trying.