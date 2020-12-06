# Well-Architected Framework

* Well-architected framework is a set of principles.
* These principles are documented as 5 pillars:
  * Operational Excellence
  * Security
  * Cost Optimization
  * Reliability
  * Performance Efficiency

### General Design Principles

* **Stop guessing capacity needs** - scale up and down as required
* **Automate everything** - automated systems ensure consistency and reliability.
* **Test at scale** - test an accurate replica of production on-demand.
* **Adapt and evolve** - adapt the architecture as needed to meet new challenges.
* **Be data driven** - drive decisions through data.
* **Game days** - practice, practice, practice. 

### The Five Pillars

[https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/](https://aws.amazon.com/blogs/apn/the-5-pillars-of-the-aws-well-architected-framework/)

* **Operational Excellence**
  * Does your architecture work? Will it continue to work?
  * There are six design principles for operational excellence in the cloud:
    * Perform operations as code
    * Annotate documentation
    * Make frequent, small, reversible changes
    * Refine operations procedures frequently
    * Anticipate failure
    * Learn from all operational failures \(and success\)
  * Prioritize to align with business priorities
    * What is the business goal?
    * What are the critical pieces needed to meet that goal?
    * Any compliance restrictions/requirements?
    * Dependencies between services?
  * Design your architecture to support business priorities
    * Is the design observable?
    * Is the entire design code? Can it be redeployed in even of a failure?
    * Are your logs and observations actionable? Can you derive values from data you're collecting?
  * Is your workload ready to go live
    * Are your processes consistent?
    * Is operational code properly managed?
    * Are tests in place?
    * Are you anticipating failure?
  * Ensure your workloads are actually working
    * Metrics indicate health of each service
    * Metrics show overall health
    * Are you monitoring business metrics too?
  * Responding to events
    * Anticipate planned and unplanned events
    * Respond in code
    * Connect observations with 3rd party tools as needed
  * Learn from success or failure

    * Post-event, have runbooks changed?
    * Are teams evaluating their processes?
    * Test assumptions
    * Experiment early and often to find better solutions
* **Cost Optimization**
  * Spend only what you have to. Deliver business value for the lowest price point.
  * There are five design principles for cost optimization in the cloud:
    * Adopt a consumption model
    * Measure overall efficiency
    * Stop spending money on data center operations
    * Analyze and attribute expenditure
    * Use managed services to reduce cost of ownership
  * Use the appropriate resources and configurations
    * Provision for current needs with an eye to the future
    * "Right size" to lowest resource that meets the needs
    * Use data to choose purchase options
    * Optimize by geography
    * Default to managed services
    * Optimize data transfer
  * Matching supply and demand
  * Know how much you're spending and where
    * Understand your stakeholders
    * Implement a governance model
    * Attribute cost to teams/projects
    * Tag AWS resources
    * Track lifecycle of the resources
  * Continuously work to maximize value delivered

    * Align utilization with requirements
    * Report and validate findings
    * Evaluate new services for value
    * Continue push for managed services, if they're cost-effective
* **Reliability**
  * There are five design principles for reliability in the cloud:
    * Test recovery procedures
    * Automatically recover from failure
    * Scale horizontally to increase aggregate system availability
    * Stop guessing capacity, reduce idle resources
    * Manage change in automation
  * Will this system work consistently and recover quickly
    * Recover from issues automatically
    * Scale horizontally first for resiliency
    * Reduce idle resources
    * Manage change through automation
  * Understand the default and requested limits
    * Are you planning beyond current limits for a resource?
    * Will you scale past specific resource limits?
    * Can those limits be lifted?
    * Can you plan around those limits?
  * Networking
    * IP address space management \(are you considering IPv6\)
    * Subnets structures
    * Resilient topologies
    * Ability to handle sudden increase in traffic
    * Provide consistent performance regardless \(latency\)
  * Ensure your application is ready for business use

    * Can users access your application?
    * Deploy without an issue
    * Can you push issue to a planned downtime?
    * Can your application withstand partial outages?
* **Performance Efficiency**
  * There are five design principles for performance efficiency in the cloud:
    * Democratize advanced technologies
    * Go global in minutes
    * Use serverless architectures
    * Experiment more often
    * Mechanical sympathy
  * Remove bottlenecks, reduce waste
    * Let AWS do the work whenever possible
    * Reduce latency through regions and AWS Edge
    * Serverless whenever possible, then containers, only then fall down to instances
    * Experiment as new services are released
    * Think about the user, not your tech stack
  * Is this the optimal solution for this workload
    * What type of compute best suits?
    * Which data store is ideal for this workload?
    * Does your network design complement compute and data store choices?
  * Continuously ensure choices work for your workloads
    * Is infrastructure stored as code?
    * Are deployments simple and automated?
    * Can benchmarks be taken automatically?
    * Does load testing interfere with production?
  * Monitoring

    * Use active and passive monitoring where appropriate
    * Understand the 5 phases of monitoring - generation, aggregation, real-time processing, storage, analytics
    * Create actionable metrics
* **Security**
  * There are six design principles for security in the cloud:
    * Implement a strong identity foundation
    * Enable traceability
    * Apply security at all layers
    * Automate security best practices
    * Protect data in transit and at rest
    * Prepare for security events
  * Does this system work only as intended?
    * Identities have the least privileges required
    * Know who did what and when
    * Security is woven into the fabric of the system
    * Automate security tasks
    * Encrypt all data at rest and in transit
    * Prepare for the worst
  * Look for abnormal behavior in your logs
    * Capture and analyze logs
    * Regularly audit controls and configurations \(AWS CloudFormation drift, AWS Config\)
  * Defense in depth
    * Establish trust boundaries
    * Protect the network in/out
    * Protect all hosts
    * Configure services to meet security posture needs
    * Enforce service level protection
  * Classify and protect data
    * How sensitive is the data?
    * Who should have access to the data and when?
    * Encrypt in transit and at rest
    * Backup your data, test backups
  * Contain and recover from an unplanned event
    * Do you have a plan to tag affected resources?
    * Can you adjust permissions to allow for containment?
    * Can you redeploy to recover quickly?
    * Did you learn from the incident and adjust?

