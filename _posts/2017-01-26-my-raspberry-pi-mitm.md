---
published: false
---
## Raspbery-pi mitm device
 With all the 'Russian hacking' that's going on. I decided to attempt to create a man-in-the-middle device that could be discreetly attached to a remote network and could redirect and sniff traffic.
  For those who don't know, a man-in-the-middle attack involves secretly becoming an intermediary between the communication between two parties; each thinks they are talking to the other when in fact they are both talking to the attacker. The attacker can choose to pass the information along unmodified(simply observing the communication) or may choose to modify parts of the communication for their own nefarious ends.
   One of the counter measures of mitm is the use of SSL/TLS to the verify the other party in a communication.TLS however relies on a public key infrastructure, and there have already been examples of hackers breaking into certificate authorities and issuing fraudulent certificates so as to perform man-in-the-middle attacks on HTTPS sessions
 I'll provide updates on this interesting project.This project will only perform mitm attacks on HTTP traffic.

<img src ="/imgs/">
<meta content="http://carsor007.github.com//_posts/" property="og:image">