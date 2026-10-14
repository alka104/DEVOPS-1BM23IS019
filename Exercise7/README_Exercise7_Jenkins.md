# Exercise 7: Introduction to Continuous Integration (CI) and Jenkins Installation

## 1. Objective

This exercise introduces **Continuous Integration (CI)** and Jenkins.
The goal is to understand the CI workflow, learn basic Jenkins concepts,
run Jenkins using Docker, complete its initial setup, and open the
Jenkins dashboard.

> **Note:** Jenkins had already been installed and configured during the
> previous exercise. For Exercise 7, the existing Jenkins container was
> reused instead of creating a duplicate container.

## 2. What is Continuous Integration (CI)?

Continuous Integration is a development practice in which developers
frequently integrate code changes into a shared repository. Automated
builds and tests help detect problems early and provide quick feedback.

### Key features

-   Frequent integration of code changes
-   Automated builds
-   Automated testing
-   Quick feedback when a build or test fails

### Benefits

-   Detects bugs and integration issues early
-   Improves collaboration
-   Speeds up development cycles
-   Helps maintain software quality

### Typical CI workflow

1.  A developer changes code and pushes it to a Git repository.
2.  A CI server detects the change or receives a build trigger.
3.  The server builds the application and resolves dependencies.
4.  Automated tests run.
5.  The server reports whether the build and tests succeeded.

## 3. Jenkins overview

Jenkins is an open-source automation server commonly used for Continuous
Integration and Continuous Delivery.

Important Jenkins concepts:

-   **Job/Project:** A task configured in Jenkins.
-   **Build:** One execution of a job.
-   **Pipeline:** A sequence of automated steps.
-   **Plugins:** Extensions that add integrations and features.
-   **Nodes:** Machines or environments that execute Jenkins jobs.

## 4. Prerequisites

-   Windows with PowerShell
-   Docker Desktop installed and running
-   A web browser

Check that Docker is available:

``` powershell
docker --version
```

Check whether the existing Jenkins container is present:

``` powershell
docker ps -a --filter "name=jenkins"
```

The container was named `jenkins` and used the image
`jenkins/jenkins:lts`.

## 5. Jenkins installation command

The standard command in the assignment to create a Jenkins container is:

``` powershell
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```

-   `-d` runs the container in the background.
-   `--name jenkins` names the container `jenkins`.
-   `-p 8080:8080` maps the Jenkins web interface to port `8080` on the
    local computer.
-   `-p 50000:50000` maps port `50000`, commonly used for inbound
    Jenkins agents.
-   `jenkins/jenkins:lts` specifies the Jenkins Long-Term Support image.

### Important: reuse the existing container

In the previous exercise, Jenkins was created with a persistent volume:

``` powershell
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

The `-v jenkins_home:/var/jenkins_home` option stores Jenkins data in a
Docker volume so that configuration and jobs can persist when the
container is stopped and started again.

**Do not run either `docker run` command again if the `jenkins`
container already exists.** Docker will report a name conflict. Start
the existing container instead.

## 6. Commands used to resume Jenkins

### Check the container

``` powershell
docker ps -a --filter "name=jenkins"
```

An output showing `Exited (143)` means the container existed but was
stopped. It does not mean Jenkins was uninstalled.

### Start the existing container

``` powershell
docker start jenkins
```

### Confirm that it is running

``` powershell
docker ps --filter "name=jenkins"
```

Look for a status such as `Up ...` and a port mapping for `8080`.

### View recent logs if Jenkins does not open

``` powershell
docker logs --tail 50 jenkins
```

To follow the logs continuously:

``` powershell
docker logs -f jenkins
```

Press `Ctrl+C` to stop following the logs; this does not stop the
container.

## 7. Initial setup and browser access

Open the Jenkins web interface:

<http://localhost:8080>

For a first-time setup, Jenkins may ask for an initial administrator
password. If needed, retrieve it with:

``` powershell
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Copy the password into the unlock page, follow the setup wizard, install
the suggested plugins, and create an administrator account.

Because Jenkins had already been configured in the previous exercise,
the existing administrator login and setup were reused. The initial
password is generally needed only during the first-time unlock process.

## 8. Verify the Jenkins job

The existing job from the previous exercise was named
`Quick-commerce-monitoring`.

In the browser:

1.  Open the Jenkins dashboard.
2.  Select `Quick-commerce-monitoring`.
3.  Open the build history and select the successful build.
4.  Select **Console Output**.
5.  Confirm that the output ends with:

``` text
Finished: SUCCESS
```

This job checks the health of the Python metrics endpoint, Prometheus,
and Grafana. It is additional practical work from Exercise 6; the
Exercise 7 handout itself focuses on CI concepts and Jenkins
installation/setup.

## 9. Stop Jenkins when finished

To stop Jenkins without deleting its configuration:

``` powershell
docker stop jenkins
```

To start it again later:

``` powershell
docker start jenkins
```

Stopping a container does not delete it. Avoid running
`docker rm jenkins` or deleting the `jenkins_home` volume if you want to
preserve the existing setup and jobs.

## 10. Expected outcome / completion checklist

-   [x] Understand the purpose, features, and benefits of CI.
-   [x] Understand the basic Jenkins concepts.
-   [x] Run Jenkins using Docker.
-   [x] Complete the initial setup and access the dashboard.
-   [x] Verify that the existing Jenkins job has a successful build.
-   [x] Capture screenshots of the dashboard and successful build as
    evidence.

## 11. Screenshots for submission

Recommended screenshots:

1.  Jenkins dashboard.
2.  `Quick-commerce-monitoring` job with its successful build.
3.  Build Console Output showing `Finished: SUCCESS`.

## 12. Summary

This exercise covers the fundamentals of Continuous Integration and the
installation and setup of Jenkins using Docker. The existing Jenkins
container was restarted and reused, avoiding unnecessary reinstallation
while preserving the configured account and job.
