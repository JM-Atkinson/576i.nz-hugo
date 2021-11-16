---
title: "A few themes in the world of the Cloud"
date: 2021-11-16T13:59:22+13:00
type: article
draft: true
tags: [ "internet technologies", "situation report", "product management" ]
---

{{< figure src="container.jpg" caption="" attr="Baycrest - Wikipedia user - CC-BY-SA-2.5 / Baycrest - 維基百科用戶 - CC-BY-SA-2.5" attrlink="https://commons.wikimedia.org/wiki/User:Baycrest" >}}

I'll soon be moving to a new role with my company, which has me leaving telco and making a step into the cloud. Although I have peripheral experience with the cloud (having taken advantage of the considerable free allowances that Azure and AWS provide to students), I've never played in the industry commercially in any facet, so it made sense to do a refresher. While I was taking notes, things started looking like a bit of an article, and thus here we are.

My themes are: **Diversification, Delineation, and Productization** (wish I could've found another ‘d’ word). I should note, these are really architectural trends in how we use the cloud, rather than market trends or external influences. If you're experienced in the cloud industry, nothing here will be new information. This is mainly oriented at newbies to the industry, like myself.

Previously, I'd looked down on the term ‘cloud’ as a overhyped marketing buzzword for putting your workloads off-prem, something that's been done since the dawn of networking. I think it's redefined itself for me to mean the greater concept of the flexibility and generalized abstraction away from traditional bare metal.

What is a ‘cloud’?
------------------

The cloud generally refers to all remote computing facilities in the world, in aggregate. You'll often see this definition used in media. A cloud is a single environment of remote compute capacity, most commonly delimited by who the owner is. Google has ‘a’ cloud, as does Amazon, as does all the local cloud providers in your country. If your business has remote off-site compute, even it has ‘a’ cloud.

There are several sub-categories:

*   Private cloud: A retronym that catches all traditional data centre _or_ internetworked on-prem set-ups, that meet a set of definitions to distinguish them from colocation. Private clouds have hardware that is owned or solely dedicated per client
*   Public cloud: An abstraction away from hardware where clients purchase quantities of compute/resources (similar to the old-school concept of timesharing) on hardware not owned by them
*   Hybrid cloud: A strategy where a client uses a mix of private and public cloud, to increase **diversification,** meet regulatory requirements, or create an realistic velocity of cloud migration
*   Multi-cloud: Intentionally using two or more clouds (when one would have suited fine) to provide **diversification** or take advantage of better pricing. Distinct from hybrid in that multi-cloud environments could be made up purely of public clouds, for example

So it seems that diversification is one of the key drivers for the more complex cloud architectures. How is this achieved?

Diversification
---------------

Unlike the consumer market, businesses with their more rational understanding of risk have a healthy fear of vendor lock-in. With the rapid rise of large public cloud providers, vendors like AWS looked to **productize** their offerings, providing solutions that add value, but in many cases increase vendor lock-in. I won't waste time talking about why vendor lock-in is bad, but instead I'll talk about the trends that help enable diversification in cloud environments. Vendor lock-in can be mitigated by:

1.  Ensuring your workloads are portable, **delineated**, and not vendor specific by using technologies such as [containerization](https://opencontainers.org/) or open virtualization standards ([LXC](https://linuxcontainers.org/) and vanilla Linux flavours instead of your public cloud's custom Linux distro - I'm looking at you [Amazon Linux](https://aws.amazon.com/amazon-linux-2/))
2.  Inversely, not using technologies that are highly vendor specific. One such example would be running business critical workloads using serverless functions - [typically much more difficult to port](https://www.scitepress.org/Papers/2020/95741/95741.pdf)
3.  Actively build a multi-cloud environment, with at least a contingency plan for porting workloads to your alternative's equivalent solution
4.  Build a hybrid-cloud environment with the option to scale your private cloud
5.  Use a new multi-cloud automation service such as [cast.ai](https://cast.ai/) or [Anthos](https://cloud.google.com/anthos) These are really a flavour of option 1, except **productizing** the actual implementation. They provide added value in that they also offer the ability to dynamically move workloads to optimise cost

{{< figure src="crh.jpg" caption="" attr="Baycrest - Wikipedia user - CC-BY-SA-2.5 / Baycrest - 維基百科用戶 - CC-BY-SA-2.5" attrlink="https://commons.wikimedia.org/wiki/User:Baycrest" >}}

Delineation
-----------

I must confess I chose this word because it started with D. The real word here is **containerized.** They essentially mean the same thing, it's just one has an additional meaning in computing. With a traditional monolithic app design, sometimes it's hard to see where to draw the line for specific components. Monolithic design means that your application must be developed, tested, packaged, and operated in one chunk. This makes it difficult to continuously iterate, and also nigh-impossible to split into discrete components.

##### Microservices

In recent years, the ‘[microservice](https://microservices.io/patterns/microservices.html)’ architecture of application design has come about. Microservice architecture is an umbrella term to describe application design that focuses on separating discrete components and loosely connecting them via internal standardised APIs (particularly RESTful ones). This allows individual components to be upgraded, modified, and improved independently, so long as the API standard is maintained.

If we have clearly distinct components that can communicate exclusively via XML/HTTP calls, suddenly our underlying OS, implementation language, etc. doesn't matter, and we don't need to maintain this across a project (although synergies can be found from a human resource perspective). 

Additionally, if we keep to RESTful standards in our API design, individual components become strongly delineated ephemeral black-boxes - that can be hosted anywhere, on any platform, via any implementation, provided you can make a fetch request to it. And because it's stateless and doesn't require contextual knowledge other than what's provided in the call, you can create as many copies of this component as is required to handle the load.

##### How does this manifest in the cloud?

This is what some call a “cloud-native app”. One that isn't fussy as to where it exists, so long as it can see the internet.

All this is supported by application containers, a newer trend that brings stronger delineation to appliances. With containerization technologies such as Docker, operating systems and program libraries that support our delineated components, now themselves are delineated and can move as one unit. A container for OS, libraries, and application.

Now, if you can run a Docker container, you can run your application on whatever platform offers this service. And this is exactly what public cloud services are offering, typically using the most popular flavour of orchestration, [Kubernetes](https://kubernetes.io/).

Kubernetes started with Google, and [this comic does a good job of explaining how it works](https://cloud.google.com/kubernetes-engine/kubernetes-comic/).

In short, Kubernetes allows you to write a simple manifest that defines the components of your application, and takes supplied Docker containers from your dev team, turning it into a full microservices application architecture entirely in Docker. Assuming you have the containers prepared, deploying on a cloud provider is absurdly fast through this method - and portable. Many public cloud providers offer Kubernetes platforms, and this enables you to move your workloads as you please, often within hours - how's that for diversification.

Productization
--------------

This is almost the direct counter to the above trend. Public cloud providers are doing their absolute best to keep users on their platform, and continue to make innovative (yet somewhat malicious) strides towards increasing vendor lock-in.

These techniques are getting interesting. For example, Google's [Anthos](https://cloud.google.com/anthos) is in-of-itself a product for _reducing_ vendor lock-in, but in the process, you lock yourself with Google via usage fees for Anthos. Additionally, the increase of specialized SaaS services provided by public cloud providers increase vendor lock-in, for example any of the Amazon machine learning products. Luckily, we have the alternative to use open-source solutions such as Tensor Flow, but Amazon will be doing all they can to entice people to their private standard.

This is something for decision makers to keep in mind. I can't blame people for choosing proprietary services where it makes sense, but it's good to be aware of the risks.

{{< figure src="openstack.png" caption="" attr="" attrlink="" >}}

### OpenStack: an interesting alternative

What if you liked the ease of use and tight integration of AWS, Azure, or Google Cloud, but, for some reason, didn't want to use any of those public clouds? Could you get that on-prem, or in colocation? Yep, [OpenStack](https://www.openstack.org/) is the answer.

OpenStack seeks to provide an open source alternative to the public cloud giants, and can give your organisation the large operational efficiencies and benefits of a cloud environment, even if you are limited (for regulatory reasons perhaps), to using private cloud. Interestingly, you can even run OpenStack _on_ public cloud providers, which could provide another way to diversify with a multi-cloud environment.