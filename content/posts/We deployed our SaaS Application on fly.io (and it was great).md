---
title: We deployed our SaaS Application on fly.io (and it was great)
subtitle: How we deployed our application in a fraction of the time while saving 100% of the cost.
date: 2024-10-23
toc: false
language: de
---

Our team, a bunch of experienced software engineers without prior contact to cloud deployments, wanted to deploy our OCPP-compliant EV Charging Station Simulator (EVCSS) publicly, so that early users and other stakeholders can easily test it.

Sounds simple enough, right? That's what we thought. And optimistically pulled the “Cloud Deployment” Issue in our current Sprint because we had a couple of additional days to work on it.

## Let's make it a size M, just to be sure…

On our first introduction to AWS, we were greeted with an admin panel, a considerable selection of services and found ourselves right in the middle of choice paralysis. What do we actually need? It was difficult to find the best or even any way to deploy our simple client server application. So we enlisted a specialist, someone with prior experience with AWS and cloud deployments, who may shed some light on the path we couldn't see in the haze of three-letter acronyms and similar-sounding services (is that what S3 stands for?).

## When in Doubt, Ask the Cloud Whisperer

And we spent a couple of hours communicating our needs and discussing the tradeoffs of one service vs. the other. We drew boxes and arrows to try to visualize how our future deployment would look and behave. All the while, a thought crept up in the back of my mind: Does it have to be so complicated?

We don't have complicated needs. We just want our static single-page application to be available to our stakeholders. Bonus: A public address we can direct potential users to. So why did we end up with around 10 services that interconnect in intricate ways that no one, apart from the cloud engineer, knew anything about? Is AWS really the best choice if the result is a highly complex and likely expensive infrastructure, just so we can invite a couple of people to test our app?

## Enter fly.io

I have the deep-seated belief that good products and services meet their users where they are. As a cloud novice, I did not see AWS meeting me where I am. So I tried something different. I remembered hearing about fly.io on [a podcast](https://podcasts.apple.com/de/podcast/xe-iaso-on-fly-io/id120906714) and visited their site. Their tagline:

> A Public Cloud Built For Developers Who Ship
> ...
> Deploy your App in 5 minutes.

Hey, that's me! And that's precisely what I had in mind when estimating our cloud deployment story. So how to get started? Well, with the Getting Started docs, of course! And as I was looking for a quick start to find out whether this could replace our plans for AWS, I started with the [Quickstart](https://fly.io/docs/getting-started/launch/) guide.

## Only 4 steps until take-off

1. Install the `flyctl` command-line tool
2. Create an account using `fly auth signup`
3. Configure your deployment using `fly launch` from inside your project
4. Deploy the application using `fly deploy`

That was everything required to deploy our application on fly.io. My mind was blown. Could it really be that easy?

## Can you hear me?

It turns out that `flyctl` was able to detect our client and server application and found our docker container, but wasn't able to configure the WebSocket connection between client and server correctly. Also, we didn't have a health-endpoint yet to validate our server is indeed up and running. So after a bit of reading through their documentation (which is a highly rewarding experience, as far as reading documentation goes) and implementing the necessary endpoint to validate our app is deployed correctly, everything was working as it should. And the best part, it was not even lunchtime yet.

## Finally, Bliss.

The deployment took less than 4 hours and working through the documentation was actually enjoyable. I always felt like I was on the right track, something I never experienced in the brief encounter with AWS and its offerings. Meanwhile, my colleagues bit the bullet and powered through the deployment on AWS. So we now have two instances of our application. One is basically free (as fly.io doesn't charge for deployments that stay below 5 Euros per month), the other one needed approval from Finance, as the accrued monthly cost was already above 100 Euros without any users actually visiting it yet.

I'm sure our AWS deployment is not really cost-optimal yet. Also, I acknowledge that AWS is the more mature and flexible platform. There are certainly reasons why they are the dominant player (and have been for quite some time). *But when I'm building my next application and want to show it to potential users as early as possible in the development lifecycle, I will definitely go with fly.io again.*
