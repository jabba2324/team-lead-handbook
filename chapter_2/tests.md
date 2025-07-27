# Test Coverage

Test coverage is a measure of how much of your code is executed during testing. It helps identify untested code paths and assess the thoroughness of your test suite.

## Types of Coverage

### 1. Statement Coverage
- **Definition**: Percentage of code statements executed during tests
- **Pros**: Easy to understand and measure
- **Cons**: Doesn't account for branch conditions

### 2. Branch Coverage
- **Definition**: Percentage of branches (if/else, switch cases) executed
- **Pros**: More thorough than statement coverage
- **Cons**: Doesn't ensure all combinations of conditions are tested

### 3. Path Coverage
- **Definition**: Percentage of possible paths through the code executed
- **Pros**: Most comprehensive coverage metric
- **Cons**: Exponential complexity, often impractical for full coverage

### 4. Function Coverage
- **Definition**: Percentage of functions/methods called during tests
- **Pros**: Good high-level indicator
- **Cons**: Doesn't reveal internal function complexity

## Coverage Tools

| Language | Tools | Features |
|----------|-------|----------|
| JavaScript | [Istanbul/NYC](https://istanbul.js.org/), [Jest](https://jestjs.io/) | Statement, branch, function coverage |
| Python | [Coverage.py](https://coverage.readthedocs.io/), [pytest-cov](https://pytest-cov.readthedocs.io/) | Statement, branch coverage |
| Java | [JaCoCo](https://www.eclemma.org/jacoco/), [Cobertura](https://cobertura.github.io/cobertura/) | Line, branch, complexity coverage |
| C# | [Coverlet](https://github.com/coverlet-coverage/coverlet), [NCover](https://www.ncover.com/) | Line, branch, method coverage |
| Go | [Go Cover](https://golang.org/pkg/cmd/cover/) | Statement coverage |
| Ruby | [SimpleCov](https://github.com/simplecov-ruby/simplecov) | Line, branch coverage |

## Setting Coverage Targets

### Factors to Consider
- **Project Type**: Different applications need different coverage levels
- **Risk Profile**: Critical systems need higher coverage
- **Resource Constraints**: Balance coverage with development velocity
- **Code Complexity**: Complex code needs more thorough testing

### Recommended Targets
- **Critical Systems**: 90%+ statement and branch coverage
- **Business Applications**: 70-80% statement coverage
- **Utility Libraries**: 80-90% statement coverage
- **UI Components**: 60-70% statement coverage

## Best Practices

- **Focus on Quality**: Coverage is a means, not an end
- **Prioritize Critical Paths**: Ensure business-critical code has high coverage
- **Don't Chase 100%**: Diminishing returns after certain thresholds
- **Review Uncovered Code**: Understand why code isn't covered
- **Include Coverage in CI**: Fail builds if coverage drops below thresholds
- **Combine with Other Metrics**: Use with code quality and complexity metrics

## Examples

*Coming soon*

## Sources
- [Martin Fowler on Test Coverage](https://martinfowler.com/bliki/TestCoverage.html)
- [Kent Beck's Test-Driven Development](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)
- [Google Testing Blog](https://testing.googleblog.com/)

[Back to Table of Contents](/README.md)