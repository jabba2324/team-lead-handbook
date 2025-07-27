# Load Testing

Load testing is the practice of simulating real-world load on software, applications, or websites to verify system behavior under normal and peak conditions. It helps identify performance bottlenecks, determine system capacity, and ensure reliability under expected workloads.

## Load Testing Tools Comparison

| Tool | Type | Protocol Support | Scripting Language | Cloud Support | Reporting | Best For |
|------|------|------------------|-------------------|--------------|-----------|----------|
| [Apache JMeter](https://jmeter.apache.org/) | Open Source | HTTP, HTTPS, SOAP, REST, FTP, JDBC, LDAP, WebSockets | Java, Groovy | Via plugins | Customizable dashboards | General-purpose testing, API testing |
| [Locust](https://locust.io/) | Open Source | HTTP/HTTPS, WebSockets | Python | Self-hosted on cloud | Real-time metrics, customizable | Developer-centric testing, microservices |
| [k6](https://k6.io/) | Open Source/Commercial | HTTP, WebSockets, gRPC | JavaScript | Native cloud integration | Detailed performance metrics | Modern web applications, CI/CD integration |
| [Gatling](https://gatling.io/) | Open Source/Commercial | HTTP, WebSockets, JMS, MQTT | Scala DSL | Via enterprise version | Comprehensive HTML reports | Continuous load testing, high-performance scenarios |
| [LoadRunner](https://www.microfocus.com/en-us/products/loadrunner-professional/overview) | Commercial | 50+ protocols | C-based VuGen, JavaScript | Yes | Advanced analytics | Enterprise applications, complex scenarios |
| [Artillery](https://artillery.io/) | Open Source | HTTP, WebSockets, Socket.io | JavaScript, YAML | Yes | Customizable reports | API and microservices testing |
| [Tsung](http://tsung.erlang-projects.org/) | Open Source | HTTP, WebDAV, SOAP, PostgreSQL, MySQL, LDAP | Erlang | Self-hosted | HTML reports with graphs | High-scalability testing |
| [Vegeta](https://github.com/tsenart/vegeta) | Open Source | HTTP | Command-line | Self-hosted | Text and plot formats | HTTP service testing, constant-rate load testing |

## Example Load Test Plan

**Application**: E-commerce Website

**Test Objectives**:
- Verify the system can handle 10,000 concurrent users
- Ensure response times remain under 2 seconds at peak load
- Identify performance bottlenecks in the checkout process
- Determine maximum throughput before degradation

**Test Scenarios**:

1. **Browse Products**
   - Users browse product categories
   - Users search for specific products
   - Users filter and sort product listings
   - Think time: 5-15 seconds between actions

2. **Shopping Cart Operations**
   - Users add items to cart
   - Users update quantities
   - Users remove items
   - Think time: 3-8 seconds between actions

3. **Checkout Process**
   - Users proceed to checkout
   - Users enter shipping information
   - Users enter payment details
   - Users complete purchase
   - Think time: 10-30 seconds between actions

4. **User Account**
   - Users log in
   - Users view order history
   - Users update account information
   - Think time: 5-10 seconds between actions

**Load Profile**:

| Phase | Duration | Users | Ramp-up Time |
|-------|----------|-------|-------------|
| Warm-up | 10 minutes | 0 → 1,000 | 10 minutes |
| Steady state | 30 minutes | 1,000 | N/A |
| Ramp-up | 20 minutes | 1,000 → 5,000 | 20 minutes |
| Peak load | 30 minutes | 5,000 | N/A |
| Stress test | 15 minutes | 5,000 → 10,000 | 15 minutes |
| Cool-down | 10 minutes | 10,000 → 0 | 10 minutes |

**Test Data Requirements**:

| Data Type | Quantity | Notes |
|-----------|----------|-------|
| User accounts | 20,000 | Pre-created with various history patterns |
| Product catalog | 10,000 items | Across 50 categories with varied pricing |
| Payment methods | 5 types | Credit card, PayPal, gift cards, etc. |
| Shipping options | 3 options | Standard, express, next-day |
| Discount codes | 20 codes | Various discount types and amounts |

**Monitoring Points**:
- Web server response times
- Database query execution times
- Memory utilization
- CPU utilization
- Network throughput
- Error rates
- Cache hit/miss ratios

**Success Criteria**:
- 95th percentile response time < 2 seconds at 5,000 concurrent users
- 99th percentile response time < 4 seconds at 5,000 concurrent users
- Error rate < 0.1% under all conditions
- No resource saturation below 7,500 concurrent users

**Example JMeter Script Structure**:
```xml
<jmeterTestPlan version="1.2" properties="5.0">
  <hashTree>
    <TestPlan guiclass="TestPlanGui" testname="E-commerce Load Test">
      <ThreadGroup guiclass="ThreadGroupGui" testname="Shoppers">
        <stringProp name="ThreadGroup.num_threads">5000</stringProp>
        <stringProp name="ThreadGroup.ramp_time">1800</stringProp>
        <elementProp name="ThreadGroup.main_controller" elementType="LoopController">
          <stringProp name="LoopController.loops">10</stringProp>
        </elementProp>
      </ThreadGroup>
      <hashTree>
        <CookieManager/>
        <HTTPSamplerProxy guiclass="HttpTestSampleGui" testname="Home Page">
          <stringProp name="HTTPSampler.domain">example.com</stringProp>
          <stringProp name="HTTPSampler.path">/</stringProp>
          <stringProp name="HTTPSampler.method">GET</stringProp>
        </HTTPSamplerProxy>
        <GaussianRandomTimer guiclass="GaussianRandomTimerGui">
          <stringProp name="ConstantTimer.delay">5000</stringProp>
          <stringProp name="RandomTimer.range">2000</stringProp>
        </GaussianRandomTimer>
        <!-- Additional samplers for other user journeys -->
      </hashTree>
    </hashTree>
  </hashTree>
</jmeterTestPlan>
```

**Reporting Example**:

```
Summary Report:
- Samples: 1,245,632
- Average Response Time: 1.2s
- 90% Line: 1.8s
- 95% Line: 2.1s
- 99% Line: 3.5s
- Min: 0.1s
- Max: 8.2s
- Error Rate: 0.05%
- Throughput: 694.2/sec
```

**Key Performance Insights**:
- System performed within acceptable parameters up to 7,500 concurrent users
- Database connection pool saturation occurred at 8,200 users
- Product search response times degraded significantly above 6,000 users
- Checkout process maintained acceptable performance throughout all test phases
- Payment processing API became a bottleneck during peak load
- Caching improvements could significantly enhance browse performance

## Sources

- [Apache JMeter User Manual](https://jmeter.apache.org/usermanual/index.html)
- [Microsoft Performance Testing Guidance](https://docs.microsoft.com/en-us/azure/architecture/framework/scalability/performance-test)
- [Web Performance Testing with k6](https://k6.io/docs/)
- [The Art of Application Performance Testing](https://www.oreilly.com/library/view/the-art-of/9781491900536/) by Ian Molyneaux

[Back to Table of Contents](/README.md)