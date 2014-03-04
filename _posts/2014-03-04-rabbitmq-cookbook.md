---
layout: post
title:  "RabbitMQ Cookbook"
date:   2014-03-04 22:00:00
categories: 
---

If you have to design a distributed system, messaging pattern is you best friend.
[RabbitMQ](http://www.rabbitmq.com) is the best solution for message passing between application modules, for several reasons.

RabbitMQ is a messaging broker, which uses a standard protocol: AMQP.
It's very fast, a single RabbitMQ server can handle very huge messages load, and if it's not enough for you, you can scale horizontally by adding more broker instances.

There are a lot of libraries for various languages: Erlang, Ruby, Java, Python, C, C# and [so on](http://www.rabbitmq.com/devtools.html). So connecting different systems, coded in different languages is very easy.

Also RabbitMQ has a lot of interesting features for dispatching messages, like dead-lettering, alternate-exchange, mirrored queues and federation between geographicaly separated nodes.

To learn something more about there is a book written by Sigismondo Boschi and Gabriele Santomaggio: [RabbitMQ Cookbook](http://www.packtpub.com/rabbitmq-cookbook/book). As the name _cookbook_ says, it's a set of recipes to quickly start using RabbitMQ. Understanding how it works by using it.

Starting with simple message publishing and consuming, going to how to handle High Availability on broker failures. It's useful also to learn same good practices when using RabbitMQ. This book covers also the deploy process, for example an interesting use case on AWS to achieve load balancing and dynamic grow of broker instances to handle load spikes.

Interesting are also usages from mobile clients, like iOS or Android apps. Which can bring a good _realtime_ experience in your apps, for games or social networks. They are also explained, RabbitMQ it's compatible also with lightweight protocols like MQTT, which is suitable for embedded devices.