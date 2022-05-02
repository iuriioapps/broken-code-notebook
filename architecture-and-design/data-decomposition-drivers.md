# Data Decomposition Drivers

Breaking apart a monolytic database can be a daunting task, and as such it is important to understand if (and when) a database should be decomposed.

Architects can justify a data decomposition effort by understanding and analyzing data disintegrators (drivers that justify breaking apart data) and data integrators (drivers that justify keeping the data together). Striving for a balance between these two forces and analysing the trade-offs of each is the key to getting the data granularity right.

#### Data Disintegrators

Data disintegration drivers provide answers and justifications for the question "when should I consider breaking apart my data?". The main drivers area as follows:

{% hint style="info" %}
* **Change control**: How many services are impacted by a database change?
* **Connection management**: Can my database handle the connection needed from multiple distributed services?
* **Scalability**: Can the database scale to meet the demands of the services accessing it?
* **Fault tolerance**: How many services are impacted by a database crash or maintenance downtime?
* **Architectural quanta**: Is a single shared database forcing me into an undesirable single architecture quantum?
* **Database type optimization**: Can I optimize my data by using multiple database types?
{% endhint %}

#### Data Integrators

Data integrators do the opposite of the data disintegrators. These drivers provide answers and justifications for the question: "when should I consider putting data back together". Along with data disintegrators, they provide the balance and trade-offs for analyzing when to break apart data and when not to.

The two main integration drivers for putting data back together are the following:

{% hint style="info" %}
* **Data relationship**: Are there foreign keys, triggers, or views that form a close relationship between the tables?
* **Database transactions**: Is a single transactional unit of work necessary to ensure data integrity and consistency?
{% endhint %}
