---
title: "Serverless Architectures"
date: 2017-12-19T14:45:05+01:00
---

In the last few months, one of my main topics to learn about has been Serverless. After this time
I think that, like some other trends in software, there is still confusion about what Serverless is, so
I would like to share my reflexions about it to:

* Try to help and get feedback to/from people reading this article.
* As probably I have not fully understood some concepts yet, I will come back here periodically in order to see my progress understanding it :P

### Why Serverless?

Before discussing what Serverless is, I think it is important to mention **what people who use serverless are willing to achieve**.

* **Reduce operational burden**: Serverless removes a lot of operational burdens from your team. You do not have to manage operating systems or running low level infrastructure. Instead, you are using a third party service to do it.

* **Time to market**: In my opinion, this is one of the most important benefits of using serverless. The entire time from a new idea to the initial deployment of a elastic platform with metrics, automatic scaling and costs control can be very short and with a low risk and cost.

* **Cost**: A resource is only running when it is necessary so the correspondence between billed and used is very high.

### What is Serverless?

Before going deeper into this topic, almost all the times that I heard something about Serverless it was related to AWS Lambda, but **Serverless is not just AWS Lambda**. AWS Lambda, like other services such as Azure Functions or Google Cloud Functions, is an implementation of Function as a Service (FaaS). FaaS is an important component within the Serverless architecture, where you put your code. But the most important change that is accelerating the adoption of Serverless is the power of the ecosystem of services in each public cloud. Without this ecosystem (services like SQS, API Gateway, S3, DynamoDB, Kinesis, etc), AWS Lambda would not have gained as much adoption as it did in this short period of time.

So, **when an application is Serverless?**

For an application to be Serverless, it needs to move towards these directions:

* **Event-triggered**: the code that you put in FaaS services like AWS Lambda is triggered by events. For instance, in AWS you can choose between different triggers like include a file in S3, add a message to a bus (Kinesis), schedule a periodic task, etc.

* **Fully managed by a third party**: as an application is increasingly going to Serverless, it relies more on external services to be able to provide the features of the system. For instance, you have to use services like DynamoDB if you want to persist data, services like Kinesis or S3 as we pointed above, etc.

* **Epheremeral compute**: this is a continuation of the ideas exposed by [The Twelve-Factor App](https://12factor.net/), where we see guidelines like _Twelve-factor processes are stateless and share-nothing. Any data that needs to persist must be stored in a stateful backing service, typically a database_ and [Immutable Infrastructure](https://blog.codeship.com/immutable-infrastructure/). Serverless goes beyond that, so **the infrastructure should not even exist when it is not handling data**

As we have seen, the infrastructure in Serverless applications does not exist when there is no data to handle (_**A persistent computing resource is a server no matter what form that resource takes**_). When data is available (there is an event that trigger our application), all the components have to be running with a low latency in order to give a proper response. Here, we have a proof of the power of this ecosystem.

### Downsides

We have seen some reasons to go Serverless and we explained the main concepts behind this architecture. Now, we are going to mention some trade-offs.

* **Cloud Provider lock-in**: as we have said above, when your application go to Serverless, it relies on many external services. Usually you use a service to get a specific feature. Sometimes, that feature is better implemented in a service from another cloud provider, and maybe you want to jump to it. This has a cost, because you will have to update some things. In order to reduce this problem, there are frameworks like [Serverless](https://serverless.com/) which offer a general abstraction of multiple providers.

* **Magic**: with a few lines of code, you can have a platform with continuos deployment and automatic scaling. If you use frameworks like [Serverless](https://serverless.com/), you will be able to deploy that infrastructure with a single command in different providers. This sounds very good, but you have to understand what there is under this magic. Otherwise, you will lose the control of your platform and this can be very dangerous when the platform is increasingly growing.


#### References

* [Serverless: Looking Back to see Forward](https://m.subbu.org/serverless-looking-back-to-see-forward-74dd1a02cb62)

* [The Serverless Spectrum](https://read.acloud.guru/the-serverless-spectrum-147b02cb2292)

* [Debunking Serverless Myths](https://read.acloud.guru/debunking-serverless-myths-cc329460d061)
