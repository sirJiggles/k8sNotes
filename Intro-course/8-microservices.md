# Micro-services

Benefit of k8s really shows off with these. They are an application arch, build your app around small independent services that work together to form the entire application.

As apposed to the mono application, all pieces parts and features all combined to one large executable.

In a micro service things that can live by themselves are things like

- auth
- customer data
- search x 3 (notice the amount of replicas 🔥)
- product data x 2

They can be written in any tech that matches the need and you can make many replicas of the service. You can scale up parts of your application without scaling up all of it. With a mono application you cannot do that. You also get cleaner code by design. The more decoupled it is the cleaner it will tend to be. you are forced into this which is awesome. Reliability is also a key advantage. if there is an issue, there is less chance it will effect the entire application.

Finally you can use a wider variety of tools, languages, architectures and so on. you get the right tool for the job.
