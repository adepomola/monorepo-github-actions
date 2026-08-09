# Building an Efficient CI/CD Pipeline for a Monorepo

## 1. Project Overview

This project focuses on designing and implementing an efficient Continuous Integration and Continuous Delivery (CI/CD) pipeline for a monorepository using GitHub Actions.

A monorepo is a single Git repository that contains multiple services or applications. Each service is maintained in its own dedicated directory.

The main challenge with a monorepo is that a change to one service can unnecessarily trigger builds, tests, and deployments for other services that were not changed.

This project addresses that challenge by creating a GitHub Actions workflow that identifies the services affected by a change and processes only those services.

### Main workflow

```text
Developer makes a change
        ↓
Git push
        ↓
GitHub Actions starts
        ↓
Detect changed service
        ↓
Identify affected service(s)
        ↓
Matrix processing
        ↓
Build
        ↓
Test
        ↓
Deploy
```


### Screenshot — First Successful GitHub Action

[![First Successful GitHub Action](images/06-monorepo-success.jpeg)]


---

## 2. Problem Statement

The company uses a monorepository structure to manage multiple services.

The repository follows a structure similar to:

```text
monorepo/
├── service-a/
├── service-b/
├── service-c/
└── ...
```

The problem is that not every change should trigger the complete CI/CD pipeline for every service.

For example, if a developer changes:

```text
service-b/index.html
```

the pipeline should identify that `service-b` is the affected service.

It should not unnecessarily build and deploy:

- `service-a`
- `service-c`

The objective is therefore to create an intelligent CI/CD pipeline that detects changes and processes only the affected services.

---

## 3. Project Objectives

The objectives of this project are to:

- Create a monorepo structure.
- Create multiple independent services.
- Store all services in one Git repository.
- Create a GitHub Actions workflow.
- Detect changes within individual service directories.
- Identify affected services.
- Use matrix jobs to process affected services.
- Implement build stages.
- Implement test stages.
- Implement deployment stages.
- Apply conditional execution.
- Reduce unnecessary builds.
- Reduce unnecessary deployments.
- Improve CI/CD execution speed.
- Reduce resource consumption.
- Apply CI/CD security best practices.
- Document the complete implementation.

---

## 4. Project Requirements

The project contains four major requirements.

### 4.1 Change Detection

The workflow must identify which service directory has been modified.

For example:

```text
service-a/index.html
```

should be recognized as a change affecting:

```text
service-a
```

### 4.2 Matrix Jobs

The workflow should use GitHub Actions matrix jobs to dynamically process affected services.

A matrix allows one job definition to process multiple services without creating separate duplicated jobs.

### 4.3 Pipeline Stages

The pipeline should contain appropriate CI/CD stages such as:

- Build
- Test
- Deploy

### 4.4 Conditional Execution

The workflow must ensure that pipeline stages execute only for the services that have been affected.

This prevents unnecessary processing of unchanged services.

---

## 5. Technologies and Tools

| Technology       | Purpose                        |
|------------------|--------------------------------|
| Git              | Version control                |
| GitHub           | Remote repository              |
| GitHub Actions   | CI/CD automation               |
| YAML             | Workflow configuration         |
| HTML             | Web application structure      |
| CSS              | Web application styling        |
| JavaScript       | Application functionality      |
| Visual Studio Code | Code editing                 |

---

## 6. Monorepo Architecture

A monorepo contains multiple services inside one repository.

The project uses the following architecture:

```text
Monorepo
│
├── service-a
│
├── service-b
│
└── service-c
```

Each service has its own files and can be developed independently.

The advantage of this approach is that related services can share the same repository, version control system, CI/CD configuration, and documentation.

However, the CI/CD system must be intelligent enough to determine which service needs to be processed.

![Monorepo Structure](images/01-monorepo-structure.jpg)

---

## 7. Creating the Project Directory

The first step was to create the main project directory.

The main directory represents the monorepository.

```text
monorepo/
```

All services and CI/CD configuration files are stored inside this directory.

---

## 8. Creating the Service Directories

Three service directories were created:

- `service-a/`
- `service-b/`
- `service-c/`

Each directory represents an individual service within the monorepository.

This separation is important because the GitHub Actions workflow needs to determine which directory has changed.

For example:

```text
service-a/
```

contains files belonging to Service A, while:

```text
service-b/
```

contains files belonging to Service B.

![Services Created](images/02-services-created.jpg)

---

## 9. Creating index.html

An `index.html` file was created inside each service.

For example:

```text
service-a/
└── index.html
```

The `index.html` file acts as the main entry point for the web application.

It provides the basic structure of the webpage and gives each service an actual application file that can be modified and tracked by Git.

The file is also useful for testing the change-detection mechanism.

For example, changing:

```text
service-a/index.html
```

should cause the pipeline to identify `service-a` as the affected service.

---

## 10. Creating style.css

The `style.css` file was created to control the appearance of the web application.

It can be used to define:

- Colours
- Fonts
- Spacing
- Layout
- Page dimensions
- Visual presentation

The file is stored inside each service directory.

Example:

```text
service-a/
├── index.html
└── style.css
```

---

## 11. Creating script.js

The `script.js` file provides JavaScript functionality for the service.

JavaScript can be used to make the application interactive.

For example, it can handle:

- Button interactions
- Events
- Dynamic content
- Browser functionality

The resulting service structure is:

```text
service-a/
├── index.html
├── style.css
└── script.js
```

The same structure can be used for the other services.

---

## 12. Initializing Git

Git was initialized in the project directory to provide version control.

The command used was:

```bash
git init
```

Git allows changes to the project files to be tracked over time.

This is particularly important for CI/CD because GitHub Actions responds to repository events such as commits and pushes.

### Screenshot — Git Initialization

[![Git Initialization](images/03-git-initialized.png)](images/03-git-initialized.png)

---

## 13. Adding the Project Files to Git

After creating the project files, they were added to Git using:

```bash
git add .
```

The `.` means that Git should stage the files in the current project directory.

The staged files can then be committed.

```bash
git commit -m "Initial monorepo setup"
```

This creates a recorded version of the project.

---

## 14. Creating the GitHub Repository

A GitHub repository was created to store the project remotely.

GitHub provides the remote repository where the source code and GitHub Actions workflow are stored.

The local repository was connected to GitHub using the repository’s remote URL.

The main branch was configured as:

```bash
git branch -M main
```

The project was then pushed to GitHub.

```bash
git push -u origin main
```

### Screenshot — GitHub Repository

[![GitHub Repository](images/04-github-repository.png)](images/04-github-repository.png)

---

## 15. Creating the GitHub Actions Directory

GitHub Actions workflow files must be stored inside:

```text
.github/workflows/
```

Therefore, the following directory structure was created:

```text
.github/
└── workflows/
```

This tells GitHub where to find the workflow configuration.

---

## 16. Creating ci.yml

The CI/CD workflow was created inside the workflows directory.

The final path is:

```text
.github/workflows/ci.yml
```

The `ci.yml` file contains the instructions GitHub Actions follows when executing the pipeline.

The workflow defines:

- When the pipeline should run
- What jobs should execute
- Which service should be processed
- How changes are detected
- How matrix jobs operate
- How build and test operations are performed
- How deployment logic is handled

### Screenshot — CI Workflow Configuration

[![CI Workflow Configuration](images/08-ci-yml-code.jpeg)](images/08-ci-yml-code.jpeg)

---

## 17. Understanding the Workflow

The workflow can be understood as a sequence of automated operations.

```text
Git Push
   ↓
GitHub Actions
   ↓
Change Detection
   ↓
Affected Service
   ↓
Matrix Job
   ↓
Build
   ↓
Test
   ↓
Deploy
```

The important part is that the workflow does not blindly process every service.

Instead, it first determines which services were affected.

---

## 18. Change Detection

Change detection is one of the most important parts of the project.

The workflow checks which files have changed.

For example, if the following file changes:

```text
service-b/index.html
```

the workflow determines that:

```text
service-b
```

is affected.

The workflow can use Git’s change information to determine the affected service.

This is important because it prevents unnecessary CI/CD work.

### Screenshot — Change Detection

[![Change Detection](images/05-change-detection.png)](images/05-change-detection.png)

---

## 19. Why Change Detection Is Important

Without change detection, the pipeline could process every service.

For example:

```text
Developer changes service-a
        ↓
Build service-a
Build service-b
Build service-c
        ↓
Test service-a
Test service-b
Test service-c
        ↓
Deploy all services
```

This is inefficient.

With change detection:

```text
Developer changes service-a
        ↓
Detect service-a
        ↓
Build service-a
        ↓
Test service-a
        ↓
Deploy service-a
```

The second approach uses fewer resources and provides faster feedback.

---

## 20. Matrix Jobs

Matrix jobs allow GitHub Actions to run the same job configuration against multiple values.

In this project, the matrix values represent services.

For example:

- `service-a`
- `service-b`
- `service-c`

A matrix can therefore process affected services using a reusable job structure.

Conceptually:

```text
Matrix
│
├── service-a
├── service-b
└── service-c
```

This avoids creating separate copies of the same workflow for every service.

---

## 21. Processing Multiple Changed Services

The workflow also supports situations where more than one service has changed.

For example:

- `service-a`
- `service-c`

If both services are affected, the matrix can process both.

```text
Change Detection
       ↓
 ┌───────────────┐
 ↓               ↓
service-a     service-c
 ↓               ↓
Build           Build
 ↓               ↓
Test            Test
 ↓               ↓
Deploy          Deploy
```

An unchanged service does not need to be processed.

### Screenshot — Multiple Services Matrix Success

[![Multiple Services Matrix Success](images/07-multiple-services-matrix-success.png)](images/07-multiple-services-matrix-success.png)

---

## 22. Build Stage

The Build stage prepares the affected service for further processing.

Depending on the technology used by the application, a build may involve:

- Installing dependencies
- Compiling source code
- Generating production files
- Packaging the application
- Building a Docker image

The important requirement is that the build should apply to the affected service.

The workflow therefore follows:

```text
Affected Service
      ↓
     Build
      ↓
Build Successful?
   ↓          ↓
 No          Yes
 ↓            ↓
Stop        Continue
```

A failed build should prevent invalid code from progressing.

---

## 23. Test Stage

After a successful build, the service can be tested.

Testing helps determine whether the application behaves correctly.

Possible tests include:

- Unit tests
- Integration tests
- Application checks
- Syntax validation
- Smoke tests

The pipeline follows:

```text
Build
 ↓
Test
 ↓
Tests Passed?
 ↓       ↓
No      Yes
↓        ↓
Stop   Continue
```

If testing fails, deployment should not proceed.

---

## 24. Deployment Stage

The deployment stage is responsible for delivering the validated application to a target environment.

A production CI/CD pipeline could use:

```text
Build
 ↓
Test
 ↓
Staging
 ↓
Approval
 ↓
Production
```

For this project, the deployment stage demonstrates how the affected service can progress toward deployment without requiring unrelated services to be processed.

---

## 25. Conditional Execution

Conditional execution ensures that jobs or steps execute only when their required conditions are satisfied.

For example:

```text
service-a changed
       ↓
Condition = TRUE
       ↓
Process service-a
```

For an unaffected service:

```text
service-b unchanged
       ↓
Condition = FALSE
       ↓
Skip service-b
```

This is one of the mechanisms that makes the pipeline efficient.

---

## 26. Triggering the Workflow

After the workflow was created and committed to GitHub, changes were pushed to the repository.

The push triggered GitHub Actions.

The execution process was:

```text
Git Commit
    ↓
Git Push
    ↓
GitHub Repository
    ↓
GitHub Actions
    ↓
Workflow Execution
```

The Actions section of the GitHub repository was used to monitor the workflow.

### Screenshot — First Successful GitHub Action

[![First Successful GitHub Action](images/06-monorepo-success.jpeg)](images/06-monorepo-success.jpeg)

---

## 27. Verifying Pipeline Execution

The GitHub Actions execution was checked to confirm that the workflow was recognized and executed successfully.

A successful workflow indicates that:

- The workflow file was valid.
- GitHub recognized the workflow.
- The required jobs could execute.
- The configured pipeline logic was processed.
- The repository was successfully integrated with GitHub Actions.

---

## 28. Dependency Management

Dependency management is important because applications often rely on external packages and libraries.

Poor dependency management can lead to:

- Inconsistent builds
- Compatibility problems
- Security vulnerabilities
- Unexpected failures

A production project should lock dependency versions where appropriate.

For JavaScript applications, this can involve:

- `package.json`
- `package-lock.json`

Dependency caching can also reduce build times by avoiding unnecessary downloads.

Additional tools such as Dependabot or Renovate can be used to automate dependency updates.

---

## 29. Environment Configuration

Different deployment environments may require different configuration values.

For example:

- Development
- Staging
- Production

Each environment may have different:

- API endpoints
- Database connections
- Feature flags
- Credentials
- Environment variables

Sensitive configuration should never be hardcoded into source code.

GitHub Secrets or an external secrets manager should be used for sensitive information.

---

## 30. Error Handling

A CI/CD pipeline must handle failures safely.

Possible failures include:

- Invalid YAML
- Build failure
- Test failure
- Dependency failure
- Incorrect file path
- Deployment failure

A good pipeline prevents failed stages from continuing automatically.

For example:

```text
Build
 ↓
FAILED
 ↓
Test skipped
 ↓
Deployment skipped
```

This prevents potentially broken software from reaching the deployment environment.

---

## 31. Security

Security is an important part of CI/CD.

Sensitive information such as:

- Passwords
- API keys
- Tokens
- Cloud credentials
- SSH keys

should not be written directly into workflow files.

Instead, secure secrets should be stored using GitHub Secrets or an appropriate secrets-management solution.

Important security practices include:

- Never commit secrets.
- Use least-privilege permissions.
- Protect repository access.
- Review third-party Actions.
- Keep dependencies updated.
- Avoid printing secrets in workflow logs.
- Regularly scan repositories for accidentally exposed credentials.

---

## 32. Scalability

The pipeline architecture can scale as more services are added.

For example:

```text
3 services
   ↓
10 services
   ↓
50 services
   ↓
100 services
```

If every commit triggered every service, CI/CD execution could become slow and expensive.

Change detection solves part of this problem by processing only affected services.

Matrix jobs then provide a reusable mechanism for processing multiple affected services.

---

## 33. Monitoring and Logging

GitHub Actions provides logs for workflow executions.

These logs help identify:

- Which job executed.
- Which step succeeded.
- Which step failed.
- Why a step failed.
- How long the workflow took.

For larger production environments, additional monitoring systems could include:

- Prometheus
- Grafana
- ELK Stack
- Datadog
- Cloud monitoring services

These tools can provide deeper visibility into pipeline health and application performance.

---

## 34. Resource Optimization

The main optimization achieved by this project is reducing unnecessary CI/CD execution.

**Before optimization**

```text
Change service-a
       ↓
Build A
Build B
Build C
       ↓
Test A
Test B
Test C
       ↓
Deploy A
Deploy B
Deploy C
```

**After optimization**

```text
Change service-a
       ↓
Detect service-a
       ↓
Build A
       ↓
Test A
       ↓
Deploy A
```

This approach can reduce:

- Build time
- Runner usage
- Compute consumption
- Test execution time
- Deployment overhead

It also provides developers with faster feedback.

---

## 35. Troubleshooting

Several common issues can affect GitHub Actions workflows.

**YAML indentation**

YAML is indentation-sensitive.

Incorrect indentation can cause the workflow to fail validation.

**Incorrect workflow location**

The workflow must be located inside:

```text
.github/workflows/
```

For this project:

```text
.github/workflows/ci.yml
```

**Incorrect service paths**

The service paths used for change detection must match the actual repository structure.

For example:

```text
service-a/index.html
```

is different from:

```text
service-b/index.html
```

**Changes not pushed**

GitHub Actions runs against the remote GitHub repository.

Therefore, local changes must be committed and pushed.

```bash
git add .
git commit -m "Update workflow"
git push
```

---

## 36. Project Limitations

One limitation encountered during the project documentation process was a hardware problem with the laptop keyboard.

The keyboard was not functioning normally. Some keys were not responding while other keys were producing incorrect characters.

Because screenshots were required as evidence for different stages of the project, this hardware issue affected the ability to consistently capture screenshots directly from the laptop.

Some screenshots were therefore captured using a mobile phone.

Consequently, some evidence files are stored as JPEG images rather than PNG images.

For example:

- `images/06-monorepo-success.jpeg'
- `images/08-ci-yml-code.jpeg`

This does not affect the implementation of the project. The image format only reflects the method used to capture the evidence.

The available screenshots have been included under the relevant steps in this documentation.

Where an individual screenshot is unavailable, the implementation is explained through the documented workflow, repository structure, and configuration.

---

## 37. Future Improvements

The pipeline can be improved further by introducing additional DevOps technologies and practices.

Possible improvements include:

**Docker**

Each service can be packaged into a Docker image.

```text
Service
 ↓
Docker Build
 ↓
Docker Image
```

**Kubernetes**

Kubernetes can be introduced for container orchestration and scalable deployments.

**Automated Rollback**

The pipeline can automatically return to a previously stable version if deployment health checks fail.

**Canary Deployment**

A new version can initially be deployed to a small percentage of users before a complete rollout.

**Blue-Green Deployment**

Two environments can be maintained, allowing traffic to be switched between application versions.

**Security Scanning**

Automated vulnerability and secret scanning can be added to the pipeline.

**Advanced Monitoring**

Prometheus and Grafana can be integrated to provide detailed pipeline and application monitoring.

---

## 38. Requirements Verification

| Requirement              | Implementation                          | Status     |
|--------------------------|-----------------------------------------|------------|
| Monorepo structure       | Multiple service directories            | Completed  |
| Individual services      | Service A, B and C                      | Completed  |
| Git version control      | Git repository initialized              | Completed  |
| GitHub repository        | Repository created and pushed           | Completed  |
| GitHub Actions           | ci.yml workflow                         | Completed  |
| Change detection         | Affected services identified            | Implemented|
| Matrix jobs              | Multiple services processed through matrix | Implemented |
| Build stage              | Build stage included                    | Implemented|
| Test stage               | Test stage included                     | Implemented|
| Deployment stage         | Deployment stage included               | Implemented|
| Conditional execution    | Unaffected services skipped             | Implemented|
| Resource optimization    | Only relevant services processed        | Demonstrated |
| Security                 | Security practices documented           | Addressed  |
| Troubleshooting          | Common problems documented              | Addressed  |
| Technical documentation  | README created                          | Completed  |

---

## 39. Project Outcome

The completed project demonstrates an efficient CI/CD architecture for a monorepository.

The final workflow follows:

```text
Developer
   ↓
Code Change
   ↓
Git Commit
   ↓
Git Push
   ↓
GitHub
   ↓
GitHub Actions
   ↓
Change Detection
   ↓
Affected Service(s)
   ↓
Matrix Job
   ↓
Build
   ↓
Test
   ↓
Deploy
```

The major achievement of the project is the ability to identify affected services and avoid unnecessarily processing services that have not changed.

This improves CI/CD efficiency and provides a foundation for a scalable monorepo deployment architecture.

---

## 40. DevOps Concepts Demonstrated

This project demonstrates practical understanding of:

**Version Control**

Git was used to track source-code changes.

**GitHub**

GitHub was used as the remote repository and CI/CD platform.

**Continuous Integration**

GitHub Actions automatically processes repository changes.

**Continuous Delivery**

The pipeline is structured to move validated changes toward deployment.

**Pipeline as Code**

The CI/CD configuration is stored in:

```text
.github/workflows/ci.yml
```

**Change Detection**

The workflow identifies affected service directories.

**Matrix Jobs**

Matrix jobs allow the same workflow logic to process multiple services.

**Conditional Execution**

Only relevant services are processed.

**Automation**

Manual CI/CD tasks are automated through GitHub Actions.

**Scalability**

The architecture can be extended as more services are added.

**Security**

The project considers secret management and least-privilege principles.

---

## 41. Conclusion

This project successfully demonstrates the design and implementation of an efficient CI/CD pipeline for a monorepository using GitHub Actions.

The main challenge was to prevent unnecessary builds, tests, and deployments when only one service had been modified.

The solution involved:

- Creating a monorepo structure.
- Creating independent service directories.
- Adding application files.
- Initializing Git.
- Creating a GitHub repository.
- Creating a GitHub Actions workflow.
- Detecting changed services.
- Using matrix jobs.
- Applying conditional execution.
- Implementing CI/CD stages.
- Testing workflow execution.
- Documenting the implementation.

The project demonstrates that an effective CI/CD pipeline should not simply automate every task on every change.

Instead, the pipeline should intelligently determine what needs to be processed.

By detecting affected services before executing CI/CD operations, the system can reduce unnecessary builds, reduce resource consumption, improve feedback time, and provide a more scalable approach to managing a monorepository.

---

## 42. References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Actions Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Using a Matrix for Your Jobs](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)
- [GitHub Actions Security](https://docs.github.com/en/actions/security-guides)
- [Using Secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [Git Documentation](https://git-scm.com/doc)

---

## 43. Screenshot Evidence

All project screenshots are stored inside the `images` directory.

Each screenshot is linked to its actual file so that it can be opened directly from the GitHub repository.

| No. | Evidence                          | File |
|-----|-----------------------------------|------|
| 1   | Monorepo structure                | [Open Image](images/01-monorepo-structure.jpg) |
| 2   | Services created                  | [Open Image](images/02-services-created.jpg) |
| 3   | Git initialization                | [Open Image](images/03-git-initialized.jpg) |
| 4   | GitHub repository                 | [Open Image](images/04-github-repository.jpg) |
| 5   | Change detection                  | [Open Image](images/05-change-detection.jpg) |
| 6   | First successful Action           | [Open Image](images/06-first-successful-action.jpg) |
| 7   | Multiple-service matrix success   | [Open Image](images/07-multiple-services-matrix-success.jpg) |
| 8   | CI workflow configuration         | [Open Image](images/08-ci-yml-code.jpg) |

---


---

## Final Project Statement

The repository contains the implementation, GitHub Actions workflow, service structure, screenshot evidence, and technical documentation required for the project.

The project demonstrates how GitHub Actions can be used to build an efficient CI/CD pipeline for a monorepo by detecting affected services, processing them through matrix jobs, and avoiding unnecessary CI/CD operations for unchanged services.
