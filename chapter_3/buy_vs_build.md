# Buy vs Build

The decision to buy an existing solution or build a custom one is a common challenge for technology teams. This framework helps evaluate the tradeoffs.

## Decision Framework

### Buy When:

- **Core vs. Context**: The functionality is not core to your business differentiation
- **Time to Market**: You need to implement quickly
- **Resource Constraints**: Your team lacks specialized expertise or bandwidth
- **Standardization**: The problem is well-understood with established solutions
- **Cost Efficiency**: TCO of buying is lower than building and maintaining
- **Maintenance**: You want to offload maintenance and updates

### Build When:

- **Competitive Advantage**: The functionality provides unique business value
- **Specific Requirements**: Off-the-shelf solutions don't meet your specific needs
- **Integration Complexity**: Custom integration would be more complex than building
- **Long-term Control**: You need complete control over the roadmap
- **Intellectual Property**: You want to own the IP for strategic reasons
- **Cost at Scale**: At your scale, building becomes more cost-effective

## Evaluation Matrix

Create a weighted decision matrix with the following factors:

1. **Strategic Alignment**: How well does the solution align with business strategy?
2. **Time to Value**: How quickly can you realize benefits?
3. **Total Cost of Ownership**: Initial and ongoing costs
4. **Risk**: Technical, operational, and business risks
5. **Flexibility**: Ability to adapt to changing requirements
6. **Integration**: Ease of integration with existing systems
7. **Support & Maintenance**: Resources required for ongoing support

## Examples

### Case Study 1: Spotify's Decision to Build Backstage

Spotify faced challenges with developer productivity and maintaining consistency across hundreds of microservices. They evaluated existing developer portal solutions but ultimately decided to build their own tool, Backstage.

**Buy Considerations:**
- Several commercial developer portals existed
- Would have provided faster initial implementation
- Vendor would handle maintenance and updates

**Build Considerations:**
- Needed deep integration with Spotify's unique microservice architecture
- Required flexibility to support Spotify's specific workflows
- Wanted to create a platform that could evolve with their needs

**Decision:** Build, then open-source. Spotify built Backstage internally, then later open-sourced it to gain community contributions while maintaining control over the core platform.

**Outcome:** Backstage became a successful CNCF project used by many companies, validating their decision to build. The platform unified Spotify's tooling ecosystem and significantly improved developer productivity.

**Source:** [Spotify Engineering Blog: What the Heck is Backstage Anyway?](https://engineering.atspotify.com/2020/03/what-the-heck-is-backstage-anyway/)

### Case Study 2: Lyft's Payment Processing Decision

Lyft needed to decide whether to build their own payment processing system or use a third-party solution like Stripe or Braintree.

**Buy Considerations:**
- Payment providers offered ready-to-use solutions
- Compliance (PCI) handled by the provider
- Lower initial engineering investment

**Build Considerations:**
- High transaction volume meant significant processing fees
- Custom routing between payment processors could optimize costs
- Specific needs for driver payouts and rider payments

**Decision:** Hybrid approach. Lyft built a payment orchestration layer that could route transactions between different payment providers while using third-party processors for the actual transactions.

**Outcome:** This approach gave Lyft flexibility to optimize costs and reliability while avoiding building the most complex and regulated parts of the payment infrastructure.

**Source:** [Lyft Engineering Blog: Building a Payment Platform](https://eng.lyft.com/building-a-payment-platform-part-1-7a41a34a4c43)

### Example: Buy vs. Build Analysis for Customer Support Ticketing System

#### Scenario
A growing e-commerce company with 50 customer support agents needs to implement a ticketing system to manage customer inquiries. The team is evaluating whether to build a custom solution integrated with their e-commerce platform or purchase an established SaaS ticketing system.

#### Weighted Decision Matrix

| Factor | Weight | Buy (SaaS Solution) | Build Custom Solution |
|--------|--------|---------------------|----------------------|
| **Strategic Alignment** | 15% | 7/10<br>Meets most needs but lacks deep integration with proprietary inventory system | 9/10<br>Can be tailored to specific business processes |
| **Time to Value** | 20% | 9/10<br>Can be implemented in 2-4 weeks | 3/10<br>Estimated 4-6 months development time |
| **Total Cost of Ownership (3 years)** | 20% | 6/10<br>$120K ($100/agent/month + implementation) | 5/10<br>$180K (development + maintenance) |
| **Risk** | 15% | 8/10<br>Established solution with proven reliability | 5/10<br>Potential for bugs, delays, and resource constraints |
| **Flexibility** | 10% | 5/10<br>Limited to vendor's roadmap and customization options | 9/10<br>Can evolve with changing business requirements |
| **Integration** | 15% | 6/10<br>Has APIs but requires custom work for deep integration | 9/10<br>Can be built with native integration to all systems |
| **Support & Maintenance** | 5% | 9/10<br>Vendor handles updates, security, and scaling | 4/10<br>Requires dedicated internal resources |
| **Weighted Score** | 100% | **7.15/10** | **6.15/10** |

#### Cost Breakdown

**Buy Option:**
- Subscription: $100/agent/month × 50 agents × 36 months = $180,000
- Implementation and training: $20,000
- Integration development: $40,000
- Ongoing administration (part-time): $60,000
- **Total 3-year TCO: $300,000**

**Build Option:**
- Initial development (5 developers × 4 months): $200,000
- Infrastructure costs: $30,000
- Ongoing maintenance and enhancements: $120,000
- **Total 3-year TCO: $350,000**

#### Decision Factors Beyond the Matrix

1. **Team Focus:** Building would divert engineering resources from core product development
2. **Expertise:** The team lacks experience in building support systems
3. **Future Growth:** The SaaS solution can easily scale to 200+ agents
4. **Competitive Advantage:** The ticketing system is not a core differentiator for the business

#### Final Recommendation

Buy the SaaS solution. While building would offer better customization and integration, the significant time-to-market advantage, lower risk, and ability to keep engineering resources focused on core product development make buying the more strategic choice. The company should allocate some of the saved development time to creating solid integrations between the SaaS solution and their proprietary systems.

## Sources

- [Harvard Business Review: When to Build vs. Buy](https://hbr.org/2021/01/when-to-build-and-when-to-buy)
- [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar)

[Back to Table of Contents](/README.md)