1. What is DevSecOps?

DevSecOps = Development + Security + Operations.

DevSecOps means integrating security throughout the entire Software Development Life Cycle (SDLC) instead of performing security testing only at the end.

The main goal is to identify and fix security vulnerabilities as early as possible, automate security checks in the CI/CD pipeline, enforce security policies, and continuously monitor the production environment.

Interview answer

“DevSecOps is an approach where security is integrated into every stage of the software development lifecycle. Instead of treating security as a final manual step, we automate security testing in the CI/CD pipeline.

For example, we perform SAST for source code, SCA for third-party dependencies, secret scanning, IaC scanning, container image scanning, and DAST for running applications. We also implement security gates so that critical vulnerabilities can prevent deployment. After deployment, we continuously monitor the application and infrastructure for security threats.”

2. What do you do in DevSecOps on a daily basis?

This is an important real-world interview question.

You can answer:

“On a daily basis, I work on integrating and maintaining security controls in the CI/CD pipeline. I check source-code vulnerabilities, dependency vulnerabilities, leaked secrets, infrastructure configuration, and container-image vulnerabilities. I also maintain security gates and monitor production for security events.”

Then explain your activities:

1. Secure the source code

I run SAST — Static Application Security Testing.

It analyzes source code without executing the application and identifies things such as:

SQL injection
XSS
insecure coding practices
hardcoded credentials
vulnerable code patterns
code smells and bugs

Tools: SonarQube, Checkmarx.

2. Perform SCA

SCA = Software Composition Analysis.

Modern applications use many third-party libraries and packages.

SCA checks whether those dependencies contain known vulnerabilities.

For example:

Application
   ↓
Node.js
   ↓
express
   ↓
some vulnerable package

If a dependency has a known CVE, SCA can report it.

Tools:

Snyk
OWASP Dependency-Check
3. Scan for secrets

I scan the repository and CI/CD environment for accidentally committed secrets.

Examples:

AWS Access Key
Password
API Token
Private Key
Database Credential

Tools:

Gitleaks
GitGuardian

For example:

Developer → Git Push
              ↓
        Secret Scanner
              ↓
       Secret detected
              ↓
       Pipeline FAILED
4. Secure Infrastructure as Code

If we use Terraform, Kubernetes YAML, CloudFormation, etc., I scan the configuration before deployment.

For example, Terraform might accidentally create:

Security Group
Port 22
Source: 0.0.0.0/0

That is a security risk.

Tools:

Checkov
Trivy
5. Scan container images

After building a Docker image, I scan it for vulnerabilities.

Example:

docker build -t myapp:v1 .

Then:

trivy image myapp:v1

It can identify vulnerable:

OS packages
libraries
application dependencies
CVEs

If critical vulnerabilities are found, the pipeline can fail.

6. Perform DAST

DAST = Dynamic Application Security Testing.

Unlike SAST, DAST tests the running application.

For example:

Deploy application
       ↓
Application running
       ↓
OWASP ZAP
       ↓
Security testing
       ↓
Vulnerabilities

Tools:

OWASP ZAP
Burp Suite
7. Implement security gates

This is one of the most important DevSecOps responsibilities.

For example:

Code
 ↓
Build
 ↓
SAST
 ↓
SCA
 ↓
Secret Scan
 ↓
IaC Scan
 ↓
Container Scan
 ↓
Security Gate
 ↓
Deploy

Suppose Trivy finds a critical CVE.

Then:

Critical vulnerability
        ↓
Security Gate
        ↓
Pipeline FAILED
        ↓
Deployment BLOCKED

This prevents insecure code from reaching production.

3. Example DevSecOps CI/CD pipeline

You can explain your pipeline like this in an interview:

Developer
    ↓
GitHub / GitLab
    ↓
Checkout Code
    ↓
SAST
SonarQube
    ↓
SCA
Snyk / Dependency-Check
    ↓
Secret Scan
Gitleaks
    ↓
Build Application
    ↓
Docker Build
    ↓
Container Scan
Trivy
    ↓
IaC Scan
Checkov / Trivy
    ↓
Security Gate
    ↓
Push Image to Registry
    ↓
Deploy to Kubernetes
    ↓
DAST
OWASP ZAP
    ↓
Production
    ↓
Falco / Monitoring
4. Your main DevSecOps tools

Your list can be organized like this:

Security Area	Tools
SAST / Code Security	SonarQube, Checkmarx
SCA / Dependencies	Snyk, OWASP Dependency-Check
Secret Detection	Gitleaks, GitGuardian
IaC Security	Checkov, Trivy
Container Security	Trivy, Grype
DAST	OWASP ZAP, Burp Suite
CI/CD	Jenkins, GitHub Actions, GitLab CI/CD
Security Policy	OPA
Production Runtime Security	Falco
Vulnerability Management	DefectDojo, Snyk
Your main tools

If the interviewer asks "Which DevSecOps tools have you worked with?", you can say:

“My primary DevSecOps tools are SonarQube for code quality and SAST, Trivy for container and IaC scanning, Gitleaks for secret detection, OWASP ZAP for DAST, and Falco for runtime security monitoring. I integrate these tools with Jenkins or GitHub Actions to automate security checks in the CI/CD pipeline.”

5. Important DevSecOps concepts
Shift Left Security

Security testing is moved earlier in the SDLC.

Instead of:

Develop → Build → Deploy → Security Test

we do:

Develop → Security Test → Build → Deploy

The earlier we detect a vulnerability, the cheaper and easier it is to fix.

Shift Right Security

Security doesn't stop after deployment.

We continuously monitor production.

Development
     ↓
CI/CD Security
     ↓
Production
     ↓
Runtime Monitoring
     ↓
Incident Detection

Falco can be used for runtime threat detection in Kubernetes environments.

Defense in Depth

Don't depend on a single security mechanism.

Use multiple layers:

Developer Security
       ↓
SAST
       ↓
SCA
       ↓
Secret Scanning
       ↓
IaC Security
       ↓
Container Security
       ↓
Network Security
       ↓
Runtime Security

If one layer fails, another layer can still protect the environment.

Least Privilege

Give users, applications and services only the permissions they actually need.

For example, don't give an application:

AdministratorAccess

if it only needs to read from an S3 bucket.

Zero Trust

The basic principle is:

Never automatically trust; always verify.

Access should be based on identity, permissions, context and policy rather than simply trusting something because it is inside the network.

Security Gate

A security gate determines whether the pipeline can continue.

Example:

Trivy Scan
    ↓
Critical CVE found?
    ↓
YES → FAIL PIPELINE
NO  → Continue Deployment
Threat Modeling

Before implementing an application, we identify:

What can go wrong?
Who can attack the application?
What assets need protection?
What attack paths exist?
What controls can reduce the risk?
Attack Surface

The attack surface is the collection of possible entry points an attacker could use.

Examples:

APIs
open ports
web applications
exposed cloud services
containers
databases
public S3 buckets
credentials/secrets

The goal is to minimize unnecessary attack surface.

6. CIA Triad

CIA = Confidentiality + Integrity + Availability

Confidentiality

Only authorized users should access information.

Example:

IAM
Encryption
Access Control
Integrity

Data should not be modified without authorization.

Example:

Hashing
Digital Signatures
File Integrity Monitoring
Availability

Applications and services should remain available when required.

Example:

High Availability
Auto Scaling
Load Balancing
Disaster Recovery
⭐ Strong interview answer: "Explain DevSecOps in your project"

You can give this answer:

“In my project, I integrated security into the CI/CD pipeline rather than performing security testing manually at the end. Whenever a developer pushes code to Git, the pipeline starts automatically.

First, I run SonarQube for code quality and SAST analysis. Then I perform SCA using tools such as Snyk or OWASP Dependency-Check to identify vulnerable third-party dependencies. I also run Gitleaks to make sure no credentials or secrets have been committed.

If we use Terraform or Kubernetes manifests, I scan the IaC using Checkov or Trivy. After that, I build the Docker image and scan it using Trivy for OS package and dependency vulnerabilities.

Based on the vulnerability severity and organizational policies, I implement security gates. For example, if a critical vulnerability is detected, the pipeline fails and deployment is blocked.

After successful security checks, the image is pushed to the container registry and deployed to Kubernetes. For the running application, I can use OWASP ZAP for DAST. In production, runtime security and monitoring can be handled using tools such as Falco along with the existing monitoring stack.

This approach helps us follow shift-left security, reduce security risks early, and make security a shared responsibility between development, security and operations teams.”

One correction to your notes: “thread modeling” should be threat modeling, and “Mange vulnerabilities” should be manage vulnerabilities.