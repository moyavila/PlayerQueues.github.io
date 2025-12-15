---
layout: post
title: Introduction
author: Moises Avila
date: 2025-12-15
---
# Monte Carlo Simulation

#### This research project's focus is to explore the workings of Monte Carlo Simulations.
#### Monte Carlo Simulation is a method that utilizes repeated random sampling to model and evaluate complex systems. 

An important theorem used in Monte Carlo Simulations is the Law of Large Numbers.

Law of Large Numbers states that as the number of observations increases, it reaches a point where the average sample relatively equals the mean. 
This is important for Monte Carlo Simulations because large amounts of random repeated trials are simulated, therefore with this theorem there is more control over the estimates and the outcome is more accurate. The image below represents this idea. 

![Monte Carlo Simulation Example]({{ "/assets/images/lawoflarge.png" | relative_url }})

## Queueing Systems
Utilizing the logic of Monte Carlo Simulations, real-world systems can be replicated such as Queueing Systems.

Before diving deeper into queueing systems, it is important to define and understand queues. 
A queue is a sequence or line of individuals or object awaiting their turn to be processed. The different types of sequences individuals are served in a queue are 'First Come, First Serve', 'Last Come, First Serve', Service in Random Order, and Priority Classes of Customers. 

Understanding the functionality of a queue, a queueing systems is a model used to analyze the process of a queue.
This model is written with Kendall's Notation, which describes the distribution of the arrival process, service process, and amount of servers. Additionally, there are distribution codes for the arrival and service process. 
These codes are 
- Markovian (M): utilizes the exponential distribution for inter-arrivals and service
- Deterministic Distribution (D): uses a fixed variable
- General Probability Distribution (G): utilizes any type of distribution.

A queueing system helps calculate the performance of a queue by calculating:
- Utilization Parameter: proportion of time the server is busy
- Sojourn Time: the average cycle time that it takes to move through the entire system
- Waiting Time: the average time that the customer has to wait before being served
- Average number of customers waiting to be served
- Average number of customers in the system

Below is an example of a Markovian/Markovian/1 Server (M/M/1) Queue. It is evident that the Monte Carlo Simulation is an integral part of a queueing system because as the number of customers in the queue increases, the simulated values becomes almost equal to the theoretical value.
