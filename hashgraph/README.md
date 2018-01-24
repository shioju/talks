Hashgraph
===

## The Future of Decentralized Technology

<style>
.container {
  display: flex;
  justify-content: space-around;
}
.text {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding-left: 100px;
}
.pic {
  height: 700px;
}
strong {
  color: maroon;
}
</style>

---

# Byzantine Generals Problem

- A commanding general must send an order to his $n-1$ lieutenant generals such that
  - All loyal lieutenants obey the same order
  - If the commanding general is loyal, then every loyal lieutenant obeys the order he sends

![40%](images/bgp1.png)![40%](images/bgp2.png)

---

# Byzantine Generals Problem

- distributed systems in real life
  - need to deal with failure or conflicting behaviours of its components
  - need consensus on the distributed state
- fun fact: *Lamport: "I have long felt that, because it was posed as a cute problem about philosophers seated around a table, Dijkstra's dining philosopher's problem received much more attention than it deserves."*

---

# Asnchronous Byzantine Fault Tolerance (BFT)

- What does Byzantine mean?
  - will reach consensus
  - know when consensus reached
  - consensus never changes
- Assumptions
  - attacker controls < 1/3 (theoretical limit)
  - attacker controls the network
    - messages between honest nodes eventually get through
    - asynchronous BFT -> no assumptions on the timing

---

# Can Bitcoin tolerate 1/3 attacker nodes?

![70% center](images/attack.png)


---

# Distributed Consensus Algorithms

- Proof-of-work (Bitcoin, Ethereum)
  - slow (10 transactions per second)
  - waste of energy ($1M/day ~ Mauritius energy consumption)
- Proof-of-stake (Ethereum Casper)
  - much lower energy consumption
- Leader-based (Hyperledger Fabric)
  - Paxos, Raft, PBFT
- Voting-based (no implementation)
  - excellent theoretical properties
  - high bandwidth requirements $O(n^2)$

---

# What is hashgraph?

Hashgraph is a data structure and consensus algorithm that is:

- Fast: With a very high throughput and low consensus latency
- Secure: Asynchronous Byzantine fault tolerant
- Fair: Fairness of access, ordering, and timestamps

These properties enable new decentralized applications such as a stock market, improved collaborative applications, real-time multiplayer games, and auctions.

---

![](images/hashgraph.gif)

---

# Interesting characteristics

- absolute confirmation of transactions (unlike proof of work)
- order of transactions preserved (compare with proof of work where transaction order is determined by miners)
- no wasted computation (compare with blockchain forking)
- just gossip and everything will work (low overhead)
- really fast virtual voting (no additional comms for consensus)

---

![center](images/blockchain-and-hashgraph.jpeg)

---

# The consensus algorithm

Refer to [Hashgraph graphical example](graphical.pdf)

---

<div class="container">
<img class="pic" src="images/01-initial.png">
</div>

---

<div class="container">
<img class="pic" src="images/02-gossip.png">
<div class="text">
<p>Randomly pick someone to gossip with to create <strong>events</strong></p>
<img src="images/03-gossip-internals.png">
</div>
</div>

---

<div class="container">
<img class="pic" src="images/06-hashgraph.png">
</div>

---

<div class="container">
<img class="pic" src="images/08-witnesses.png">
<div class="text">
<p>Events are divided into <strong>rounds created</strong></p>
<p>First event from each member in each round are <strong>witnesses</strong></p>
</div>
</div>

---
### voting on <strong>fame</strong> by next round witnesses (<strong>seeing</strong>)
<div class="container">
<img class="pic" src="images/10-fame1.png">
<img class="pic" src="images/11-fame2.png">
<img class="pic" src="images/12-fame3.png">
<img class="pic" src="images/13-fame4.png">
</div>

---
### vote counting by next round witnesses (<strong>strongly seeing</strong>)
<div class="container">
<img class="pic" src="images/14-strongly-seeing1.png">
<img class="pic" src="images/15-strongly-seeing2.png">
<img class="pic" src="images/16-strongly-seeing3.png">
<img class="pic" src="images/17-strongly-seeing4.png">
</div>

---

<div class="container">
<img class="pic" src="images/18-b2-famous.png">
<div class="text">
<p>B2 is decided to be <strong>famous</strong></p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/19-c2-votes.png">
<div class="text">
<p>Of all the round 3 witnesses, only C3 can see C2</p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/20-c2-count-votes.png">
<div class="text">
<p>B4 can strongly see all 4 votes, and decides that C2 is not famous</p>
<p>But assuming B4 wasn't able to strongly see a <strong>supermajority</strong> of the same votes, it will fail to decide the result, and will vote based on the majority of its collected votes, and leave it to other witnesses to decide (potentially from future rounds).</p>
<p></p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/21-decide-more-fames.png">
<div class="text">
<p>Similarly, elections are conducted for the other witnesses as well</p>
<p>Once the fame of all witnesses in a round is decided, we can use that to find the consensus ordering of another set of events</p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/22-round-received.png">
<div class="text">
<p>Black event seen by all famous witnesses, so round received of 2</p>
<p>Doesn't matter that C2 can't see it, as C2 is not famous</p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/23-not-received-yet.png">
<div class="text">
<p>For events not seen by all famous witnesses, wait for the next round</p>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/24-consensus.png">
<div class="text">
<p>For Consensus order, sort by:</p>
<ol>
<li>Round received</li>
<li>Consensus timestamp (median of first received timings by creators of famous witnesses)
<li>Whitened signature (signature XOR with signatures of all famous witnesses)
</ol>
</div>
</div>

---

<div class="container">
<img class="pic" src="images/25-demo-screenshot.png">
<div class="text">
<p>The example was adapted from an actual run with 4 members</p>
</div>
</div>

---

# Intuition of how hashgraph works

- order transactions by the time a majority of the nodes learns about it (thru the gossips)
  - must be 2/3 or more, received by 50% is not good enough when 1/3 are attackers
- famous witnesses are like well-known and trusted events, and simplifies the computation
- strongly seeing protects against forking, so no attacker can cheat by creating multiple famous witnesses in a single round

---

# References and recommended reading

The Byzantine Generals Problem
http://research.microsoft.com/users/lamport/pubs/pubs.html#byz

How Hashgraph Works (Graphically)
http://www.swirlds.com/downloads/SWIRLDS-TR-2016-02.pdf

Hashgraph security and attack resilience
https://www.youtube.com/watch?v=pcToFASnyrc

Deconfusing Decentralization
https://youtu.be/7S1IqaSLrq8

Hashgraph introduction at TechCrunch Disrupt
https://youtu.be/ZrFrXFdRW4k

Leemon Baird x Havard Talk
https://youtu.be/IjQkag6VOo0

---

# References and recommended reading

Beginner's Guide to Ethereum Casper Hardfork
https://blockonomi.com/ethereum-casper/

Ethereum Proof of Stake FAQ
https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ

A (Short) Guide to Blockchain Consensus Protocols
https://www.coindesk.com/short-guide-blockchain-consensus-protocols/

Proof of Activity: Extending Bitcoin's Proof of Work via Proof of Stake
https://eprint.iacr.org/2014/452.pdf