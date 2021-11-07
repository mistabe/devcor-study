# Application Deployment and Security

## Heading 2

### 4.1 Diagnose a CI/CD pipeline failure (such as missing dependency, incompatible versions of components, and failed tests)

CI/CD (Continuous Integration/Continuous Delivery) pipelines are a series of steps that must be peformed to deliver a new version of an application.

CI means that code changes are made to the app and regularly built, tested and merged to a shared repository.

CD automates later stages of the pipeline

THe "other" CD is Continuous Deployment. It's intended to automated the final stages of the application into being used by customers.

Sequential execution of the pipeline is normal. A failure in any part of the pipeline would mean the entire process is required to start again. This should not labourious in and of itself of course as the process is automation. The changes one needs to make to resove the blockage in the pipeline could be trivial, or not.

Possible pipline failures:

- Pipeline misconfiguration
- Missing Dependency
- Incompatible versions
- Failed unit or integration tests

### 4.2 Integrate an application into a prebuilt CD environment leveraging Docker and Kubernetes

Kubernetes facilitates both declerative configuration and automation.
Performs:

- Service discovery and load balancing
- Storage orchestration
- Automated rollouts and rollbacks
- Self-healing
- Secret and configuration management

Kubernetes is not:

- Limited to supported applications. If it runs on Docker, you should be good.
- Able to build source code or deploy your applications
- Able to provide middleware or back end services, but those service can run on K8S

A pod is the smallest deployable unit of computing in K8S.
It's usual to have one contiainer per pod, which means the pod can scale out.
If you have more than one container, have more than one pod.

A deployment of K8S is a cluster. In the cluster you get worker machines called nodes that run containerized applications. Worker nodes host pods.

The control plane runs on _master_ nodes and manages _worker_ nodes and pods in the cluster.

- kubelet
- kube-proxy
- Container runtime, for example Docker

### 4.3 Describe the benefits of continuous testing and static code analysis in a CI pipeline

When code is executed and validated aginst a specific outcome - using Python unittest as example - is called Dynamic Testing.

- Improved code quality
- Accelerated software delivery
- Agile and repeatable process taking hours rather than weeks
- Elimates the siloing between development and test teams
- Improved customer loyalty due to continuous improvements and better quality
- Reduced business risks as problems are assessed before reaching production.

Static code analysis on the other hand provides additional advantages

- Error detection
- Security vulnerabilties detection
- Low cost
- Coding standards compliance
- Better source code

[Black](https://pypi.org/project/black/) can analyse your source code for [PEP8 compliance](https://www.python.org/dev/peps/pep-0008/) at the time of commiting to the repo. You'd use a [client side hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to enforce the review of code by Black and get any push backs that it had for you.

### 4.4 Utlize Docker to containerize an application

### 4.5 Describe the tenets of the "12 factor app"

A [methodology](https://12factor.net/) for building SaaS applications

- Factor 1: Codebase

    One codebase with version control. No exceptions

- Factor 2: Dependencies

    Create a Python virtual environment - pull the code and run `pip -r requirements.txt`

- Factor 3: Config

    Use of environment variables. This is intended to reduce the chance of config making it into the codebase

- Factor 4: Backing services

    The code for a 12-factor app makes no distinction between local and third-party service. One resource may be replaced with another without any change to the app code.

- Factor 5: Build, release, run

    Strictly separate build and run stages. No changes are allowed in the running release. If an update is required, apply it to the source code and repeat all the stages.

- Factor 6: Processes

    The app consists of one or more stateless processes. Any data that must persist is stored in a stateful backing service - typically a database.

- Factor 7: Port binding

    The app provides HTTP service by binding to a port and listening to requests coming in on that port.

- Factor 8: Concurrency

    Allows easy independent scale-out for each individual process type.

- Factor 9: Disposability

    Can be stopped and started at a moments notice and can handle ungraceful shutdowns.

- Factor 10: Dev/Prod Parity

    Make the tools as similar, personel closely tied between the two and time gaps short for builds

- Factor 11: Logs

    Each running process writes its event stream, unbuffered, to stdout.
    These archival destinations are _not visible to or configurable_ by the app.

- Factor 12: Admin processess

    Admin code must ship with application code to avoid synchronization issues.

### 4.6 Describe an effective logging strategy for an application

Example of a code snippet which logs to files

```python
import logging

logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %H:%M:%S', filename='zoomacl.log', encoding='utf-8', level=logging.INFO)

logger = logging.getLogger(app_name)
logger.info(f'Starting {app_name} in a {run_mode} mode')

if zoomstr.status_code != 200:
    logging.error('There is a problem with accessing the source URL %s', zoomlist)
    logging.error("Status code: %s", zoomstr.status_code)
```

One could and [should use streaming](https://docs.python.org/3/library/logging.handlers.html#sysloghandler)

```python
from logging.handlers import SysLogHandler
import logging

logger = logging.getLogger()
logger.addHandler(SysLogHandler('/dev/log'))
logger.addHandler(logging.FileHandler("filename.log"))

logging.warn("Hello world")
```

it’s best to preprocess log messages and transform them into some common standard before aggregating.
Pick a single source for the log’s time (for example, add the aggregator’s time to every processed log message).

- Collection
- Forwarding
- Storage
- Indexing (optional)
- Alerting (optional)

### 4.7 Explain data privacy concerns related to storage and transmission of data

### 4.8 Identify the secret storage approach relevant to a given scenario

### 4.9 Configure application-specific SSL certificates

CRLs and OCSP are two mechanisms to check revocation status of certificates.
CRLs are a list of certs which must be parsed, OCSP is a key:value service which the client sends the exact cert of interest and recieves a specific reply regarding that certificate rather than the CRL for the CA which could include vast numbers of certificates which are of no interest to the client.

The most common issues with certificates

- Hostname/Identity mismatch
- Validity date-range
- Signature valiadation error

### 4.10 Implement mitigation strategies for OWASP threats (such as XSS, CSRF, and SQL injection)

For the context of the exam - currently Nov 2021, the _older_ OWASP Top 10 is referred to.

A1:2017-Injection
Application functions related to authentication and session management are often implemented
incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit
other implementation flaws to assume other users’ identities temporarily or permanently.
A2:2017-Broken Authentication
Many web applications and APIs do not properly protect sensitive data, such as financial,
healthcare, and PII. Attackers may steal or modify such weakly protected data to conduct credit
card fraud, identity theft, or other crimes. Sensitive data may be compromised without extra
protection, such as encryption at rest or in transit, and requires special precautions when
exchanged with the browser.
A3:2017-Sensitive Data Exposure
Many older or poorly configured XML processors evaluate external entity references within XML
documents. External entities can be used to disclose internal files using the file URI handler,
internal file shares, internal port scanning, remote code execution, and denial of service attacks.
A4:2017-XML External Entities (XXE)
Restrictions on what authenticated users are allowed to do are often not properly enforced.
Attackers can exploit these flaws to access unauthorized functionality and/or data, such as access
other users' accounts, view sensitive files, modify other users’ data, change access rights, etc.
A5:2017-Broken Access Control
Security misconfiguration is the most commonly seen issue. This is commonly a result of insecure
default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured
HTTP headers, and verbose error messages containing sensitive information. Not only must all
operating systems, frameworks, libraries, and applications be securely configured, but they must
be patched and upgraded in a timely fashion.
XSS flaws occur whenever an application includes untrusted data in a new web page without
proper validation or escaping, or updates an existing web page with user-supplied data using a
browser API that can create HTML or JavaScript. XSS allows attackers to execute scripts in the
victim’s browser which can hijack user sessions, deface web sites, or redirect the user to
malicious sites.
A7:2017-Cross-Site Scripting (XSS)
Insecure deserialization often leads to remote code execution. Even if deserialization flaws do not
result in remote code execution, they can be used to perform attacks, including replay attacks,
injection attacks, and privilege escalation attacks.
A8:2017-Insecure Deserialization
Components, such as libraries, frameworks, and other software modules, run with the same
privileges as the application. If a vulnerable component is exploited, such an attack can facilitate
serious data loss or server takeover. Applications and APIs using components with known
vulnerabilities may undermine application defenses and enable various attacks and impacts.
A9:2017-Using Components with Known Vulnerabilities
Insufficient logging and monitoring, coupled with missing or ineffective integration with incident
response, allows attackers to further attack systems, maintain persistence, pivot to more systems,
and tamper, extract, or destroy data. Most breach studies show time to detect a breach is over
200 days, typically detected by external parties rather than internal processes or monitoring

For your information, there is the [current OWASP Top 10](https://owasp.org/www-project-top-ten/) which supercedes the 2017 version.

### 4.11 Describe how end-to-end encryption principles apply to APIs
