# Calculating Non-Functional/Low-Level Constraints

Non-functional requirements define the quality attributes and constraints of a system. Properly calculating and documenting these constraints is essential for system design and technical debt management.

## Key Non-Functional Areas

### 1. Performance
- **Response Time**: Maximum acceptable time for system response
- **Throughput**: Required transactions per second
- **Latency**: Acceptable delay in data processing
- **Calculation Method**: Varies by metric (see frameworks below)

**Framework Example**: RAIL Model (Response, Animation, Idle, Load)

The RAIL model is a user-centric performance framework that breaks down the user's experience into key actions:
- Response: Process events in under 50ms
- Animation: Produce a frame in 10ms
- Idle: Maximize idle time
- Load: Deliver content and become interactive in under 5 seconds

**Measurement Methods for RAIL**:
- **Response**: Use Event Timing API or performance marks to measure input latency
- **Animation**: Use Frame Timing API to measure frame rates and jank
- **Idle**: Use Intersection Observer and requestIdleCallback to measure idle time usage
- **Load**: Use Navigation Timing API, Resource Timing API, and Lighthouse metrics (FCP, LCP, TTI)

**Tools for RAIL Measurement**:
- Chrome DevTools Performance panel
- Lighthouse in Chrome DevTools or as a CI tool
- WebPageTest for synthetic testing
- Chrome User Experience Report (CrUX) for real-user monitoring

**Reference**: [Google Web Fundamentals: RAIL Model](https://developers.google.com/web/fundamentals/performance/rail)

**Related Topics**: 
- For server-side performance testing, see [Load Testing](/chapter_5/load_testing.md)
- For frontend performance metrics, see [Web Vitals](https://web.dev/vitals/)

### 2. Scalability
- **Concurrent Users**: Maximum number of simultaneous users
- **Data Volume**: Growth projections for data storage
- **Request Rate**: Peak request rates and growth projections
- **Calculation Method**: Capacity planning models, growth forecasting, load testing

**Framework Example**: Universal Scalability Law (USL)

The USL is a mathematical model that helps predict system scalability by considering:
- Linear scalability (perfect scaling)
- Contention (serialization of resources)
- Coherency (cost of maintaining consistency)

The formula: C(N) = N / (1 + α(N-1) + βN(N-1))
Where:
- N is the number of processors/nodes
- α is the contention penalty
- β is the coherency penalty

**Validation Methods**:
- Load testing to verify auto-scaling capabilities
- Stress testing to identify breaking points
- Soak testing to detect resource leaks under sustained load
- Chaos engineering to test resilience during failures

**Reference**: [Dr. Neil Gunther's "Guerrilla Capacity Planning"](https://www.springer.com/gp/book/9783540261384)

**Related Topic**: For detailed information on testing scalability through simulated load, see [Load Testing](/chapter_5/load_testing.md)

### 3. Availability
- **Uptime Requirements**: Expressed as percentage (e.g., 99.9%)
- **Planned Downtime**: Maintenance windows and frequency
- **Recovery Time Objective (RTO)**: Maximum acceptable downtime
- **Recovery Point Objective (RPO)**: Maximum acceptable data loss
- **Calculation Method**: Business impact analysis, SLA requirements

**Framework Example**: Site Reliability Engineering (SRE) Availability Framework

Google's SRE framework uses Service Level Indicators (SLIs), Service Level Objectives (SLOs), and Service Level Agreements (SLAs) to define and measure availability:
- SLI: A direct measurement of service behavior (e.g., request latency)
- SLO: A target value for an SLI (e.g., 99.9% of requests < 100ms)
- SLA: A business agreement that includes consequences of meeting/missing SLOs

The framework also introduces the concept of an "error budget" - the allowed amount of downtime based on SLOs.

**Reference**: [Google's Site Reliability Engineering Book](https://sre.google/sre-book/availability-table/)

### 4. Security
- **Authentication Requirements**: Methods and strength
- **Authorization Levels**: Access control granularity
- **Data Protection**: Encryption requirements
- **Calculation Method**: Threat modeling, compliance requirements

**Framework Example**: STRIDE Threat Modeling

STRIDE is a structured approach to identifying security threats, categorizing them into six types:
- **S**poofing: Impersonating something or someone
- **T**ampering: Modifying data or code
- **R**epudiation: Claiming to not have performed an action
- **I**nformation Disclosure: Exposing information to unauthorized individuals
- **D**enial of Service: Denying or degrading service to users
- **E**levation of Privilege: Gaining capabilities without proper authorization

The framework helps systematically identify threats and define appropriate countermeasures.

**Reference**: [Microsoft's STRIDE Threat Modeling](https://docs.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)

### 5. Resource Utilization
- **CPU Usage**: Maximum acceptable CPU utilization
- **Memory Usage**: Memory constraints and requirements
- **Network Bandwidth**: Required network capacity
- **Calculation Method**: Benchmarking, resource monitoring

**Framework Example**: USE Method (Utilization, Saturation, Errors)

The USE Method is a methodology for analyzing system performance and resource utilization problems by examining three metrics for every resource:
- **Utilization**: Percentage of time the resource was busy
- **Saturation**: Degree to which the resource has extra work queued
- **Errors**: Count of error events

This framework provides a systematic approach to resource analysis and capacity planning.

**Reference**: [Brendan Gregg's USE Method](http://www.brendangregg.com/usemethod.html)

## Documentation Framework

For each constraint, document:

1. **Requirement**: Clear statement of the constraint
2. **Measurement**: How it will be measured
3. **Target Value**: Specific, measurable target
4. **Current Value**: Current system performance
5. **Gap Analysis**: Difference between target and current
6. **Impact**: Business impact of not meeting the constraint
7. **Remediation Plan**: Steps to address gaps

## Examples

*Coming soon*

## Additional Non-Functional Areas

### 6. Maintainability
- **Code Complexity**: Cyclomatic complexity limits
- **Documentation**: Required documentation standards
- **Modularity**: Component coupling and cohesion metrics

**Framework Example**: Software Maintainability Index (MI)

The Maintainability Index is a composite metric that incorporates several metrics to give a quantitative indication of maintainability:

MI = 171 - 5.2 * ln(HV) - 0.23 * CC - 16.2 * ln(LOC)

Where:
- HV is Halstead Volume
- CC is Cyclomatic Complexity
- LOC is Lines of Code

**Reference**: [Carnegie Mellon Software Engineering Institute](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=10527)

### 7. Reliability
- **Mean Time Between Failures (MTBF)**: Average time between system failures
- **Mean Time To Repair (MTTR)**: Average time to fix a failure
- **Failure Rate**: Expected frequency of failures

**Framework Example**: Reliability Growth Models

Reliability Growth Models predict how software reliability improves during testing and debugging. The most common is the Software Reliability Growth Model (SRGM), which uses statistical distributions to model the failure process.

**Reference**: [IEEE Standard 1633-2016](https://standards.ieee.org/ieee/1633/6575/) - Recommended Practice on Software Reliability

## Sources

- [ISO/IEC 25010:2011](https://www.iso.org/standard/35733.html) - Systems and software Quality Requirements and Evaluation
- [Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/architecture/framework/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Google SRE Book](https://sre.google/sre-book/table-of-contents/)
- [Brendan Gregg's Systems Performance](http://www.brendangregg.com/systems-performance-2nd-edition-book.html)
- [NIST Special Publication 800-160](https://csrc.nist.gov/publications/detail/sp/800-160/vol-1/final) - Systems Security Engineering

[Back to Table of Contents](/README.md)