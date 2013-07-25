A Real Time Protocol
====================

HTTP is not a real time protocol. When you want to receive many messages fast, for example when streaming content from Twitter or Facebook, you don't want to ask every 5 seconds for an update—you want to get them when they are there. For HTTP to allow you to get many messages in time, you have to fall back to all kind of strategies to work around HTTP limitations, and that’s because, in fact, HTTP was not designed for that. You have to resort, for example, to hang response connections and send messages in different times. Developers spend more time going around restrictions of the tool that creating new experiencies. That has to change.

HTTP is also not real time because when you ask for some data to a remote service, you have to wait until the remote service processes the whole of the data before continuing. This in time motivates developers to send data in larger and larger bulks, burdening processing even more. You can't just "send and forget". Even with asynchronous HTTP, such as with XHR (Ajax), when you send a package, you have to wait.

HTTP is not real time, lastly, because in its default setting you have to create new connections each time you want to load a resource. Browsers have become very efficients when it comes to handle connections, make the most of them as possible, but as a developer you have no control over effective connection reutilization, and for sure server-side HTTP implementations vary wildly in their network usage optimization.

Through REST, HTTP provides a wonderful interface to query databases and resource access frameworks (such as recently many web APIs). REST is a marvelous practice. Still, HTTP is slow: so slow in fact that in practice it oftenly tends out to be an suboptimal solution.

JSTP is, actually, real time.

JSTP is a protocol we started developed several month ago and that we use to communicate the nodes across out stack.

We needed an asynchronous protocol that can take load, that is fast and allows for easy scalation.

We started by using Websockets for browser connections, and then TCP sockets between nodes (server being no longer an accurate description).

JSTP is an asynchronous protocol: this means that for each package sent to the receiver, the receiver can send back any amount of packages. The receiver can also distribute the packages in the time as better suited.

JSTP messages are JSON. 

JSTP is fast because connections are never closed. It is fast because JSON is easy to parse and packages are by design small.

Since first developed, we implemented several ports of common tools to interact with them in plain JSTP. For example, we implemented Monje, a binding for MongoDB, and Pajaro, a binding for Twitter. Both packages are available for Node.js.

Using this ports allows us to run a single instance of a Pajaro streaming provider running on a virtual machine and query the streaming data from several different modules of the Twitter application. Since the interface is standard, we can add Pajaro streaming providers whenever we need them.

In fact, we are currently drafting the specification for JSTP Engines to be able to report saturation status so that new hosts can automatically be added as needed.

## Semantics

JSTP needed a semantics, that is, a way to describe packages what you want the receiver to do with them. We decided to use the REST semantics, to maintain a familiar conceptualization of network communication and also to lower the entrance barrier to get familiar with the protocol. JSTP is almost 100% compatible with REST, and if you are a web developer chances are its workings is readily comprensible to you.

JSTP has an extra feature over REST that makes a substantial part of its advatages: the mechanism to subscribe locally and remotely to events being fired in JSTP Engines, so that certain callbacks get executed upon triggering of the event. This allows emmitters to send a package just once, generally when a service is being requested, and receive data packages through time, whenever they are processed.