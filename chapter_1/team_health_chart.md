## Team Health Chart
The team health chart creates a snapshot of each team members perspective of the product/project across a number of topics. Each team member should annoymously submit a score between 1 to 3 and a short comment on why they gave the score. This can help you understand gaps in perspectives and oppertunities to alignment amonst the team.

The topics for a given team concernd with raising enfgineering standards might be:
* Solution Design
* Ease of implementation
* Clean code
* CI / CD pipelines
* Code vulnerabilities
* QA
* Test Automation
* Code Coverage
* Load Testing
* Perf testing
* API latency
* Monitoring services
* Logging
* Incident response
* Defect triaging 
* Defect resolution
* Branching strategy
* Clean repositories
* Easy of deploying 
* Release tagging
* Team ownership

You might have a scoring system like
| Score    | Description |
| -------- | ------- |
| 1 | Not happy AND OR needs a lot of work AND OR not started |
| 2 | Neither happy nor not happy AND OR we have progressed here but needs some work to get it to where we want it to be |
| 3 | Really happy AND OR this is the best it could be! |

### Example 
A completed submission from a team member might looks like this:

| Topic    | Score | Why |
| -------- | ------- | ------- |
|Solution Design|2|The solution will work but seems over engineered|
| Ease of implementation | 3 | Our choice of languages and frameworks are appropriate for easy development |
|Clean code |3|code is easy to read and well formatted|
|CI / CD pipelines| 3| We run linters and tests for PRs and automate deployments, additionally they could add OWASP 10 checks but not a huge deal|
|Test Automation|2| We have large test suites with high levels of coverage, we still need some higher level acceptance tests and load tests but thats being planned|

From here you can do some excel magic to understand alignment or disparity in the team and which topics to focus on.
### Source
Created by @jermaine-24x7 LinkedIn: @jgand3rson and insprited by Team Topologies