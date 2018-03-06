# Dataprism

## Abstract

This paper describes dataprism, a platform implementing the kappa architecture by leveraging Apache Kafka and other opensource technologies. Special care is taken to making the platform easy to maintain and extend.

## Introduction

Organisations are constantly at a quest to improve themselves and to get closer to their users or customers. Analyzing data has proven to be a very powerfull way of doing so, but while during the last decade analysis focussed on data produced by the organisation's business processes, today combining internal with external data appears to be the way to go to.

The data landscape within an organisation is very fragmented. Multiple tools collect, process and visualize insights for different purposes. While these services certainly have their value, it becomes hard to track which data is stored where and for which purpose, not only for the operators and engineers, but also for scientists who want to use that data to try and find the hidden treasures. Furthermore, the rate at which a data fragment loses value has increased a lot, requirering near-realtime insights.

Event-sourcing architectures like the kappa architecture are a perfect match to deal with high-velocity data and provide scalability as well as human fault-tolerance. Due to their nature however, technologies included in these architectures are complex and tedious to maintain.

Dataprism is an effort to take such an event-sourcing architecture to the next level by implementing the architecture and taking away the tedious work related to keeping such an architecture up and running.

## History

Back at the beginning of the big data hype, all processing was focussed around processing large batches of data using rather low-level technologies like map-reduce. Additional processing technologies like Pig and Cascading were developed to make processing applications easier to comprehend and more productive to develop, but it remained batch processing. 

Things changed with the introduction of the lambda architecture, providing a seemingly straightforward way of processing data not only in batch, but also in realtime. Through a combination of layers data was processed in batch while at the same time new data not included in the batch was being captured through a speedy processing layer. By combining both the results of the batch and speed calculations, a eventual-accurate informational view could be presented.

While Lambda was certainly an eye-opener for a lot of people, it suffered from a very high level of complexity. Since the structure of batch processing applications is significantly different from realtime processing, it meant processing had to written twice, often even in different technologies.

Kappa provided a simplification by dropping the batch processing and allowing all processing to happen through the realtime processing algorithms. Generated views could be reprocessed by replaying the raw events, but that posed some major operational challenges in order to keep the system consistent.

Dataprism implements the Kappa architecture and proposes best practices to extend the base architecture in order get the most out of realtime data processing.

## Problem Description

Solutions based on distributed technologies are hard to develop, but are even harder to maintain and to operate. The Kappa architecture introduces a few more procedures to keep track of when dealing with a distributed data processing solution, making operating even more complex.

In general the following parts are known to a event-driven data processing system

- Ingesting
- Processing
- Serving
- Operating

Each of these parts have their own set of problems to deal with, as described later in this document. On top  of that, Kappa also introduces some additional parts:

- Replaying
- Snapshotting

As we will see later on, these parts add more requirements on top of the system.

### Ingesting Data

Ingesting data is the process of accepting data into the system. Data can be ingested from a variety of different systems and technologies. For example, the data could be provided through an API which has to be polled at a specific interval, or the ingestion layer can provide its own API for other system to send data to. Apart from API access, Services busses and relational data stores are quite common within organisations so there should also be a means to extract data from these technologies as well.

Due to the variety as well as the different accessing paradigms (pull/push) throughout the data providing systems, the ingestion part rapidly increases in complexity. It becomes a very tedious job to control and operate all the different connectors to the systems from which data is to retrieved.

Even for a single connector, the ability should exist to scale the number of jobs gathering or retrieving the data from the external data systems, not only for performance reasons, but also to be resilient to failure. 

Aside from the diversity of data providing technologies, one of the major difficulties in building scalable distributed systems is the notion of time. Time becomes even more important when building event-sourcing-like systems where the sequence of the messages is driven by their timestamp.

### Processing Data

Once data starts flowing through the ingestion part it is required to start processing these crude data elements into something of value. This usually means pushing the data element through a chain of programs to continuously improve the value of the data. 

While this is pretty straight forward to explain, it is truely a maintenance nightmare and the main source of struggles when developing a data pipeline. Best-practices from Software Engineering are required to build a development street where the different applications are being developed, tested, accepted and put into production. This is the part where the manual effort takes place and data pipeline developers come to implement whatever logic is needed to convert data into insights.

A lot of things are coming together here and raise some important new questions:

- How do we release new versions of our logic?
- To what extend do we accept downtime when doing so?
- What are the quality metrics we will be metering on?
- Is monitoring enough to prevent things from going wrong?
- How do we scale in case of performance issues?
- What is the impact of a specific service not running?

These, and probably a lot more questions all need to be answered to build a robust processing component.

### Serving Information

While the data processing is meant to convert the RAW data into information, something needs to be done with that information to make it available to applications and end-users. Often this means storing that information inside a datastore of some sort allowing end-users and applications to make connections to these datastores and query the information in it.

However, the structure of the data often depends on the type of data store being used. Storing information in a relational database (RDBMS) for example requires the information to be structured in a totally different way than if we would be storing it in a document store.

### Operating

Operating a stack of technologies is always a challenging task due to the complexities of making all technologies work together. Distributed stacks like the ones used within the bigdata ecosystem are even more complex, for a number of reasons. For example, latencies between the nodes on which the technologies are running as well as individual node failures increases the complexity level of the stack significantly.

Monitoring and alerts are required to get a grasp on what is happening within the stack and across all the nodes of the system. But that raises another question; What exactly do we need to track and monitor? Which metrics matter in which kind of situation?

### Replaying

An important aspect of a Kappa based architecture is the ability to recalculate the information since it gives the architecture the capability to recover from human errors. But while the idea behind it is rather simple, the implementation most certainly is not.

Should you keep the "invalid" information available for as long as the replaying is going on, or is it allowed to just drop the information and start from a clean slate? Do you replay everything, or can we replay only portions?

All these questions add up to a very interesting problem space.

### Snapshotting

As explained during processing, the RAW data is being stored in order to be able to replay and reprocess. However since that RAW data is an ever-growing collection of data, the amount of time it takes to replay the RAW data will keep on rizing as well.

One way of keeping the reprocess durations manageable is to perform snapshots of the raw data using a moving aggregated baseline 

## High-Level Solution

## Details

## Business Value

## Summary

## CTA
