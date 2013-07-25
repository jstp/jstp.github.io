Implementation names are a form of art. We come from Argentina, so we named all of our implementations of JSTP in spanish. The name relates to what the implementation does, or the name of the technology: for example "pajaro", bird in spanish, is our implementation of the Twitter API, and "monje" that is a contraction of Mon-goDB and J-STP. "monje" means monk.

The most complete implementation of JSTP is developed in JavaScript, either for Node.js and for the browser.

One of the best qualities of JSTP is that the browser uses the exact same protocol as the rest of the system. If you want the HTTP server to communicate to a database, you can do that with JSTP. If you want the browser to comunicate asynchronously with the server, you can do that with JSTP. Even if you want the browser can direclty call the database, you can do that using JSTP (of course, it is not recommendable for security reasons).

We use JSTP in everithing we do. We are continuously developing more and more implementations as the time goes by. Beyond the implementation for Node.js, JSTP is a specification, because we want to build it as an open standard. In fact the specification of JSTP can be found in a public repository in github.com. All the stuff regarding JSTP is public in github; you can see it, beter yet you can submit issues, better yet you can fork it and send a pull request, better yet you can create your own implementation of JSTP for the platform or technology you want.

Using JSTP it is really easy to update content in real time. In apps for Android or iOS, or for web, everything can be synchronized in real time. Making it so with JSTP is quite simple. That was our goal from the very beginning, when we first came up with the idea of JSTP.

Since JSTP is symmetric, its potential for scalation is humongous. This means that each and every node of JSTP can be set to act as a client and a server at the same time. Almost every message of JSTP is symmetrical. That makes possible to distribute packages in a big grid of hosts, and to be able to do it asynchronously, without having to wait for an anwser.

We aim to make JSTP able to communicate with almost everything. Databases, APIs of social networks, robots in Mars and even your bluetooth coffemaker. You get the idea… anything.

The specification draft of JSTP has matured substantially. We managed to make it develop up to our expectations. After having used JSTP for a reasonable time we had been the chance to polish many details. Now we are starting a new milestone: we are going to make JSTP able to serve as the backbone for the stack of any aplication (distributed or otherwise), and give the application a way to scalate organically. To reach that goal we are currently working in this key features:

## Status reporting
1. Each engine of JSTP will be able to repost the state of saturation of the server in wich is running. For example, if a JSTP engine works as a database, it migth report to the aplications that are using the database that there is little space left in disk. And if the computer reaches a critical point, the idea is that another instance of the database is added to the sistem, and the applications can balance autmatticaly between both. The metrics that the engines will be able to report are free space in disk, memory available in ram, saturation of the cpu and networking consumption.

## Security
2. An extention of security for JSTP that works with a publick key. We want to acheive that every JSTP engine could send secure messages that could only be decrypted by the true recipient, no matter how many engines has gone trough. Besides we are going to make possible to identify the aouthor of a message to avoid that it could be forged by other user in an open infrastructure.

## Streaming extensions


---

## The future

Over a mature JSTP, we want to develop several solutions that we dream will make a radically different experience of web usage for everyone, everyday.

JSTP has the potential to make hosts communicate meaningful with each other is ways that have limitedly been exploited still. For example, hosts nowadays are only limitedly able to communicate with resources they have to offer, and their respective saturation levels. While many cloud stacks, such as Amazon AWS and Rackspace, offer automatic scalability bundled with their services, this is an after-the-fact addition in their platforms and not a native capability of the hosts. In fact, several "subjective" saturation metrics are left unreported and unaccounted for because they are not measurable by the providers.

We've drawn some inspiration from a seemingly sci-fi proposal of the [Ambient Cloud](http://highscalability.com/blog/2009/12/16/building-super-scalable-systems-blade-runner-meets-autonomic.html). The article describes a Cloud of hosts that is able to integrate any available connected device, with a geographical feature that makes the scalable from as local as a LAN to the whole Internet. The article is truly inspirational.

We have also drawn inspiration from the distributed nature of the BitCoin network, specially the skill to make secure cryptographic transactions without a central authority. And we though, why not?

We want to make a service for providing application stacks in JSTP where you can graphically manage your stack, adding and removing nodes as you please, choosing for an extensible portfolio of application services—or even providing them youself. 

Just imagine. You enter the website and you log in, may be using your github account. Once logged in, you enter the free tier plan and you start setting up your hosts. You drag a Monje host so to be able to use MongoDB.


---

We want to hear about you. We want you to get involved in this, which we believe will become something big. We need the community: two developers are not enough for such an ambicious task. To make application development accessible for more and more developers, to simplify the distributed stacks and make them more efficient. We aim high. Will you join us?