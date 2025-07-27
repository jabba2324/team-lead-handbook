# DORA Metrics

DORA (DevOps Research and Assessment) metrics are a set of measurements that indicate the performance of software development teams. They were established through research conducted by the DORA team at Google and are widely recognized as key indicators of DevOps performance.

## The Four Key Metrics

### 1. Deployment Frequency
**Definition**: How often an organization successfully releases to production.

**Performance Levels**:
- **Elite**: Multiple deployments per day
- **High**: Between once per day and once per week
- **Medium**: Between once per week and once per month
- **Low**: Between once per month and once every six months

**Measurement**: Count of production deployments over time period.

### 2. Lead Time for Changes
**Definition**: The time it takes for a commit to reach production.

**Performance Levels**:
- **Elite**: Less than one day
- **High**: Between one day and one week
- **Medium**: Between one week and one month
- **Low**: Between one month and six months

**Measurement**: Time from code commit to code successfully running in production.

### 3. Mean Time to Restore (MTTR)
**Definition**: How long it takes to restore service when an incident or defect impacts users.

**Performance Levels**:
- **Elite**: Less than one hour
- **High**: Less than one day
- **Medium**: Less than one week
- **Low**: More than one week

**Measurement**: Time from incident detection to remediation.

### 4. Change Failure Rate
**Definition**: The percentage of changes that result in degraded service or require remediation.

**Performance Levels**:
- **Elite**: 0-15%
- **High**: 16-30%
- **Medium**: 31-45%
- **Low**: 46-60%

**Measurement**: Number of deployment failures divided by total number of deployments.

## Implementation

### Data Collection
- **Deployment Frequency**: CI/CD pipeline logs
- **Lead Time**: Version control system + deployment logs
- **MTTR**: Incident management system
- **Change Failure Rate**: Incident management + deployment logs

### Visualization
- Trend charts showing metrics over time
- Comparison against industry benchmarks
- Team-level dashboards

### Improvement Strategies
- **Deployment Frequency**: Smaller batch sizes, feature flags, automated testing
- **Lead Time**: Reduce approval steps, automate manual processes
- **MTTR**: Better monitoring, standardized incident response
- **Change Failure Rate**: Improved testing, progressive deployments

## Examples

*Coming soon*

## Sources
- [DORA State of DevOps Reports](https://www.devops-research.com/research.html)
- [Google Cloud - Four Keys Project](https://github.com/GoogleCloudPlatform/fourkeys)
- [Accelerate: The Science of Lean Software and DevOps](https://itrevolution.com/book/accelerate/) by Nicole Forsgren, Jez Humble, and Gene Kim

[Back to Table of Contents](/README.md)