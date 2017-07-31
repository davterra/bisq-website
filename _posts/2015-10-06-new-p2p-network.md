---
layout: post
title: New P2P network
author: Manfred Karrer
banner: null
---
<p>Some of you might ask what causes the delay of the beta release which was planned for that summer.<br/>
  The reason for the delay is a change in a fundamental part of the application &#8211; <strong>the P2P network</strong>.
</p>
<p>
  <em>The following might be a bit technical. If you are not interested in those details you can skip to the last paragraph as well.</em>
</p>
<h3>DHT</h3>
<p>Bitsquare used <a href="http://tomp2p.net/">TomP2P</a> which is the most mature Java
  <a href="https://en.wikipedia.org/wiki/Distributed_hash_table">DHT</a> implementation available and I was lucky to get the author &#8211;
  <a href="http://www.csg.uzh.ch/staff/bocek.html">Thomas Bocek</a> &#8211; on board to help to fix the open issues with
  <a href="https://en.wikipedia.org/wiki/NAT_traversal">Nat traversal</a>.<br/>
  Those issues was the source of a constant concern since the project start as for any P2P network it is a big challenge to overcome the restrictions set up by NATs and firewalls to avoid that nodes are accessible from other nodes in the internet.
</p>
<h3>Tor</h3>
<p>
  <a href="https://github.com/JesusMcCloud">Bernd Prünster</a> introduced another idea to me which turned out to not only solve the NAT problematic but also helps to mitigate other open issues I had with a DHT solution: Using a Tor proxy to delegate the network traffic over
  <a href="https://www.torproject.org/">Tor</a> and therefore delegate the NAT problematic to Tor, which has solved that to a very satisfying level (they even pass through Chinas great firewall).
</p>
<p>But no worry the Bitsquare user don&#8217;t need to do anything. It is all integrated into the application and no special setup is required. There are also no performance drawbacks with the small amount of data sent by Bisq.</p>
<p>Using Tor not only solved the network connectivity issues but also adds the high level of anonymity Tor provides to Bitsquare. In fact we use Tors
  <a href="https://www.torproject.org/docs/hidden-services.html.en">Hidden Services</a> for every node to make the P2P communication completely anonymous (<a href="https://www.torproject.org/docs/faq.html.en#AttacksOnOnionRouting">as far Tor provides that</a>).<br/>
  That solved another open issue with the previous solution: The offerer need to be able to get contacted by a taker and therefore leaked his IP address when publishing an offer.<br/>
  Now there are no IP addresses used but onion addresses. Those cannot be used to reveal the real location or identity of the user and that previous privacy issue is therefore solved.
</p>
<h3>Flooding network</h3>
<p>The network routing algorithm used to transport the data (offers) previously stored in the DHT to all users is now a
  <a href="https://en.wikipedia.org/wiki/Flooding_%28computer_networking%29">flooding</a> (or gossiping) algorithm. A similar one is used in Bitcoin to provide a very robust P2P network with less vulnerabilities as a structured network like a DHT.
</p>
<p>More sophisticated and effective routing algorithms like
  <a href="https://en.wikipedia.org/wiki/Kademlia">Kademlia routing</a> which is used in DHTs come with a serious Sybil attack risk, as anyone who can control certain nodes could control the storage of certain data. The problem is that the network ID creation is free and the network ID is used to derive the storage location. So  you can create a huge amount of netwok IDs and then select those which are giving you the control over the data storage location of the data you want to control.<br/>
  That vulnerability is mitigated with the flooding algorithm as every node stores everything.</p>
<p>Satoshi has chosen the flooding algorithm for Bitcoins P2P network to obtain a highly decentralized and randomized network structure which is very important to secure the network against hostile takeover of parts of the network.<br/>
  Though it came with some costs regarding resource usage. As we know, every full node in the Bitcoin network has to store 50 GB of blockchain data.<br/>
  Luckily Bitsquare uses very small amount of short living data and the number of nodes will be much smaller as well. I estimate there will be data storage requirements of a few hundreds of Kilobytes or a few Megabytes. Each data has an expiration date, so our requirements will not cause any scalability problems.
</p>
<p>Additionally to the publicly readable data like the offers there are data stored which need to remain private. There are trade process messages which are stored in a kind of mailbox in case the peer is offline. Those data are encrypted and signed and also sent to every node for storage. Only the receiver (who has the private key) can decrypt the data. A similar approach is used in
  <a href="https://bitmessage.org/">Bitmessage</a>.</p>
<h3>Current state</h3>
<p>Of course <strong>building a custom P2P network</strong> is a task which
  <strong>needs time and caution</strong>. That&#8217;s what causes the delay in the
  <a href="/roadmap/">roadmap</a> to release the Beta version.<br/>
  The new network is basically already implemented in the application but it is not completed yet.</p>
<p>I hope that we can release the Beta version in about 2-6 weeks.</p>
