---
title: "Help! My computer doesn't boot!"
date: 2021-02-24T13:46:50+08:00
draft: false
author: "Daryl Galvez" 
description: "PC troubleshooting after CompTIA A+"
slug: "" 
tags: [CompTIA, A+, IT]
categories: []
externalLink: ""
series: []
---

As some of you might have known, I just passed my CompTIA A+ certification exam. That exam basically says that I am competent in a variety of IT troubleshooting and help desk skills for both software and hardware. Just a few days after obtaining that certification, my skills were immediately tested in real life.

I have used CompTIA's 6 steps in troubleshooting methodology to help me in my troubleshooting and guide my train of thought.

## Identify the Problem

My girlfriend actually is a dentist and she has this really expensive X ray machine called a CBCT. I got a message from her telling me that they have a problem with their CBCT. It is connected to a PC workstation where all the scans from that CBCT is fed, read, and printed. She told me that the workstation cannot start. They really need that workstation up and running, otherwise her clinic won't be able to service patients' needs and will set them back thousands pesos everyday that the workstation stays down. 

## Establish a theory of probable cause

I went to the clinic in the morning and tried to boot up the workstation. The fans start indicating that power is being supplied to the workstation. After a while, the fans stop spinning and the PC fails to boot. This looks like a symptom of failure to POST.

### What is POST?

POST (Power On Self Test) is a set of procedures performed by software or firmware that a device runs through immediately each time it is turned on. This is a part of the device's pre-boot sequence. If it is successful, the device then loads the operating system, otherwise the device will fail to boot.

POST will ensure that all hardware is working properly before trying to load any operating system. With this in mind, we already have an initial idea that the problem might be hardware related.

## Test the theory to determine the cause + Establish plan of action to resolve the problem and implement the solution

I went through the following steps in troubleshooting the issue. Let me guide you to my though process.

1. New Hardware

Newly installed hardware can cause POST to fail. This could mean that the new hardware installed is defective, a setting needs to be changed for the hardware to work or a firmware needs to be installed, or if the hardware itself is incompatible with your system (rare, but it can happen). Since there were no new hardware installed in the CBCT workstation, we ruled this one out.

2. External disks or USB devices

Any external devices such as USB or harddrives could interfere with your device's POST as well. Check if the issue gets resolved by removing those devices then try booting your computer again. There were not external devices connected so we ruled this one out as well.

3. External devices and peripherals

Since POST failure is caused by hardware failure, by removing any external devices except the power and booting the computer back up, we can check if the device can successfully POST. This was not the issue with our situation. POST still failed even with external devices removed.

4. Cabling

Bad cabling can also raise false hardware failure. I have double checked the cables' conditions and tightness in the ports. All good there but POST still failed.

5. Motherboard components

This was the last thing that I checked. My next step was to remove the motherboard components one by one and check if the POST will succeed. I started with removing the RAM DIMM. I immediately noticed that it was a bit loose. I removed it completely and then cleaned the dimm contacts carefully. Afterwards, I reinserted the DIMM in the slot firmly and turned on the workstation. 

## Verify full system functionality (and if possible, implement)

Voila! It worked! The workstation was able to boot up and CBCT functionality was tested. We turn off the workstation and turned it on again (to make sure that it boots again and that the previous boot was not a fluke). Afterwards, we took a couple of scans and X rays. 

My girlfriend was really happy and her stress was relieved. She can continue providing CBCT services to their patients.

## Document findings, actions, and outcomes

This is my way of documenting what I have learned in my CompTIA A+ studies and what I have done to apply those, specially in the troubleshooting side. 

## Final thoughts

I was really happy with myself because I was able to apply my newly acquired knowledge and skills in the real world, actually saving my girlfriend's dental clinic from an unnecessary downtime. 

I hope you learned a lot from this as well :)


#### References

[POST Troubleshooting Steps](https://www.computerhope.com/issues/ch000607.htm)  
[Professor Messer](https://www.professormesser.com/)  
[Jason Dion](https://www.jasondion.com/)  
[Mike Meyers](https://www.totalsem.com/)  
