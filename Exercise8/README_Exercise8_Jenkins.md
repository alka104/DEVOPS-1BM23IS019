# Exercise 8: Creating a Hello World Jenkins Job

## 1. Objective

The objective of this exercise is to create a simple shell script, push
it to GitHub, configure a Jenkins Freestyle project to retrieve the
repository, execute the script, and verify the build output.

## 2. Prerequisites

-   Windows with PowerShell
-   Git installed and configured
-   Docker Desktop installed and running
-   Jenkins running in a Docker container
-   An existing GitHub repository

## 3. Repository and script

This exercise uses the existing repository:

``` text
https://github.com/alka104/DEVOPS-1BM23IS019.git
```

The script is located at:

``` text
Exercise8/hello-world.sh
```

### `hello-world.sh`

``` bash
#!/bin/bash
echo "Hello, Jenkins!"
```

The script prints `Hello, Jenkins!` when executed.

## 4. Create the script on Windows

From the repository root, create the exercise folder and enter it:

``` powershell
mkdir Exercise8
cd Exercise8
```

Create the script:

``` powershell
@'
#!/bin/bash
echo "Hello, Jenkins!"
'@ | Set-Content -Encoding ascii hello-world.sh
```

Verify its contents:

``` powershell
Get-Content hello-world.sh
```

Expected output:

``` text
#!/bin/bash
echo "Hello, Jenkins!"
```

## 5. Commit and push the script to GitHub

Return to the repository root:

``` powershell
cd ..
```

Stage only the new script:

``` powershell
git add Exercise8/hello-world.sh
```

Check the staged changes:

``` powershell
git status
```

Commit the file:

``` powershell
git commit -m "Add Exercise 8 Hello World Jenkins script"
```

Push to the existing `main` branch:

``` powershell
git push origin main
```

Using the existing repository keeps this exercise alongside the other
exercises. Staging only the script avoids accidentally including
unrelated changes.

## 6. Start and verify Jenkins

Start the existing Jenkins container:

``` powershell
docker start jenkins
```

Check that it is running:

``` powershell
docker ps --filter "name=jenkins"
```

Open Jenkins in a browser:

<http://localhost:8080>

Sign in using the existing Jenkins credentials. No new Jenkins
installation is required if the existing container is still present.

## 7. Create the Jenkins Freestyle project

1.  On the Jenkins dashboard, click **New Item**.
2.  Enter the item name `HelloWorld`.
3.  Select **Freestyle project**.
4.  Click **OK**.
5.  Optionally, enter this description: `Hello World! Jenkins job.`

### Source Code Management

1.  Select **Git**.

2.  Set the Repository URL to:

    ``` text
    https://github.com/alka104/DEVOPS-1BM23IS019.git
    ```

3.  Leave credentials as `- none -` because this repository is public.

4.  Set **Branch Specifier** to:

    ``` text
    */main
    ```

### Build Steps

1.  Scroll to **Build Steps**.

2.  Click **Add build step**.

3.  Select **Execute shell**.

4.  Enter these commands:

    ``` bash
    cd Exercise8
    sh hello-world.sh
    ```

The first command changes into the folder containing the script. The
second command executes it.

5.  Click **Save**.

## 8. Run the job and verify the result

1.  Open the `HelloWorld` job.
2.  Click **Build Now**.
3.  Wait for the build to finish.
4.  In **Build History**, open the new build number.
5.  Click **Console Output**.

### Expected output

``` text
+ cd Exercise8
+ sh hello-world.sh
+ echo 'Hello, Jenkins!'
Hello, Jenkins!
Finished: SUCCESS
```

The actual Console Output may include additional Git checkout and
Jenkins build log lines. The key verification is that `Hello, Jenkins!`
is printed and the build ends with `Finished: SUCCESS`.

## 9. Troubleshooting

-   **Repository checkout fails:** Verify the repository URL and confirm
    the repository is public.
-   **Branch not found:** Ensure the Branch Specifier is `*/main`.
-   **`hello-world.sh` not found:** Confirm the file exists at
    `Exercise8/hello-world.sh` in GitHub and that the build step starts
    with `cd Exercise8`.
-   **Jenkins page does not open:** Check the container with
    `docker ps --filter "name=jenkins"` and inspect logs using
    `docker logs --tail 50 jenkins`.
-   **Build fails:** Open the build's **Console Output** and inspect the
    first error message.

## 10. Completion checklist

-   [x] Created `Exercise8/hello-world.sh`.
-   [x] Pushed the script to the existing GitHub repository.
-   [x] Created the `HelloWorld` Freestyle project.
-   [x] Configured GitHub source control and the `main` branch.
-   [x] Added the shell commands to execute the script.
-   [x] Ran the job and verified `Hello, Jenkins!`.
-   [x] Confirmed `Finished: SUCCESS`.

## 11. Evidence for submission

Capture a screenshot of the `HelloWorld` build's **Console Output**
showing:

``` text
Hello, Jenkins!
Finished: SUCCESS
```

## 12. Summary

This exercise demonstrates a basic Jenkins job that checks out source
code from GitHub and executes a shell script. The successful build
confirms that Jenkins can retrieve the repository, run the script from
its folder, and report the result.
