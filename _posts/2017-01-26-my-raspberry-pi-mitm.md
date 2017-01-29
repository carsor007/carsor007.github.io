
---
layout: post
type: post
tags:
  - Raspberry-Pi-3
  - SOC
  - MicroSD
  - Internet of Things
published: true
title: man in the middle device
---

## Raspbery-pi mitm device
Haven't posted in a while due to a busy schedule. CUrrently working on an interesting idea that I will hopefully share before the end of this year. 
  After looking through different pi projects and what people have built, I have decided to attempt to create a man-in-the-middle device that could be discreetly attached to a remote network and could redirect and sniff traffic.
  For those who don't know, a man-in-the-middle attack involves secretly becoming an intermediary between the communication between two parties; each thinks they are talking to the other when in fact they are both talking to the attacker. The attacker can choose to pass the information along unmodified(simply observing the communication) or may choose to modify parts of the communication for their own nefarious ends.
   
   One of the counter measures of mitm is the use of SSL/TLS to the verify the other party in a communication.TLS however relies on a public key infrastructure, and there have already been examples of hackers breaking into certificate authorities and issuing fraudulent certificates so as to perform man-in-the-middle attacks on HTTPS sessions
 I'll provide updates on this interesting project.This project will first perform mitm attacks on HTTP traffic, then we can add more features to it and make it more advanced...

<img src ="/imgs/">
<meta content="http://carsor007.github.com//_posts/" property="og:image">
