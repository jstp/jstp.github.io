
JSTP: Into the wild
===================

JSTP is an ideal: what if you could use the same protocol, a simple standard, to communicate in your application accross all devices, accross every layer of the stack and even seamlessly use it locally inside an instance of the application? What if that protocol was expressive enough to provide a clear abstraction of all the common resource operation, while at the same time being asynchronous, symmetrical and event-driven?

What if you could design your entire architecture around API calls described in a single, high level protocol that provides descriptive power but also easiness of implementation and easiness of communication? What if that protocol would be human-readable, and based on current state-of-the-art conventions so that even casual developers would be able to jump in the bandwagon almost without effort?

JSTP is that ideal and the attempt at giving the ideal a reality check. And a reference implementation.

The road so far
---------------

JSTP started out as a necessity: my team was working on the development on what lately came to be [Tiempy](tiempy.com) and we experimenting with using WebSockets to stream tweets coming from the Twitter API to the user. Of course, that could be solved by just throwing the tweets, JSON encoded, as WebSocket messages for the browser to parse, but we realized soon enough that provided the full-duplex capabilities of WebSockets, our initial routing strategy for the API calls (ie: HTTP) was getting obsolete. In order to avoid confusion in the incoming and outgoing messages sent via WebSockets, we decided to encapsulate them in a lightweight protocol—and since we were web developers and quite familiar with REST, we decided to make that protocol REST-inspired. So JSTP was born.

While working with a real-time API such as the Twitter Streaming API we realized that an architecture where the web server that provided the core functionality (registration, sessions, user account configuration, which depended on a static, classical stack) was merged with the streaming server was suboptimal. In fact, the streaming server was prone to errors, and in early development, to more unexpected situations such as memory leaks, unknown encodings, unexpected messages or even HDDs getting depleted. This feature  clearly had to be isolated for security, stability and testability reasons. 

So our inner stack began to grow.

We tried several known solutions, such as messages queues (ZMQ, Kestrel), real-time architectures (Storm) and toyed with the idea of several layers of HTTP APIs, but none proved satisfactory. First of all, there existed what I like to call a "paradigm shock": most message queue technologies focus on enterprise solutions where the sysadmin and the developer are very different people, and the main language is either C++ or Java. We had no sysadmin at all at that stage and deploying some of the solutions proved to be unnecesarily challenging. Second, while both Storm and Kestrel delivered in performance and stability, they are very un-developer friendly solutions, provide little documentation and not very good logs. 

So we returned to our then newborn JSTP and started to wonder. JSON is a widely supported format with blazing-fast native implementations in most architectures. As for the transport protocol, in implementations outside the browser we could be using vanilla TCP: that would be fast, right? Also, our REST-like API already proved to be useful on the web app, what if...?

And yes, it worked. It worked for us even when it was just a barely defined idea, and the more we refined it, the faster we turned to JSTP as a tool for that and the next projects we faced as a team. In the end, it just makes sense.

So what's the fuzz about
------------------------

Let's see a "Hello World" message in JSTP: 

```
{
  "protocol": ["JSTP", "0.6"],
  "method": "POST",
  "resource": ["message"],
  "body": {
    "content": "Well... Hello World!"
  },
  "timestamp": 1382810529592,
  "token": [],
  "to": [],
  "from": ["sample"]
}
```

This JSTP message (called a JSTP Dispatch, because "dispatch" is a rare enough word to be used as a technicality) may describe the action of posting the "Well... Hello World!" message into some sort of guestbook. We like to describe JSTP messages in an abbreviated form similar to that used with HTTP. This message can be thus described as follows:

```
POST message JSTP/0.6
from: sample

content: Well... Hello World!
```

This message (this Dispatch) is to be *triggered* locally, that is, within the current application. Most protocols describe messages to send over a network: JSTP is actually more of an extensible event manager with the ability to bind events remotely. That means that, unless the Dispatch specifies a destinatary in the `to` Header, the Dispatch will not be sent over any network and instead will be fired locally.

So imagine we want to capture Dispatches such as that described above in order to actually save the message in a text file. JSTP describes a powerful Subscription feature for capturing Dispatches; you have to issue a special JSTP Dispatch, called a Subscription Dispatch:

```
{
  "protocol": ["JSTP", "0.6"],
  "method": "BIND",
  "endpoint": {
    "method": "POST",
    "resource": ["message"]
  },
  "timestamp": 1382812279731,
  "token": ["3v3245141423dfa324", "ewr453fwq5436rsgwa"],
  "to": [],
  "from": ["sampleResource"]
}
```

...which in turn can be abbreviated as:

```
BIND POST message JSTP/0.6
from: sampleResource
token: 3v3245141423dfa324/ewr453fwq5436rsgwa
```

The JSTP *Engine* (that is, the JSTP event manager) is instructed to treat Dispatches that have the BIND method as special Dispatches that initialize subscriptions to events as described in the `"endpoint"` header. 
