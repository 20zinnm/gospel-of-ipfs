# IPFS in the Browser

[TOC]

## Abstract

This document specifies a series of problems in the current web infrastructure and ways that the [InterPlanetary File System](ipfs.io) (IPFS) can help resolve them, should it be adopted by browsers.

<!-- Probably a little more? Or merge with the introduction? -->

## Introduction

IPFS is best described in the abstract of the white paper that introduces it as a protocol:[^1]

> The InterPlanetary File System (IPFS) is a peer-to-peer distributed file system that seeks to connect all computing devices with the same system of files. In some ways, IPFS is similar to the Web, but IPFS could be seen as a single BitTorrent swarm, exchanging objects within one Git repository. In other words, IPFS provides a high throughput content-addressed block storage model, with content addressed hyper links. This forms a generalized Merkle DAG, a data structure upon which one can build versioned file systems, blockchains, and even a Permanent Web. IPFS combines a distributed hashtable, an incentivized block exchange, and a self-certifying namespace. IPFS has no single point of failure, and nodes do not need to trust each other.

<!-- Core concepts? -->

## Caching and Availability

Given that companies like CloudFlare are increasingly vital components of the internet, often fending off Distributed Denial Of Service attacks that otherwise would have "broke[n] the internet," it is no secret that caching is a crucial component of the distributed web.[^2] However, the current internet infrastructure does not reflect the growing demand for content; instead, it remains dependent on several old technologies that have been patched to "Just Work" in the face of increased strain. One example of such technology is content delivery networks (CDN), which aim to spread the burden of distributing data across a large number of "edge" servers in closer proximity to consumers, thereby decreasing the latency a consumer experiences. However, the costs to maintain such a network are significantly related to the location of consumers and the frequency of cache invalidation.

IPFS solves this problem by changing the entire paradigm of how data is located. Traditionally, data is addressed by a uniform resource locator and then located. There are several pitfalls to this approached, covered in more depth in [permanence](#permanence). IPFS uses a technique known as content-addressing, whereby data is identified by its cryptographic hash. To understand this, consider a metaphor.

Imagine that one of your friends wanted to recommend a book to you. HTTP is equivalent to him or her telling you:

> First, go to the people who know where stuff is. Ask them how to go to the building 175 North End Avenue, New York, NY. I don't care if that building is across the world, go there and walk ten steps in. Bend down to the right and three books from the left is the book you should read.

There are so many things that could go wrong here; first of all, what if you live in Sweden? It's entirely possible the book could be in your local library, but here you are, traveling halfway across the world for a book you might not even enjoy. Also, is that place a library or a bookstore? You don't know if you're supposed to pay for it! Finally, what happens if a librarian accidentally puts the book one position to the right? Then you've travelled from Sweden to New York and are returning empty-handed.

IPFS is equivalent to your friend telling you:

> You should get the book "Go in Action" with ISBN-13 978-1617291784.

Since you know what you are looking for, it's entirely possible your neighbors own a copy of the book and you could get it from them. Also, some sleemo from Half Priced Books can't try to sell you a different book because you have a way to determine whether or not the copy you receive is legitemate. It doesn't matter if the place your friend got his or her copy from is still on the face of the Earth, because you can get it from anywhere.

Applying this to the internet, consider one of the most popular Javascript libraries of all time: jQuery. Your browser likely has a cached copy of jQuery because of how popular it is. However, caches are specific to download sources; for example, if one website uses Google's CDN to load jQuery, and another website uses MaxCDN, your browser is contractually obligated to fetch two copies of the same thing because there are no guarentees that the copies are identical. Whether or not your browser keeps both copies is irrelevant, as the bottleneck is generally networking. Additionally, your browser is forced to retrieve this copy of jQuery from a remote source, be it a CDN edge server or some site on shared, free hosting, when it is entirely possible that another computer on your local network has a copy of the file that it could share with you.

<!-- Need to talk more about the benefits of caching. -->

## Permanence

<!-- Walk through all of the steps that could go wrong in HTTP that would result in no website. Walk through the steps of IPFS to show that the only way a file is lost is if, across the entire network, there is not a complete set of blocks. However, you can partially construct files and if someone comes on later with the rest, you can get it from them. -->

The process begins with a recursive DNS lookup that ideally results in an Internet Protocol (IP) address of a single machine to contact in order to retrieve the desired content. The client is then responsible for contacting that machine and requesting the resource; generally, the client has no way of verifying that it receives what it requests beyond confirming that the request is not tampered with. Thus, the accessibility of any given resource at a URL is entirely depenent on whoever controls any part of the process; for example, United States federal datasets, sized in the petabytes, are currently available for anybody to access. However, at the sole discretion of a federal employee

## Chunking

<!-- Talk about all of the great things about chunking--deduping, multiple sources, changes yield similar DAGs, etc -->

[^1]: https://github.com/ipfs/ipfs/blob/master/papers/ipfs-cap2pfs/ipfs-p2p-file-system.pdf?raw=true
[^2]: https://www.ericsson.com/thinkingahead/the-networked-society-blog/2013/09/18/the-future-of-content-caching/