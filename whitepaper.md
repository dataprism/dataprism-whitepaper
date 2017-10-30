# Dataprism

## Abstract

This paper describes dataprism, a platform implementing the kappa architecture by leveraging apache Kafka and other opensource technologies. Special care is taken to making the platform easy to maintain and extend.

## Introduction

Back at the beginning of the bigdata hype, all processing was focussed around processing large batches of data using rather low-level technologies like map-reduce. Additional processing technologies like pig and cascading were developed to make processing applications easier to comprehend and more productive to develop, but it remained batch processing. 

Things changed with the introduction of the lambda architecture, providing a seemingly straightforward way of processing data not only in batch, but also in realtime. Through a combination of layers data was processed in batch while at the same time new data not included in the batch was being captured through a speedy processing layer. By combining both the results of the batch and speed calculations, a eventual-accurate informational view could be presented.

While Lambda was certainly an eye-opener for a lot of people, it suffered from a very high level of complexity. Since the structure of batch processing applications is significantly different from realtime processing, it meant processing had to written twice, often even in different technologies.

Kappa provided a simplification by dropping the batch processing and allowing all processing to happen through the realtime processing algorithms. Generated views could be reprocessed by replaying the raw events, but that posed some major operational challenges in order to keep the system consistent.

Dataprism implements the Kappa architecture and proposes best practices to extend the base architecture in order get the most out realtime data processing.

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

Ingesting data is the process of accepting data into the system. Data can be ingested from a veriaty of different systems and technologies. For example, the data could be provided through an api which has to be polled at a specific interval, or the ingestion layer can provide its own API for other system to send data to. Apart from API access, Services busses and relational data stores are quite common within organisations so there should also be a means to extract data from these technologies as well.

Due to the veriaty as well as the different accessing paradigms (pull/push) throughout the data providing systems, the ingestion part rapidly increases in complexity. It becomes a very tedious job to control and operate all the different connectors to the systems from which data is to retrieved.

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

### Serving Data

If all goes well, the RAW data is being converted into queriable data.

### Operating

### Replaying

### Snapshotting



## High-Level Solution

## Details

## Business Value

## Summary

## CTA