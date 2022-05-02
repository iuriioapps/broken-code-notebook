# Service Granularity

#### Granularity Disintegrators

Granularity disintegrators provide guidance and justification for when to break a service into smaller parts. While the justification for breaking up service may involve only a single driver, in most cases the justification will be based on multiple drivers.

{% hint style="info" %}
* **Service scope and function**: is the service doing too many unrelated things?
* **Code volatility**: Are changes isolated to only one part of the service?
* **Scalability and throughput**: Do parts of the service need to scale differently?
* **Fault tolerance**: Are there errors that cause critical functions to fail within the service?
* **Security**: Do some parts of the service need higher security levels than others?
* **Extensibility**: Is the service always expanding to add new contexts?
{% endhint %}

#### Granularity integrators

{% hint style="info" %}
* **Database transactions**: Is an ACID transaction required between separate services?
* **Workflow and choreography**: Do services need to talk to one another?&#x20;
* **Shared code**: Do services need to share code among one another?
* **Database relationship**: Although service can be broken apart, can the data it uses be broken apart as well?
{% endhint %}
