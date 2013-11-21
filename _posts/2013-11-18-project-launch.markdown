---
layout: post
title:  "Project Launch - Integration Platform As A Service (iPAAS)"
date:   2013-11-20 21:31:25
categories: projects
tags: paas aws
published: true
excerpt_short: Launch of FME Cloud
author: Christoph Klocker
blog_image: fmecloud-blog-banner.jpg
---

Last wednesday, the project I was working on for the last 6 months finally went live. It was not
just any other project, but we have launched a so called iPAAS (Integration Platform AS A Service). I was the responsible Software / Infrastructure
Architect and the main developer of the Rails stack. At the same time I had the opportunity to introduce the agile methodologies I've been using over the last two years,
to the company through this project.

Together, in an awesome team of 4 we brought the iPAAS to life.

Definition of iPAAS by Gartner:
> *Integration Platform as a Service (iPaaS)* is a suite of cloud services enabling development, execution and governance of
> integration flows connecting any combination of on premises and cloud-based processes, services, applications and data within
> individual or across multiple organizations.

The aim of this project was to bring the spatial integration server *<a href="http://www.safe.com/fme" target="_blank">FME</a>*,
that normally is installed at a customers site, into the cloud. The customer
should be able to log into a web site, select the desired instance size, FME server version, data storage size and then launch the
fully configured server with one click. The server should be billed by the minute and it should be possible to pause and
terminate a server at any time. All of this should also be possible through an API.

We chose to build the stack on the Amazon Cloud (AWS), as it is the most versatile cloud provider out there. Looking back
at more than 5 years deploying software on AWS, it is also my preferred choice.

A quick walkthrough on what AWS services we use and why

#### EC2
You probably have guessed it right, we need server of different sizes (Linux for obvious reasons)

#### EBS
The root partition runs of an EBS and we have a separate one with all data, that we can easily
replicate and backup.

#### Route 53
Each provisioned server needs a dns name to access it, Route53 makes it easy and is scriptable.

#### S3
We use the Simple Storage Service for all kind of things.

#### Dynamo DB
At the start we used Dynamo as a central data storage between the Rails stack and the provisioned
servers, we then switched to SNS and webhooks and don't use it anymore.

#### SNS
We use SNS to publish server state changes to the webhook in the Rails stack.

#### CloudFormation
Plug all of above together

If you want to know more, check out the *<a href="http://www.youtube.com/watch?v=h631J_gDcTs" target="_blank">Unveiling FME Cloud</a>* webinar.
