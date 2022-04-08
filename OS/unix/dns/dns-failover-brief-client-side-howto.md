# DNS Failover: Brief Client Side Howto

## Overview
A hosted service that is accessible via the internet can manage its failover procedures unobtrusively to the client two basic ways. One is through the IP number to name resolution protocol [DNS](https://en.wikipedia.org/wiki/Domain_Name_System), and the other is through the internet routing protocol [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol).

This document will look at the clients requirements for using a internet service that uses DNS as its failover technique. Its goal is to enable the client to utilize the failover site as soon as possible.

## What is DNS
In its most basic implementation, computers within the internet communicate with each other by addressing their numerical IP address, like 216.109.104.11 for example. This method alone can become rather clumsy, as you must somehow remember what IP address a specific host has. DNS (Domain Name System) is a way that allow computers to address another computer using a name or names rather then only using a number. It allows us to name our host or web server something that makes sense to us, and allows us to remember it better. For example, I might have a web server that runs an important service for a company called Ariba. Rather then always having to remember the numerical IP address of 216.109.104.11, you can instead just remember the name service.ariba.com.

The translation between the names and numbers is handled locally by each organization. Thus I would manage my computers IP number to name translations, and if you would want to resolve that the name to one of my hosts, you would speak to my name server.

## How DNS Failover Works
As mentioned above, when you would like to talk to my computer (for example, my webserver), you would need to resolve the name to a number first. You would do this by first contacting my name server, and ask it what the IP address is for my host (say, service.ariba.com). My name server would then tell you its IP address (for example 216.109.104.11), and then you would connect to that address directly.

If my webservers break, I can always just change the name servers to resolve the names to different ip address, and have those computers in a different location. So in a disaster, when you ask my name server the address for service.ariba.com, it might then tell you a different address, like 216.109.108.11.

## What you need to do to have DNS failover work for you
There are a few hooks to this solution, and they require the client to make sure that they, and their organization follow a few simple rules.

First, you need to make sure that you address hosts by their name and not by their number. Some locations feel that they have a little extra control and security by not using host names and instead only using hosts numerical IP address.

Secondly, you will want your name server to cache a learned resolution for the specified amount of time suggested by the authoritative name server. When you first want to resolve my hosts name, you ask your local DNS name server. It will normally then ask me for the IP name to number resolution, and then tell you what it learned. Afterward, it will also remember that information, and if anyone else asks it will just tell them what it already knew, rather then re-asking my server. The length of time that your DNS server remembers my translation is suggested by my name server. You would need to honor that timeout period, and not remember the translation longer. Not heeding this, will result in a longer time for you to switch over to the new servers in case of a fail over.

