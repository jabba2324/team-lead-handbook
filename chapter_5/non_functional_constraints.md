# Calculating Non-Functional/Low-Level Constraints

Non-functional requirements define the quality attributes and constraints of a system. Properly calculating and documenting these constraints is essential for system design and technical debt management.

## Key Non-Functional Areas

### 1. Performance
- **Response Time**: Maximum acceptable time for system response
- **Throughput**: Required transactions per second
- **Latency**: Acceptable delay in data processing
- **Calculation Method**: Load testing with representative data volumes

### 2. Scalability
- **Concurrent Users**: Maximum number of simultaneous users
- **Data Volume**: Growth projections for data storage
- **Request Rate**: Peak request rates and growth projections
- **Calculation Method**: Capacity planning models, growth forecasting

### 3. Availability
- **Uptime Requirements**: Expressed as percentage (e.g., 99.9%)
- **Planned Downtime**: Maintenance windows and frequency
- **Recovery Time Objective (RTO)**: Maximum acceptable downtime
- **Calculation Method**: Business impact analysis, SLA requirements

### 4. Security
- **Authentication Requirements**: Methods and strength
- **Authorization Levels**: Access control granularity
- **Data Protection**: Encryption requirements
- **Calculation Method**: Threat modeling, compliance requirements

### 5. Resource Utilization
- **CPU Usage**: Maximum acceptable CPU utilization
- **Memory Usage**: Memory constraints and requirements
- **Network Bandwidth**: Required network capacity
- **Calculation Method**: Benchmarking, resource monitoring

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

## Sources

- [ISO/IEC 25010:2011](https://www.iso.org/standard/35733.html) - Systems and software Quality Requirements and Evaluation
- [Microsoft Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/architecture/framework/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

[Back to Table of Contents](/README.md)