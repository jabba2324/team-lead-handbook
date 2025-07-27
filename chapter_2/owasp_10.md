# OWASP Top 10

The OWASP (Open Web Application Security Project) Top 10 is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications.

## What is OWASP?

OWASP is a nonprofit foundation that works to improve the security of software. The OWASP Top 10 is updated periodically to reflect the changing landscape of web application security threats.

## The OWASP Top 10 (2021)

### 1. Broken Access Control
**Description**: Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data.

**Examples**:
- Modifying URLs, internal application state, or HTML page to elevate privileges
- Accessing APIs with missing access controls
- Metadata manipulation (e.g., JWT tokens, cookies)

**Prevention**:
- Implement proper access controls with deny by default
- Enforce record ownership
- Disable directory listing
- Log access control failures

### 2. Cryptographic Failures
**Description**: Failures related to cryptography that often lead to exposure of sensitive data.

**Examples**:
- Transmitting sensitive data in clear text
- Using weak or outdated cryptographic algorithms
- Using default or weak keys

**Prevention**:
- Encrypt all sensitive data at rest and in transit
- Use strong, up-to-date algorithms and protocols
- Disable caching for sensitive data
- Store passwords using strong adaptive hashing functions

### 3. Injection
**Description**: User-supplied data is not validated, filtered, or sanitized by the application. Common injections include SQL, NoSQL, OS command, and LDAP injection.

**Examples**:
- SQL injection: `SELECT * FROM accounts WHERE username='${username}'`
- Command injection: `os.system('ping ' + user_supplied_ip)`

**Prevention**:
- Use parameterized queries
- Validate and sanitize all user input
- Use ORMs and prepared statements
- Implement least privilege principle

### 4. Insecure Design
**Description**: Flaws in the design and architecture of applications that cannot be fixed by perfect implementation.

**Examples**:
- Missing business logic validation
- Allowing unlimited operations within a time period
- Insufficient protection against automated attacks

**Prevention**:
- Establish secure design patterns
- Use threat modeling for critical operations
- Integrate security requirements into user stories
- Implement secure development lifecycle

### 5. Security Misconfiguration
**Description**: Improper implementation of controls intended to keep application data safe.

**Examples**:
- Unnecessary features enabled
- Default accounts with unchanged passwords
- Error messages containing sensitive information
- Missing security headers

**Prevention**:
- Automated deployment process with security checks
- Minimal platform without unnecessary features
- Regular security review and updates
- Segmented application architecture

### 6. Vulnerable and Outdated Components
**Description**: Using components with known vulnerabilities can undermine application defenses.

**Examples**:
- Outdated libraries and frameworks
- Unsupported operating systems
- Unpatched vulnerabilities in dependencies

**Prevention**:
- Remove unused dependencies
- Continuously inventory versions of components
- Monitor for vulnerabilities in dependencies
- Obtain components from official sources

### 7. Identification and Authentication Failures
**Description**: Confirmation of the user's identity, authentication, and session management is often implemented incorrectly.

**Examples**:
- Credential stuffing
- Brute force attacks
- Weak password policies
- Missing multi-factor authentication

**Prevention**:
- Implement multi-factor authentication
- Limit failed login attempts
- Use strong password policies
- Implement proper session management

### 8. Software and Data Integrity Failures
**Description**: Code and infrastructure that does not protect against integrity violations.

**Examples**:
- Unsigned auto-updates
- Insecure CI/CD pipelines
- Using libraries from untrusted sources

**Prevention**:
- Use digital signatures for code
- Verify integrity of dependencies
- Ensure CI/CD pipelines have proper security controls
- Review code and configuration changes

### 9. Security Logging and Monitoring Failures
**Description**: Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response.

**Examples**:
- Auditable events not logged
- Logs not monitored for suspicious activity
- Alerts absent or ineffective

**Prevention**:
- Log all authentication, access control, and input validation failures
- Ensure logs are in a format suitable for consumption
- Establish effective monitoring and alerting
- Develop incident response and recovery plans

### 10. Server-Side Request Forgery (SSRF)
**Description**: SSRF flaws occur when a web application fetches a remote resource without validating the user-supplied URL.

**Examples**:
- Accessing internal services behind firewalls
- Accessing cloud services metadata
- Port scanning internal networks

**Prevention**:
- Sanitize and validate all client-supplied input data
- Enforce URL schema, port, and destination with a positive allow list
- Disable HTTP redirections
- Use network segmentation and firewall rules

## Security Scanning Tools

### Static Application Security Testing (SAST)

| Tool | Languages/Platforms | Features |
|------|---------------------|----------|
| [SonarQube](https://www.sonarqube.org/) | Java, JavaScript, C#, Python, PHP, and more | Code quality and security issues, technical debt, test coverage |
| [Checkmarx](https://www.checkmarx.com/) | 25+ languages including Java, .NET, JavaScript | Identifies vulnerabilities with remediation guidance |
| [Fortify](https://www.microfocus.com/en-us/products/static-code-analysis-sast/overview) | 25+ languages | Enterprise-grade SAST with integration into CI/CD |
| [Snyk](https://snyk.io/) | JavaScript, Java, Python, Ruby, Go, PHP | Code and open source dependency scanning |
| [ESLint](https://eslint.org/) with security plugins | JavaScript | Linting with security rules |
| [Bandit](https://github.com/PyCQA/bandit) | Python | Finds common security issues in Python code |
| [SpotBugs](https://spotbugs.github.io/) with FindSecBugs | Java | Identifies potential security vulnerabilities |

### Dynamic Application Security Testing (DAST)

| Tool | Type | Features |
|------|------|----------|
| [OWASP ZAP](https://www.zaproxy.org/) | Open Source | Web app scanner, intercepting proxy |
| [Burp Suite](https://portswigger.net/burp) | Commercial/Free Community Edition | Web vulnerability scanner, intercepting proxy |
| [Acunetix](https://www.acunetix.com/) | Commercial | Automated web vulnerability scanning |
| [Netsparker](https://www.netsparker.com/) | Commercial | Web application security scanner |

### Software Composition Analysis (SCA)

| Tool | Features |
|------|----------|
| [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/) | Identifies project dependencies with known vulnerabilities |
| [WhiteSource](https://www.whitesourcesoftware.com/) | Open source security, license compliance management |
| [Black Duck](https://www.synopsys.com/software-integrity/security-testing/software-composition-analysis.html) | Open source risk management |
| [Snyk](https://snyk.io/) | Dependency vulnerability scanning |

### Infrastructure as Code (IaC) Security

| Tool | Platforms | Features |
|------|-----------|----------|
| [Checkov](https://www.checkov.io/) | Terraform, CloudFormation, Kubernetes | Scans cloud infrastructure configurations |
| [Terrascan](https://github.com/accurics/terrascan) | Terraform, Kubernetes, Helm | Detects compliance and security violations |
| [tfsec](https://github.com/aquasecurity/tfsec) | Terraform | Security scanner for Terraform code |

## Integration into Development Workflow

### CI/CD Integration
- Integrate security scanning into your CI/CD pipeline
- Fail builds on critical security issues
- Generate security reports as part of the build process

### Developer Tools
- IDE plugins for real-time security feedback
- Pre-commit hooks for local scanning
- Pull request integration for code review

### Security as Code
- Define security policies as code
- Automate compliance checking
- Version control security configurations

## Examples

Many organizations have implemented OWASP Top 10 scanning as part of their secure development lifecycle:

- Netflix uses a combination of custom and open-source tools integrated into their CI/CD pipeline
- Microsoft's SDL includes multiple scanning tools at different stages of development
- Google's security scanning infrastructure runs continuously across their codebase

## Sources

- [OWASP Top 10:2021](https://owasp.org/Top10/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/Projects/ssdf)
- [SANS Software Security](https://www.sans.org/software-security/)
- [Snyk State of Open Source Security Report](https://snyk.io/open-source-security-report/)

[Back to Table of Contents](/README.md)