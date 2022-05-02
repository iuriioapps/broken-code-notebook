# Consumer-driven Contracts

A common problem in microservices architectures is the seemingly contradictory goals of loose coupling yet contract fidelity. One innovative approach that utilizes advances in software architecture and development is a consumer-driven contract.

In many architecture integration scenarios, a service decides what information to emit to other integration partners (a push model - the service provider pushes a contract to consumers). The concept of consumer-driven contract inverses that relationship into a pull model; here the consumer puts together a contract for the items they need from the provider, and passes the contract to the provider, who includes it in their build and keeps the contract test green at all times.

Advantages:

* Allow loose contract coupling between services
* Allow variability in strictness
* Evolvable

Disadvantages:

* Require engineering maturity
* Two interlocking mechanisms than one
