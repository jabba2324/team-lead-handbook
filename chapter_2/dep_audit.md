# Dependency Auditing

Modern software development relies heavily on third-party dependencies. While these dependencies accelerate development, they also introduce security risks through the software supply chain. Dependency auditing is the practice of continuously monitoring and evaluating these third-party components for vulnerabilities, license compliance, and malicious code.

## Understanding Supply Chain Attacks

Supply chain attacks target the less-secure elements in your software supply chain rather than attacking your application directly. These attacks exploit the trust relationship between developers and the third-party packages they use.

### Common Attack Vectors

1. **Typosquatting**: Publishing malicious packages with names similar to popular packages (e.g., "lodahs" instead of "lodash")
2. **Dependency Confusion**: Exploiting private package names by publishing public packages with the same name
3. **Maintainer Account Compromise**: Gaining access to package maintainer accounts to publish malicious updates
4. **Malicious Code Injection**: Adding malicious code to legitimate packages
5. **Abandoned Package Takeovers**: Taking over maintenance of abandoned packages and adding malicious code

## Notable Supply Chain Attack Examples

### 1. event-stream (2018)
**What Happened**: A malicious actor gained the trust of the original maintainer and took over the popular npm package. They then injected code that stole bitcoin wallet credentials from specific applications.

**Impact**: The compromised package had millions of weekly downloads and affected numerous downstream applications.

**Lesson**: Even well-established packages can be compromised if maintainer access is transferred.

### 2. SolarWinds (2020)
**What Happened**: Attackers compromised the build system of SolarWinds' Orion software and inserted a backdoor into updates.

**Impact**: Affected approximately 18,000 organizations, including multiple US government agencies and Fortune 500 companies.

**Lesson**: Build systems and update mechanisms are critical security points.

### 3. ua-parser-js (2021)
**What Happened**: The npm package ua-parser-js was compromised after the maintainer's account was hijacked. Malicious versions were published that installed cryptocurrency miners and password stealers.

**Impact**: The package had over 7 million weekly downloads and was used by major companies.

**Lesson**: Account security for package maintainers is crucial for ecosystem security.

### 4. colors.js and faker.js (2022)
**What Happened**: The developer intentionally sabotaged their own widely-used packages by releasing versions that created infinite loops and printed gibberish to the console.

**Impact**: Thousands of projects depending on these packages experienced unexpected behavior.

**Lesson**: Dependency on individual maintainers creates risk, even without malicious intent.

### 5. PyTorch Dependency Confusion (2022)
**What Happened**: An attacker published a malicious package named "torchtriton" to PyPI, which was imported by PyTorch's nightly build process due to dependency confusion.

**Impact**: The malicious package collected system information and sent it to a remote server.

**Lesson**: Private package names should be protected from public registries.

## Dependency Auditing Tools by Ecosystem

### JavaScript/Node.js

| Tool | Features | Integration |
|------|----------|-------------|
| [npm audit](https://docs.npmjs.com/cli/v8/commands/npm-audit) | Built-in vulnerability scanning | `npm audit` or `npm audit fix` |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance, automated fixes | CLI, GitHub integration, CI plugins |
| [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/) | Identifies project dependencies with known vulnerabilities | CLI, build plugins |
| [Dependabot](https://github.com/dependabot) | Automated dependency updates | GitHub integration |
| [Socket](https://socket.dev/) | Supply chain security, malicious behavior detection | GitHub app, CLI |

### Python

| Tool | Features | Integration |
|------|----------|-------------|
| [Safety](https://pyup.io/safety/) | Vulnerability scanning for Python packages | CLI, CI integration |
| [pip-audit](https://pypi.org/project/pip-audit/) | Audits Python environments for dependencies with known vulnerabilities | CLI |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance | CLI, GitHub integration |
| [PyUp](https://pyup.io/) | Security updates, vulnerability alerts | GitHub integration, CI |
| [Bandit](https://bandit.readthedocs.io/) | Finds common security issues in Python code | CLI, pre-commit hook |

### Java

| Tool | Features | Integration |
|------|----------|-------------|
| [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/) | Identifies project dependencies with known vulnerabilities | Maven/Gradle plugins |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance | CLI, Maven/Gradle plugins |
| [Sonatype Nexus IQ](https://www.sonatype.com/products/nexus-iq) | Component intelligence, policy enforcement | Maven/Gradle plugins, CI integration |
| [JFrog Xray](https://jfrog.com/xray/) | Security and license compliance | Artifactory integration |
| [Dependabot](https://github.com/dependabot) | Automated dependency updates | GitHub integration |

### Ruby

| Tool | Features | Integration |
|------|----------|-------------|
| [Bundler-audit](https://github.com/rubysec/bundler-audit) | Patch-level verification for Bundler | CLI, CI integration |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance | CLI, GitHub integration |
| [Dependabot](https://github.com/dependabot) | Automated dependency updates | GitHub integration |
| [Ruby Advisory DB](https://github.com/rubysec/ruby-advisory-db) | Database of vulnerable Ruby gems | Used by bundler-audit |

### .NET

| Tool | Features | Integration |
|------|----------|-------------|
| [NuGet Security Advisories](https://github.com/NuGet/NuGetGallery/blob/master/src/NuGetGallery/Areas/Admin/Views/SecurityPolicy/Index.cshtml) | Built-in vulnerability alerts | Visual Studio, NuGet CLI |
| [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/) | Identifies project dependencies with known vulnerabilities | MSBuild task |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance | CLI, Azure DevOps extension |
| [WhiteSource Bolt](https://www.whitesourcesoftware.com/free-developer-tools/bolt/) | Open source security and license compliance | Azure DevOps, GitHub |

### Go

| Tool | Features | Integration |
|------|----------|-------------|
| [Nancy](https://github.com/sonatype-nexus-community/nancy) | Checks dependencies for vulnerabilities | CLI, CI integration |
| [Snyk](https://snyk.io/) | Vulnerability scanning, license compliance | CLI, GitHub integration |
| [Dependabot](https://github.com/dependabot) | Automated dependency updates | GitHub integration |
| [OSV-Scanner](https://github.com/google/osv-scanner) | Vulnerability scanner using the OSV database | CLI |

### Docker/Container

| Tool | Features | Integration |
|------|----------|-------------|
| [Trivy](https://github.com/aquasecurity/trivy) | Comprehensive vulnerability scanner for containers | CLI, CI integration |
| [Clair](https://github.com/quay/clair) | Static analysis of vulnerabilities in container images | API, CI integration |
| [Docker Scan](https://docs.docker.com/engine/scan/) | Built-in vulnerability scanning | Docker CLI |
| [Snyk Container](https://snyk.io/product/container-vulnerability-management/) | Container vulnerability management | CLI, CI integration |

## Implementing Dependency Auditing in CI/CD

### GitHub Actions Example

```yaml
name: Dependency Audit

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 0'  # Run weekly

jobs:
  npm-audit:
    runs-on: ubuntu-latest
    if: contains(github.repository, 'javascript') || contains(github.repository, 'node')
    steps:
      - uses: actions/checkout@v2
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm ci
      - name: Run audit
        run: npm audit --audit-level=high

  python-audit:
    runs-on: ubuntu-latest
    if: contains(github.repository, 'python')
    steps:
      - uses: actions/checkout@v2
      - name: Setup Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pip-audit
      - name: Run audit
        run: pip-audit

  java-audit:
    runs-on: ubuntu-latest
    if: contains(github.repository, 'java') || contains(github.repository, 'maven')
    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '11'
          distribution: 'adopt'
      - name: Run OWASP Dependency-Check
        uses: dependency-check/Dependency-Check_Action@main
        with:
          project: 'My Project'
          path: '.'
          format: 'HTML'
          out: 'reports'
      - name: Upload report
        uses: actions/upload-artifact@v2
        with:
          name: dependency-check-report
          path: reports
```

### GitLab CI Example

```yaml
stages:
  - test

dependency_scanning:
  stage: test
  image: node:16
  script:
    - npm ci
    - npm audit --audit-level=high
  only:
    - main
    - merge_requests
  artifacts:
    reports:
      dependency_scanning: gl-dependency-scanning-report.json

container_scanning:
  stage: test
  image:
    name: aquasec/trivy:latest
    entrypoint: [""]
  script:
    - trivy image --format json --output gl-container-scanning-report.json $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  only:
    - main
    - merge_requests
  artifacts:
    reports:
      container_scanning: gl-container-scanning-report.json
```

### Jenkins Pipeline Example

```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Dependency Audit') {
            parallel {
                stage('NPM Audit') {
                    when {
                        expression { fileExists('package.json') }
                    }
                    steps {
                        sh 'npm ci'
                        sh 'npm audit --audit-level=high || true'
                    }
                }
                
                stage('OWASP Dependency-Check') {
                    steps {
                        dependencyCheck additionalArguments: '--scan ./ --format XML --out dependency-check-report.xml', odcInstallation: 'OWASP-Dependency-Check'
                        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                    }
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: '**/dependency-check-report.xml', allowEmptyArchive: true
        }
    }
}
```

## Best Practices for Dependency Management

### 1. Minimize Dependencies
- Use fewer dependencies to reduce attack surface
- Evaluate whether a small utility is worth the dependency
- Consider inlining small functions instead of adding dependencies

### 2. Vet Dependencies Before Adding
- Check package download statistics
- Review GitHub stars, contributors, and activity
- Examine open issues and pull requests
- Review the code for suspicious patterns

### 3. Pin Dependency Versions
- Use exact versions rather than ranges
- Use lockfiles (package-lock.json, Pipfile.lock, etc.)
- Consider using private registries with vetted packages

### 4. Implement Continuous Monitoring
- Run dependency scans on a schedule
- Set up alerts for new vulnerabilities
- Automate dependency updates with security reviews

### 5. Use Multiple Layers of Defense
- Combine dependency scanning with runtime protection
- Implement least privilege principles
- Use network segmentation to limit impact

### 6. Create a Dependency Update Policy
- Define when to update dependencies
- Establish processes for emergency updates
- Document exceptions for critical dependencies

## Mitigating Dependency Confusion Attacks

### 1. Use Private Registries
- Set up private package repositories
- Configure package managers to use private registries first
- Implement namespace protection

### 2. Scope Packages
- Use scoped packages in npm (@company/package-name)
- Use organization prefixes in other ecosystems

### 3. Verify Package Integrity
- Use checksums to verify package integrity
- Implement Subresource Integrity (SRI) for web resources
- Use signed packages where supported

### 4. Monitor for Typosquatting
- Regularly search for similarly named packages
- Consider registering common misspellings of your packages
- Use tools that detect typosquatting attempts

## Advanced Techniques

### 1. Software Bill of Materials (SBOM)
- Generate and maintain SBOMs for all applications
- Use formats like CycloneDX or SPDX
- Include SBOMs in release artifacts

### 2. Reproducible Builds
- Ensure builds are deterministic
- Verify build output matches expected checksums
- Use build systems that support reproducible builds

### 3. Runtime Application Self-Protection (RASP)
- Implement runtime protection against exploitation
- Monitor for suspicious behavior from dependencies
- Use sandboxing techniques for untrusted code

### 4. Dependency Firewall
- Implement proxies that scan packages before download
- Block packages with suspicious patterns
- Quarantine packages for manual review

## Sources

- [CISA: Software Supply Chain Security Guidance](https://www.cisa.gov/supply-chain)
- [NIST: Cybersecurity Supply Chain Risk Management](https://csrc.nist.gov/Projects/cyber-supply-chain-risk-management)
- [OWASP: Software Component Verification Standard](https://owasp.org/www-project-software-component-verification-standard/)
- [Sonatype State of the Software Supply Chain Report](https://www.sonatype.com/state-of-the-software-supply-chain)
- [Google's Supply Chain Levels for Software Artifacts (SLSA)](https://slsa.dev/)
- [npm Security Best Practices](https://docs.npmjs.com/security-best-practices/security-best-practices)
- [Securing the Software Supply Chain (Microsoft)](https://www.microsoft.com/en-us/securityengineering/opensource/securing-software-supply-chain)
- [Dependency Confusion: How I Hacked Into Apple, Microsoft and Dozens of Other Companies](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610) by Alex Birsan

[Back to Table of Contents](/README.md)