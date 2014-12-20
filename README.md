norproject
==========

Home of the Nor Project (Not Tor Project)

The Nor Project is an effort to create a light-weight but compatible fork of Tor that is an alternative to the centralized directory services of the Tor Project.

This project is intended to plug into the existing tor ecosystem and to supplement and strengthen Tor, not to replace it.

Why?  On December 19th 2014 the Tor Project blog announced the possibility of a seizure occuring against the primary directory servers of the Tor network.  These directory servers are a crucial piece of infrastructure and unfortunately there are only a handful of them.  https://blog.torproject.org/blog/possible-upcoming-attempts-disable-tor-network

We at the Nor Project believe that the particular services provided by the directory authority servers might be better served by a fully decentralized solution.

What are the options?

Crypto currencies...?
Currently there is a coin that serves as a DNS, called namecoin.  While our initial intent was to provide a "plugin" or bridge between namecoin and Tor, this was decided against.  The reasoning is that the directory servers are not really there to keep the list of .onion domains.  Instead their primary purpose is to provide a list of relays and serve in the process of helping tor clients learn the list of relays that comprise the tor network.  This list is constantly in a state of flux.  It would be problematic to maintain it on the namecoin blockchain since that blockchain is not intended for tracking rapidly changing information.

Another alternative might be to attempt to encode the information directly into the bitcoin blockchain via a series of small sends.  That solution was found to be non-viable because the size of the chain is significant.

Yet another proposal would be to create a new coin on it's own block chain.  This proposal would not viable with current cryptocoin tech because an attacker could easily gain 51% hash power on the nascient chain, thereby taking it over.

The final nail may be that nearly any blockchain based solution is going to fail because by definition it must contain the complete list of relay nodes from the time it was created. Knowing all the nodes gives a powerful tool to bad actors.  With the complete list, they can paralyze the entire network by attacking the relay nodes.

Our proposed solution is different.  
There is no Directory Authority and no blockchain required. 
It is designed to be lightweight, fast, cryptographically secure and able to withstand the assault of bad actors who may have the full backing and power of bad-actor nation-states who may want to bring it down.

1. Each time a relay node joins the network, it creates a one time use "access token" per channel.  In this case a channel means fixed amount of bandwidth for a specified amount of time with limitations and privileges spelled out such as exit/no exit etc.  It represents a guarantee and a contract.  We call this an offer token.
2. The offer token is then broadcast to a specific channel on bitmessage.
3. This token circulates for up to an hour before expiring out.
4. When a new node joins tor it picks an offer token out of the token stream and sends an encrypted connection request back through bitmessage. This is called an "acceptance token".
5. The relay then responds with details on how to connect.
6. Once the request is accepted then the node can use the relay in circuit building, the regular tor process takes over at this point.

There are some obvious flaws in this approach such as how to track known good vs bad relays and how to handle rating.  It is not a complete solution

(the following parts are optional, but built into the protocol)

There is also an opportunity here to provide a sort of "fast lane" through tor, by compensating relay operators for providing bandwidth, by giving them a method of acquiring more bandwidth.

This is where blockchain tech comes in.  The Nor Project will feature a blockchain based crypto currency, but with a twist.  This twist is called "Proof of Bandwidth".  Each time a relay has an invite token utilized, it has the option of requesting the connecting node to rate the throughput provided and sign an acknowledgement.  
To sign the acknowledgment, the node will be required to spend a previously created "accept" token, thereby passing the right to that bandwidth onto the relay which can use it to purchase additional bandwidth from other nodes for its own circuits.  Optionally it can be kept by the relay operator as a source of revenue.

These tokens are essentially the same thing as coins in other crypto currencies the plan is to keep chain, by having it merge mine with bitcoin.  Merge mining with bitcoin will provide enough hashing power to make it all but impossible for someone to perform a 51% attack against the currency.

The final piece of the puzzle is to create a plugin for popular bitcoin mining software that will enable existing miners to contribute their excess bandwidth to the project by becoming relays themselves.

Flaws and solutions?

One flaw is how to handle self dealing, a relay pretending to also be the node that used the token, in order to create more tokens.  A token does not create bandwidth, it only creates a claim to existing bandwidth.  Tokens are created by coinbase transactions achieved during the mining cycle.  It is an offer and accept chain with new coins being created only when a new block is created.  

The difficulty level will be tied to the total available bandwidth as indicated by the block chain.  

Another flaw is how to handle grief nodes.  A node cannot be forced to sign and there is no way to punish a node that refuses.  There is also no way to verify QoS from the relay.  Our solution is that the accept token should contain a provisional signature to prove the node owns the source token and this is broadcast over bitmessage.  If there is no corresponding "rated" signature within 2 hours, the source token is considered destroyed and not usable by either party.

To keep things simple, there will be 21 Million coins total with a block generation rate of 50 coins per block and a new block every 10 minutes and a halving period of 52560 blocks or ~1 year.
