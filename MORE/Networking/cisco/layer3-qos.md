# Layer3 QoS
There's QoS and then there's QoS.  In an ideal world, QoS would be more along a reservation of services, where the flow would be guaranteed a specific level of service.  This was how things were meant to be setup with ATM.  In the IP world, there really is no good way to guarantee, so instead we either have enough bandwidth, so we do nothing, or when things get tight, we back off some flows for the sake of others. 

- [QoS Theory and Review](qos-theory-and-review.md): A discussion on the different kinds of traffic characteristics and how QoS come in to play.  Also discussing the different ways QoS can be used, and how it works.
- [Basic QoS at Layer 3](basic-qos-at-layer-3.md): How to deploy QoS on Cisco Routers.