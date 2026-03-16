# Cloud Fundamentals

## Cloud computing concepts
- Cloud computing delivers computing services—including servers, storage, databases, networking, software, analytics, and intelligence—over the internet ("the cloud") to offer faster innovation, flexible resources, and economies of scale
- The core concept involves pooling computing resources to serve multiple consumers using a multi-tenant model, with different physical and virtual resources dynamically assigned and reassigned according to consumer demand
- On-demand self-service enables consumers to provision computing capabilities automatically without requiring human interaction with service providers
- Broad network access means capabilities are available over the network and accessed through standard mechanisms that promote use by heterogeneous thin or thick client platforms
- Resource pooling allows provider's computing resources to be pooled to serve multiple consumers using a multi-tenant model, with different physical and virtual resources dynamically assigned and reassigned according to consumer demand
- Rapid elasticity enables capabilities to be elastically provisioned and released to scale rapidly outward and inward commensurate with demand
- Measured service means cloud systems automatically control and optimize resource use by leveraging a metering capability at some level of abstraction appropriate to the type of service
- Virtualization is the foundational technology that enables cloud computing by abstracting physical hardware and allowing multiple virtual machines to run on a single physical server
- Abstraction separates the management layer from the hardware layer, enabling administrators to manage resources independently of the underlying physical infrastructure
- Orchestration automates the arrangement, coordination, and management of complex computer systems, middleware, and services in cloud environments

## Evolution of cloud computing
- Mainframe computing (1950s-1960s) introduced time-sharing and multiple user access to centralized computing resources, establishing concepts later used in cloud computing
- Client-server architecture (1970s-1980s) distributed processing between clients requesting services and servers providing them, creating the foundation for distributed computing models
- Virtualization technology (1990s) revolutionized data centers by allowing multiple operating systems to run on a single physical machine, significantly improving hardware utilization
- Application Service Providers (ASPs) emerged in the late 1990s, delivering software applications over the internet, foreshadowing modern SaaS models
- Amazon Web Services launched in 2006 with Simple Storage Service (S3) and Elastic Compute Cloud (EC2), marking the beginning of modern IaaS cloud computing
- Google App Engine launched in 2008 as one of the first PaaS offerings, abstracting infrastructure management and allowing developers to focus solely on code
- Microsoft Azure launched in 2010, providing comprehensive IaaS and PaaS capabilities integrated with Microsoft's enterprise software ecosystem
- Hybrid cloud models evolved around 2012-2015 as enterprises sought to balance on-premises control with public cloud flexibility and scalability
- Serverless computing emerged around 2014-2016 with AWS Lambda, abstracting even server management and enabling pure event-driven, function-based architectures
- Multi-cloud and distributed cloud strategies have become prevalent since 2018, with organizations leveraging multiple providers and edge computing for optimization and resilience

## IaaS vs PaaS vs SaaS
- Infrastructure as a Service (IaaS) provides on-demand access to IT infrastructure—servers, virtual machines, storage, networks—for tasks such as hosting applications, running workloads, and managing data while maintaining control over operating systems and applications
- IaaS offers the highest level of flexibility and management control, making it ideal for complex, custom applications, lift-and-shift migrations, and organizations with specific compliance or security requirements
- Platform as a Service (PaaS) delivers a framework for developers to build and deploy custom applications without managing the underlying infrastructure, including operating systems, databases, and middleware
- PaaS accelerates development and deployment of applications by providing pre-built tools, services, and frameworks, reducing the complexity of managing development stacks and infrastructure
- PaaS offerings typically include development tools, database management systems, middleware, and business analytics services integrated into a single platform
- Software as a Service (SaaS) delivers complete software applications over the internet on a subscription basis, eliminating the need for organizations to install, manage, and maintain software
- SaaS provides the lowest level of control but highest level of convenience, with providers managing everything from infrastructure to application data and security
- Key comparison factors include control level (highest in IaaS, lowest in SaaS), management responsibility (user manages applications and data in IaaS, provider manages everything in SaaS)
- Pricing models differ: IaaS typically charges for compute, storage, and network usage; PaaS charges for platform services and resources consumed; SaaS charges per-user subscription fees
- Use case alignment: IaaS for custom infrastructure and lift-and-shift, PaaS for application development and deployment, SaaS for ready-to-use business applications

## Public vs private vs hybrid cloud
- Public cloud is owned and operated by third-party cloud service providers who deliver computing resources over the internet, with all hardware, software, and supporting infrastructure managed by the provider
- Public cloud advantages include eliminating capital expenditure, unlimited scalability, rapid deployment, and no infrastructure management burden, with costs shared across multiple tenants
- Public cloud challenges include potential security concerns, limited control over infrastructure, compliance considerations for regulated data, and potential for unexpected costs
- Private cloud consists of computing resources used exclusively by a single organization, which can be located on-premises or hosted by a third-party service provider
- Private cloud advantages include greater control, enhanced security and compliance capabilities, predictable performance, and the ability to customize infrastructure to specific needs
- Private cloud challenges include higher costs, increased management responsibility, limited scalability compared to public cloud, and requirement for specialized IT skills
- Hybrid cloud combines public and private clouds, allowing data and applications to be shared between them, enabling organizations to gain the benefits of both models
- Hybrid cloud enables workload portability, orchestration, and management across environments, allowing organizations to keep sensitive data in private cloud while leveraging public cloud for burst capacity
- Key hybrid cloud use cases include cloud bursting (using public cloud during peak demand), disaster recovery, data backup, and running workloads where they are most cost-effective
- Hybrid cloud challenges include complexity in integration, network latency considerations, security consistency across environments, and requiring robust management tools

## Multi-cloud strategies
- Multi-cloud strategy involves using multiple cloud services from different providers—such as AWS, Azure, and GCP—for different workloads or as redundancy for critical applications
- Avoiding vendor lock-in is a primary driver, allowing organizations to maintain negotiating power and flexibility to switch providers when advantageous
- Best-of-breed approach enables organizations to select the most suitable services from each provider based on specific workload requirements and performance characteristics
- Geographic distribution benefits include leveraging different providers' regional presence to optimize latency and comply with data sovereignty requirements
- Risk mitigation through diversification reduces impact of provider outages, service disruptions, or security incidents affecting a single cloud provider
- Cost optimization opportunities arise from comparing pricing models and taking advantage of different providers' pricing structures and commitment discounts
- Compliance and regulatory requirements may necessitate using specific providers in certain regions or for particular data classifications
- Technical challenges include increased complexity in management, security, networking, and monitoring across multiple platforms requiring specialized skills
- Governance complexity increases as organizations must maintain consistent policies, access controls, and compliance frameworks across disparate cloud environments
- Multi-cloud management tools and abstraction layers help address challenges by providing unified visibility, orchestration, and policy enforcement across providers

## Cloud service models
- Infrastructure services provide fundamental computing resources like virtual machines, block storage, and virtual networks, with customers responsible for managing operating systems and applications
- Compute services include virtual machines, containers, and serverless functions that execute application code and process workloads in the cloud
- Storage services encompass object storage, block storage, file storage, and archival storage, each optimized for different data access patterns and durability requirements
- Database services range from relational databases to NoSQL databases, managed services, and data warehouses, abstracting database administration tasks
- Networking services include virtual networks, load balancers, firewalls, DNS, and content delivery networks that connect and protect cloud resources
- Platform services provide application hosting, development tools, integration services, and middleware that enable developers to build and deploy applications
- Analytics services include data processing, business intelligence, machine learning, and data visualization tools that derive insights from data
- Integration services facilitate connectivity between applications and data sources through APIs, message queues, event buses, and workflow orchestration
- Security and identity services manage authentication, authorization, encryption, and threat detection across cloud environments
- Management and governance services provide monitoring, logging, cost management, and compliance tools for operating cloud environments

## Cloud deployment models
- Public cloud deployment makes resources available to the general public over the internet, with infrastructure owned and operated by the cloud provider at their data centers
- Private cloud deployment dedicates cloud infrastructure to a single organization, which may be managed internally or by a third party, and hosted on-premises or externally
- Community cloud deployment shares infrastructure between several organizations with common concerns—such as security, compliance, or jurisdiction—for shared benefits
- Hybrid cloud deployment combines two or more distinct cloud infrastructures (public, private, or community) that remain unique entities but bound by standardized technology
- Multi-cloud deployment involves using multiple public cloud services from different providers, with workloads distributed across AWS, Azure, GCP, and others
- Distributed cloud deployment extends public cloud infrastructure to different physical locations while maintaining centralized management and control
- Edge cloud deployment places computing resources at the edge of the network, closer to data sources and users, to reduce latency and bandwidth usage
- Sovereign cloud deployment ensures data residency and compliance with local laws by operating cloud services within specific geographic boundaries
- Telco cloud deployment optimizes cloud infrastructure for telecommunications workloads, supporting network functions virtualization and edge computing
- Bare metal cloud deployment provides dedicated physical servers without virtualization, offering raw performance for specialized workloads while maintaining cloud-like provisioning

## Shared responsibility model
- Shared responsibility model defines security and compliance responsibilities between cloud providers and customers, which varies depending on the service model (IaaS, PaaS, SaaS)
- Provider is always responsible for physical security of data centers, hardware, software, and networking that comprise the cloud infrastructure globally
- Provider manages the virtualization layer, including hypervisors, which isolates customer workloads and prevents unauthorized access between tenants
- Provider maintains global infrastructure availability and durability, ensuring core services meet published SLAs and redundancy requirements
- For IaaS, customers are responsible for managing guest operating systems, applications, data, network configurations, firewall settings, and identity access management
- For PaaS, customers focus on their applications and data while providers handle runtime environments, middleware, operating systems, and infrastructure
- For SaaS, customers typically only manage their data, user access, and application configurations, with provider managing everything else
- Data classification and accountability remains with customers regardless of service model, including where data is stored and who can access it
- Identity and access management is a shared area where providers offer tools but customers must implement proper authentication and authorization policies
- Compliance certifications and attestations are provided by cloud vendors, but customers must ensure their use of services meets specific regulatory requirements

## Cloud economics
- Capital expenditure (CapEx) shifts to operational expenditure (OpEx) when moving to cloud, replacing upfront hardware purchases with pay-as-you-go consumption-based costs
- Total Cost of Ownership (TCO) analysis compares direct and indirect costs of on-premises infrastructure versus cloud solutions over time
- Economies of scale enable cloud providers to achieve lower costs than individual organizations through massive infrastructure purchasing power and operational efficiency
- Variable costs replace fixed costs, allowing organizations to pay only for resources consumed rather than maintaining capacity for peak demand
- Reduced time-to-market accelerates business value by eliminating procurement cycles and infrastructure deployment delays
- Labor cost optimization reduces staff required for hardware maintenance, data center operations, and infrastructure management
- Energy and facility cost elimination removes expenses for power, cooling, physical security, and data center space
- Resource optimization through rightsizing ensures organizations don't overpay for underutilized resources and can scale down when demand decreases
- Reserved instances and savings plans provide significant discounts for committed usage, reducing costs for predictable workloads
- Spot instances and preemptible VMs offer steep discounts for fault-tolerant, interruptible workloads, maximizing cost efficiency

## Pay-as-you-go pricing
- Pay-as-you-go model charges customers based on actual resource consumption without upfront commitments or long-term contracts
- Compute pricing typically charges per second or per hour of virtual machine usage, with variations based on instance type, region, and operating system
- Storage pricing depends on data volume stored, storage class or tier, and data access patterns including retrieval frequency and data transfer
- Data transfer pricing includes charges for data moving between regions, to the internet, or between availability zones, while ingress often remains free
- API request pricing applies to many services, charging per operation such as S3 PUT/GET requests or Lambda function invocations
- Reserved capacity pricing offers discounted rates in exchange for committing to use specific resources for one or three-year terms
- Spot pricing allows bidding on unused capacity, with prices fluctuating based on supply and demand, and instances potentially reclaimed with notice
- Tiered pricing provides volume discounts where per-unit costs decrease as usage increases across compute, storage, or data transfer
- Sustained use discounts automatically apply for extended usage periods, particularly in Google Cloud's compute pricing model
- Free tiers provide limited usage at no cost, allowing organizations to experiment and run small workloads without charges

## Elasticity and scalability
- Elasticity refers to the ability to automatically scale resources up or down based on real-time demand, matching capacity to current requirements
- Horizontal scaling (scaling out) adds more instances of a resource, distributing load across multiple servers for increased capacity and fault tolerance
- Vertical scaling (scaling up) increases the capacity of existing instances by adding more CPU, memory, or storage to handle larger workloads
- Auto-scaling automatically adjusts compute capacity based on policies, schedules, or real-time metrics like CPU utilization or request count
- Predictive scaling uses machine learning to forecast traffic and proactively scale resources before demand increases
- Scheduled scaling adjusts capacity at predetermined times to accommodate known patterns like end-of-month processing or seasonal peaks
- Manual scaling provides direct control for administrators to adjust resources when automatic policies aren't suitable or during planned events
- Load balancing distributes traffic across scaled resources, ensuring even distribution and high availability during scaling events
- Database scaling involves techniques like read replicas, sharding, and connection pooling to handle increased query volume
- Scaling considerations include startup time for new instances, state management, and ensuring applications are designed to handle dynamic environments

## High availability concepts
- High availability ensures systems remain operational and accessible despite component failures, typically measured as percentage of uptime over time
- Availability targets are expressed as "nines"—99.9% (three nines) allows ~8.76 hours downtime yearly, while 99.999% (five nines) allows ~5.26 minutes
- Redundancy eliminates single points of failure by deploying multiple instances of critical components across different failure domains
- Active-active configurations distribute workload across multiple running instances, maximizing resource utilization and providing seamless failover
- Active-passive configurations maintain standby instances that take over when primary fails, potentially simpler but with some failover delay
- Failover automatically transfers operations to redundant systems when primary systems fail, requiring detection, routing changes, and state synchronization
- Health monitoring continuously checks system components to detect failures quickly and trigger appropriate remediation actions
- Graceful degradation ensures core functionality remains available even when non-critical components fail, maintaining essential user experience
- Maintenance windows for updates and patches must be considered in HA design, with rolling updates enabling zero-downtime maintenance
- Geographic distribution across regions protects against region-wide outages but introduces complexity in data synchronization and latency

## Fault tolerance
- Fault tolerance enables systems to continue operating correctly even when some components fail, without any interruption in service
- Redundancy at multiple levels—hardware, network, power, data—ensures that failures in individual components don't cause system failure
- Replication maintains identical copies of data or services across different physical locations to withstand localized failures
- Failover mechanisms automatically redirect traffic and operations from failed components to healthy ones without manual intervention
- Graceful degradation allows systems to maintain partial functionality when complete operation isn't possible, prioritizing critical functions
- Isolation prevents failures from cascading through systems, using techniques like bulkheads, circuit breakers, and rate limiting
- Recovery mechanisms include automated restart of failed processes, reconstruction of corrupted data, and reintegration of recovered components
- Byzantine fault tolerance addresses arbitrary failures including malicious behavior, requiring consensus among distributed components
- Testing fault tolerance through chaos engineering deliberately introduces failures to validate system resilience and recovery capabilities
- Cost considerations require balancing fault tolerance levels against business requirements, as five-nines availability becomes exponentially expensive

## Disaster recovery fundamentals
- Disaster recovery encompasses policies, procedures, and technologies for restoring critical systems and data after catastrophic failures
- Recovery Time Objective (RTO) defines maximum acceptable downtime for restoring systems after disaster, driving infrastructure and process requirements
- Recovery Point Objective (RPO) defines maximum acceptable data loss measured in time, determining backup frequency and replication requirements
- Backup and restore approach uses regular backups to stored snapshots, providing cost-effective recovery but longer RTO for full system restoration
- Pilot light maintains minimal core infrastructure running in recovery region, allowing rapid scale-up when disaster strikes the primary region
- Warm standby runs reduced-capacity production environment in recovery region, enabling faster failover than pilot light with moderate costs
- Multi-site active-active runs production workloads simultaneously across regions, providing near-zero RTO and RPO at highest cost
- Data replication synchronously or asynchronously copies data to recovery region, balancing data loss risk against performance impact
- Disaster recovery testing through regular drills validates procedures, identifies gaps, and ensures team readiness for actual events
- Automation of failover processes reduces recovery time and eliminates human error during high-stress disaster situations

## Regions and availability zones
- Regions are geographically distinct locations containing multiple, isolated Availability Zones, providing global footprint for cloud services
- Availability Zones consist of one or more discrete data centers with independent power, cooling, and networking, isolated from failures in other zones
- Latency between Availability Zones in the same region is typically under 2 milliseconds, enabling synchronous replication and distributed applications
- Region selection considerations include data residency requirements, compliance regulations, latency to users, and service availability
- Multi-region deployments provide resilience against region-wide disasters but introduce challenges in data consistency and cross-region latency
- Edge locations form content delivery networks, caching data close to users for low-latency access without full regional infrastructure
- Local zones extend regions to metropolitan areas for ultra-low latency applications requiring single-digit millisecond response times
- Wavelength zones embed AWS compute at 5G network edges, enabling applications requiring single-digit millisecond latency to mobile devices
- Outposts bring AWS infrastructure on-premises for workloads requiring low latency to on-premises systems or local data processing
- Government regions provide physically isolated infrastructure meeting specific compliance requirements for public sector workloads

## Cloud SLAs
- Service Level Agreements are formal contracts between cloud providers and customers defining expected service performance and availability commitments
- Availability commitments typically range from 99.5% to 99.99% depending on service tier, with premium tiers offering higher guarantees
- Financial credits (service credits) are provided when SLAs aren't met, typically as percentage discounts on future bills based on actual uptime
- Exclusions and limitations specify circumstances where SLA doesn't apply, including scheduled maintenance, force majeure, and customer actions
- Composite SLA calculation for multi-service applications requires combining individual service SLAs, potentially resulting in lower overall availability
- Multi-region deployments can achieve higher effective availability by routing around regional failures despite individual regional SLAs
- Performance SLAs address metrics beyond availability including latency, throughput, and request rates for specific services
- Measurement and reporting defines how service levels are monitored, verified, and reported to customers, typically through service health dashboards
- Notification requirements specify provider obligations for incident communication, maintenance windows, and service status updates
- Third-party verification through independent auditors validates provider compliance with published SLAs and operational practices

## Cloud compliance basics
- Compliance frameworks (ISO 27001, SOC 1/2/3, PCI DSS, HIPAA, FedRAMP) provide standardized security and privacy controls for cloud services
- Data residency requirements mandate that certain data must remain within specific geographic boundaries to comply with local laws
- Shared responsibility extends to compliance, with providers responsible for infrastructure compliance and customers for their use of services
- Compliance certifications are obtained by cloud providers through third-party audits, validating their controls meet framework requirements
- Customer compliance obligations include configuring services properly, managing access controls, and protecting data according to requirements
- Audit rights enable customers to review provider compliance evidence, often through access to audit reports rather than direct inspection
- Compliance automation tools help customers maintain compliance by continuously monitoring configurations and generating evidence
- Industry-specific regulations (GDPR, HIPAA, FedRAMP) impose additional requirements for handling protected data in specific sectors
- Compliance inheritance allows customers to leverage provider certifications for their own compliance programs, reducing audit burden
- Continuous compliance monitoring through cloud-native tools detects violations and generates alerts for non-compliant configurations

## Cloud governance
- Cloud governance establishes policies, procedures, and controls for managing cloud resources effectively, securely, and cost-efficiently
- Resource hierarchy organizes cloud assets into logical groups (organizations, folders, projects) for applying policies and managing access
- Policy as code defines governance rules programmatically, enabling automated enforcement and consistent application across environments
- Tagging and labeling strategies attach metadata to resources for cost allocation, ownership tracking, and compliance classification
- Budget controls and alerts prevent cost overruns by notifying stakeholders when spending approaches or exceeds thresholds
- Service limits and quotas prevent resource exhaustion or unexpected bills by cording maximum resource consumption
- Compliance frameworks integrate with governance to ensure resources meet regulatory and security requirements continuously
- Access governance ensures least-privilege principle through identity management, role-based access control, and regular access reviews
- Change management controls govern infrastructure modifications through approval workflows and automated compliance checks
- Governance automation continuously monitors, detects violations, and remediates non-compliant configurations without human intervention

## Cloud adoption frameworks
- Cloud Adoption Framework (CAF) provides structured guidance, best practices, and tools for organizations planning and executing cloud adoption
- AWS CAF organizes guidance into six perspectives: Business, People, Governance, Platform, Security, and Operations, each addressing specific stakeholder concerns
- Microsoft CAF for Azure includes strategy, plan, ready, adopt, govern, and manage phases with detailed methodologies for each stage
- Google CAF focuses on four themes: Learn, Lead, Scale, and Secure, helping organizations transform through cloud adoption
- Business perspective addresses financial management, strategy alignment, and value realization from cloud investments
- People perspective focuses on organizational change management, training, and culture transformation for cloud adoption
- Governance perspective establishes policies, controls, and management processes for maintaining oversight of cloud environments
- Platform perspective guides technical implementation, including landing zones, architecture patterns, and workload migration
- Security perspective ensures protection of data, identities, and applications throughout cloud adoption journey
- Operations perspective defines processes for running, monitoring, and optimizing cloud workloads post-migration

## Cloud migration strategies
- Migration strategies (the "6 Rs") provide standardized approaches for moving applications to cloud based on business and technical requirements
- Rehost (lift-and-shift) moves applications to cloud without modifications, providing quick migration with minimal changes but missing cloud optimization benefits
- Replatform (lift-tinker-and-shift) makes targeted cloud optimizations like moving to managed databases while keeping core application architecture
- Refactor/Re-architect modifies applications to use cloud-native features, maximizing cloud benefits but requiring more time and investment
- Repurchase replaces existing applications with SaaS alternatives, trading customization for reduced management overhead
- Retire decommissions applications no longer needed, eliminating maintenance costs and reducing migration scope
- Retain keeps applications on-premises temporarily or permanently due to technical, regulatory, or business constraints
- Migration waves group applications for phased migration based on dependencies, complexity, and business priorities
- Application discovery and assessment evaluates existing applications to determine appropriate migration strategies
- Post-migration optimization continuously improves migrated workloads through rightsizing, architecture enhancements, and cost management

## Lift-and-shift vs refactor
- Lift-and-shift migrates applications to cloud with minimal changes, preserving existing architecture while changing infrastructure location
- Lift-and-shift advantages include fastest migration timelines, lowest initial risk, minimal application changes, and preserving existing skills
- Lift-and-shift disadvantages include missing cloud optimization benefits, potential higher costs without rightsizing, and limited scalability improvements
- Refactoring modifies applications to use cloud-native services and architectures, often involving significant code changes
- Refactoring advantages include optimized cloud benefits, improved scalability, reduced operational costs, and enhanced agility
- Refactoring disadvantages include longer timelines, higher upfront investment, development risks, and requiring new skills
- Replatforming offers middle ground by making targeted optimizations like managed databases while maintaining core application architecture
- Business drivers for lift-and-shift include data center exit deadlines, quick wins, and risk-averse compliance requirements
- Business drivers for refactor include long-term cost optimization, scaling requirements, and competitive advantage through agility
- Hybrid approaches migrate some applications with lift-and-shift while refactoring others based on business value and technical requirements

## Vendor lock-in considerations
- Vendor lock-in occurs when dependency on a specific cloud provider's proprietary services makes switching providers costly or technically difficult
- Technical lock-in arises from using provider-specific APIs, services, or features that have no equivalent in other cloud platforms
- Contractual lock-in results from long-term commitments, volume discounts, or early termination penalties that make switching expensive
- Data lock-in involves challenges in moving large volumes of data between providers due to egress costs, transfer times, and format differences
- Skills lock-in occurs when teams develop expertise in specific provider tools and services that don't transfer to other platforms
- Mitigation strategies include using open standards, multi-cloud approaches, and abstraction layers to reduce dependency on specific providers
- Containerization and Kubernetes provide portable deployment models that reduce lock-in at the application packaging level
- Infrastructure as code enables re-deployment across providers but requires provider-specific configurations for each platform
- Cost analysis should include switching costs when comparing provider offerings and negotiating contracts
- Strategic decisions require balancing lock-in risks against benefits of using advanced provider-specific services that may justify the dependency

## Cloud security fundamentals
- Defense in depth implements multiple layers of security controls so that failure of one layer doesn't compromise overall security
- Identity and access management authenticates users and authorizes their access to resources following least-privilege principles
- Encryption protects data at rest and in transit, ensuring confidentiality even if other controls fail or data is intercepted
- Network security controls including firewalls, segmentation, and web application firewalls protect against network-based attacks
- Security monitoring continuously detects threats, anomalies, and policy violations through logging, analysis, and alerting
- Vulnerability management identifies and remediates security weaknesses in applications, configurations, and infrastructure
- Incident response procedures define how to detect, contain, eradicate, and recover from security incidents
- Compliance validation ensures security controls meet regulatory and policy requirements through continuous assessment
- Shared responsibility clarifies which security aspects customers own versus providers, preventing gaps in coverage
- Security automation enables rapid response to threats and consistent enforcement of security policies across environments

## Identity and access management basics
- Authentication verifies identity through factors including something you know (password), something you have (token), or something you are (biometrics)
- Authorization determines what authenticated identities can access and perform, following least-privilege principles
- Multi-factor authentication requires two or more verification methods, significantly reducing risk of credential compromise
- Single sign-on enables users to authenticate once and access multiple applications without re-entering credentials
- Identity federation establishes trust between identity providers and service providers, enabling external identity usage
- Role-based access control assigns permissions to roles rather than individuals, simplifying management and improving security
- Policy-based access defines fine-grained permissions through policies that specify allowed or denied actions under conditions
- Service identities (service accounts) enable applications and services to authenticate and access resources without human interaction
- Privileged access management controls and monitors highly privileged accounts that could cause significant damage if compromised
- Identity lifecycle management provisions, modifies, and deprovisions access as users join, change roles, or leave organizations

## Cloud networking fundamentals
- Virtual networks provide isolated, software-defined network environments in the cloud, resembling traditional on-premises networks
- Subnets divide virtual networks into smaller segments for organization, security, and traffic management purposes
- IP addressing assigns public and private IP addresses to cloud resources, with private addresses used for internal communication
- Network address translation enables resources with private IPs to access internet while hiding internal addressing from external networks
- DNS resolves domain names to IP addresses, with cloud providers offering managed DNS services for reliability and low latency
- Load balancing distributes traffic across multiple resources for high availability, scalability, and health-aware routing
- Firewalls control traffic based on rules, filtering at the network or application layer to block unauthorized access
- Virtual private networks extend on-premises networks to cloud securely over the internet using encrypted tunnels
- Direct connections provide private, dedicated physical connections between on-premises and cloud for consistent performance
- Content delivery networks cache content at edge locations, reducing latency and bandwidth costs for global users

## Cloud monitoring basics
- Metrics collect numerical data about resource utilization, performance, and health at regular intervals for trend analysis and alerting
- Logs capture detailed records of events, errors, and activities from applications, operating systems, and cloud services
- Traces track requests as they travel through distributed systems, identifying bottlenecks and failures in complex architectures
- Dashboards visualize monitoring data in customizable views, providing at-a-glance status of system health and performance
- Alerts notify operators when metrics exceed thresholds or specific events occur, enabling rapid response to issues
- Health checks continuously verify that resources and applications are responding correctly, triggering remediation when they fail
- Uptime monitoring tracks service availability from multiple locations, detecting outages before users are affected
- Performance monitoring measures response times, throughput, and error rates to identify degradation and capacity needs
- Cost monitoring tracks cloud spending, identifying anomalies and opportunities for optimization
- Compliance monitoring continuously checks configurations against policies, detecting violations and generating audit evidence

## Cloud automation basics
- Infrastructure as code manages and provisions infrastructure through machine-readable definition files rather than manual processes
- Configuration management tools ensure systems maintain desired state, automatically correcting drift from defined configurations
- Orchestration coordinates multiple automated tasks into workflows, managing dependencies and execution order
- Continuous integration automatically builds and tests code changes, providing rapid feedback on quality and integration issues
- Continuous deployment automates release of validated code to environments, reducing manual effort and errors
- Policy as code defines governance rules programmatically, enabling automated enforcement across cloud resources
- Auto-scaling automatically adjusts resource capacity based on demand, optimizing cost and performance without manual intervention
- Event-driven automation triggers actions in response to events, enabling self-healing and automated remediation
- Serverless automation runs code in response to events without managing servers, simplifying automation implementation
- API-driven automation enables programmatic control of cloud resources, integrating with existing tools and workflows

# Cloud Architecture

## Cloud architecture principles
- Design for failure assumes components will fail and builds systems that remain available despite individual failures
- Loose coupling minimizes dependencies between components, allowing them to evolve independently and failures to be contained
- Elasticity enables systems to scale resources dynamically based on demand, optimizing cost and performance
- Automation reduces manual intervention, minimizing human error and enabling consistent, repeatable operations
- Security by design incorporates security throughout architecture rather than adding as an afterthought
- Data management strategies address data partitioning, replication, consistency, and lifecycle for distributed systems
- Cost optimization balances performance and features against expenses, eliminating waste and maximizing business value
- Performance efficiency ensures systems meet response time and throughput requirements under expected loads
- Operational excellence focuses on running and monitoring systems effectively, with observability and incident response
- Sustainability considers environmental impact through efficient resource utilization and workload placement

## Well-architected frameworks
- AWS Well-Architected Framework provides six pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability
- Operational Excellence pillar focuses on running and monitoring systems, continuous improvement, and automated operations
- Security pillar addresses data protection, identity management, infrastructure protection, and incident detection and response
- Reliability pillar ensures systems recover from failures, scale dynamically, and meet availability and recovery objectives
- Performance Efficiency pillar maintains efficiency as demand changes, using monitoring and optimization to evolve systems
- Cost Optimization pillar avoids unnecessary costs, matches supply with demand, and increases expenditure awareness over time
- Sustainability pillar minimizes environmental impact through efficient resource utilization and workload optimization
- Microsoft Azure Well-Architected Framework includes five pillars: Cost Optimization, Operational Excellence, Performance Efficiency, Reliability, and Security
- Google Cloud Architecture Framework organizes best practices across system design, security, privacy, reliability, cost optimization, and performance
- Review questions in each pillar guide architects through evaluating designs against best practices and identifying improvements

## Scalability patterns
- Horizontal scaling (scale out) adds more instances of a resource, distributing load across multiple servers for increased capacity
- Vertical scaling (scale up) increases capacity of existing instances by adding more CPU, memory, or other resources
- Stateless design ensures any instance can handle any request by storing session state in external shared storage
- Database read replicas distribute read queries across multiple copies, increasing overall query throughput
- Sharding partitions data across multiple databases based on a shard key, distributing both read and write load
- Queue-based load leveling uses message queues to smooth request spikes, allowing consumers to process at optimal rate
- Auto-scaling automatically adjusts resources based on demand, using metrics, schedules, or predictive algorithms
- Content delivery networks cache static and dynamic content at edge locations, reducing load on origin servers
- Caching stores frequently accessed data in fast memory, reducing database load and improving response times
- Asynchronous processing offloads long-running tasks to background processes, keeping frontends responsive under load

## Resilience patterns
- Retry with exponential backoff automatically retries failed operations with increasing delays, preventing overwhelming recovering services
- Circuit breaker prevents repeated calls to failing services, allowing time for recovery and failing fast when appropriate
- Bulkhead isolates failures by partitioning resources into separate pools, preventing failure in one area from cascading
- Health endpoint monitoring exposes service health through dedicated endpoints, enabling load balancers and orchestrators to route traffic appropriately
- Throttling limits request rates to protect services from overload, maintaining availability for all users
- Graceful degradation reduces functionality rather than completely failing, maintaining core user experience during partial outages
- Fallback provides alternative responses or functionality when primary services fail, maintaining some level of service
- Leader election ensures only one instance performs specific tasks, preventing conflicts and ensuring reliable execution
- Quorum-based decisions require consensus among distributed components, ensuring consistency despite partial failures
- Chaos engineering proactively introduces failures to test resilience, revealing weaknesses before they cause real incidents

## Reliability engineering
- Service Level Objectives define target reliability levels that guide design decisions and operational practices
- Error budgets calculate acceptable failure rates as difference between perfect reliability and SLO, balancing innovation against stability
- Redundancy eliminates single points of failure through multiple instances, availability zones, and regions
- Fault isolation prevents failures from spreading by containing them within boundaries like cells or shards
- Automated recovery responds to failures without human intervention, reducing downtime and eliminating human error
- Capacity planning ensures resources meet demand through monitoring trends, modeling growth, and planning for spikes
- Dependency management identifies and mitigates risks from external dependencies through fallbacks and graceful degradation
- Incident management processes detect, respond to, and learn from failures through blameless post-mortems
- Load testing validates system behavior under expected and peak loads, identifying bottlenecks before production
- Release engineering ensures safe deployments through canary releases, blue-green deployments, and feature flags

## Distributed systems fundamentals
- Distributed systems consist of multiple independent computers appearing to users as a single coherent system
- Network communication introduces latency, partial failure, and security considerations not present in single systems
- Consensus algorithms enable distributed components to agree on values despite failures (Paxos, Raft)
- Time and ordering challenges arise from lack of global clock, requiring logical clocks for event ordering
- Distributed transactions coordinate operations across multiple systems, with trade-offs between consistency and availability
- Message passing enables communication between components through synchronous (request-response) or asynchronous patterns
- Distributed coordination services (ZooKeeper, etcd) manage configuration, synchronization, and group services
- Distributed caching improves performance by storing data closer to computation, with consistency challenges
- Distributed monitoring aggregates metrics, logs, and traces from multiple components for observability
- Failure detection identifies failed nodes through timeouts, heartbeats, and gossip protocols

## CAP theorem
- CAP theorem states distributed data stores can provide at most two of three guarantees: Consistency, Availability, and Partition tolerance
- Consistency means all nodes see the same data simultaneously, with reads returning most recent write
- Availability means every request receives a response, without guarantee it contains the most recent write
- Partition tolerance means system continues operating despite network partitions dropping or delaying messages
- During network partition, systems must choose between consistency (refusing writes until partition heals) or availability (accepting writes with potential inconsistency)
- CP systems prioritize consistency over availability during partitions, blocking writes until consistency can be ensured
- AP systems prioritize availability over consistency during partitions, accepting writes that may cause temporary inconsistency
- PACELC extends CAP, stating in absence of partition, systems choose between latency and consistency
- Real-world systems often provide tunable consistency, allowing different guarantees for different operations
- Understanding CAP guides database selection and architecture design based on application requirements

## Consistency models
- Strong consistency ensures all reads see the most recent write, providing a linearizable view of data
- Eventual consistency guarantees that if no new updates, all replicas will eventually converge to the same value
- Read-after-write consistency ensures a client always sees its own writes immediately after writing
- Monotonic read consistency guarantees a client sees increasingly recent data over time, never going back in time
- Monotonic write consistency ensures writes from a client are applied in the order they were issued
- Causal consistency ensures causally related operations appear in the same order to all nodes
- Quorum-based consistency uses configurable read and write quorums to balance consistency and performance
- Session consistency provides consistency guarantees within a client session, simplifying application development
- Transactional consistency (ACID) ensures atomic, consistent, isolated, and durable operations
- BASE (Basically Available, Soft state, Eventual consistency) contrasts with ACID for distributed systems

## Event-driven architecture
- Event-driven architecture uses events to trigger and communicate between decoupled services
- Event producers generate events without knowledge of consumers, enabling loose coupling and scalability
- Event consumers react to events asynchronously, processing them when resources are available
- Event routers or brokers (Kafka, EventBridge, SNS) receive events and deliver them to appropriate consumers
- Event sourcing stores state changes as sequence of events, enabling auditability and state reconstruction
- Command Query Responsibility Segregation separates write operations (commands) from read operations (queries)
- Eventual consistency accepts temporary inconsistency between event publication and consumer processing
- Dead letter queues capture events that cannot be processed, enabling debugging and reprocessing
- Schema evolution manages










