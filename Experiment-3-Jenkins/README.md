# Experiment 3: Jenkins and GitHub Integration

## Aim
To integrate a GitHub repository with Jenkins and automate project execution.

## Build methods
1. Manual build
2. Periodic build using Poll SCM
3. Event-based build using GitHub Webhooks

## Application
A simple HTML page used to verify repository checkout.

## Jenkins Install
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21  


# Experiment 3: Jenkins and GitHub Integration

## Aim

To integrate Jenkins with GitHub, create a Freestyle project, execute a build, and configure Poll SCM to automatically trigger builds when changes are pushed to the GitHub repository.

## Requirements

- macOS
- Docker Desktop
- Jenkins
- Git
- GitHub account
- Visual Studio Code

## GitHub Repository

**Repository URL:** https://github.com/AjithRaj2525/DevOpsIA.git

**Branch:** `main`

---

## Step 1: Start Jenkins Using Docker

Open the VS Code terminal and execute the following command to start Jenkins in a Docker container:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts-jdk21
```

The Docker volume `jenkins_home` stores Jenkins configuration and build data.

If the Jenkins container is already running, this command does not need to be executed again.

## Step 2: Access Jenkins

Check the Jenkins startup logs:

```bash
docker logs -f jenkins
```

Wait until Jenkins finishes initializing. Press `Control + C` to exit the log display without stopping the container.

Retrieve the initial administrator password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Open the following URL in a browser:

http://localhost:8080

1. Enter the initial administrator password.
2. Select **Install suggested plugins**.
3. Create an administrator account.
4. Complete the setup wizard and open the Jenkins dashboard.

## Step 3: Create a Freestyle Project

1. Open the Jenkins dashboard.
2. Click **New Item**.
3. Enter the project name `Experiment-3-GitHub`.
4. Select **Freestyle project**.
5. Click **OK**.

## Step 4: Configure GitHub Repository

In the project configuration page, locate **Source Code Management**.

1. Select **Git**.
2. Enter the repository URL:

   ```text
   https://github.com/AjithRaj2525/DevOpsIA.git
   ```

3. Select `- none -` under Credentials if the repository is public.
4. Under **Branches to build**, enter:

   ```text
   */main
   ```

Jenkins will fetch the source code from the GitHub repository's `main` branch.

## Step 5: Configure the Build Step

Under **Build Steps**:

1. Click **Add build step**.
2. Select **Execute shell**.
3. Enter the following commands:

   ```bash
   echo "Experiment 3: Jenkins and GitHub Integration"
   echo "Checking out source code..."
   ls -la
   test -f Experiment-3-Jenkins/Jenkinsfile
   echo "GitHub integration successful!"
   ```

4. Click **Save**.

These commands display a message, list the checked-out files, verify that the Jenkinsfile exists, and print a success message.

## Step 6: Execute a Manual Build

1. Open the `Experiment-3-GitHub` project.
2. Click **Build Now**.
3. Under Build History, click the latest build number.
4. Select **Console Output**.

Verify that the output contains:

```text
Experiment 3: Jenkins and GitHub Integration
Checking out source code...
GitHub integration successful!
Finished: SUCCESS
```

A `SUCCESS` status confirms that Jenkins successfully retrieved the source code and executed the configured build commands.

## Step 7: Configure Poll SCM

Poll SCM periodically checks the GitHub repository for new commits.

1. Open `Experiment-3-GitHub`.
2. Click **Configure**.
3. Locate **Build Triggers** or **Triggers**.
4. Enable **Poll SCM**.
5. Enter the following schedule:

   ```text
   * * * * *
   ```

This schedule checks the repository every minute.

6. Click **Save**.

**Note:** Poll SCM triggers a build when it detects relevant source-code changes. It does not necessarily run a new build every minute when no changes are found.

## Step 8: Test Poll SCM

To verify automatic build triggering, make a small change and push it to GitHub.

### A. Modify a file

Open the local project directory:

```bash
cd ~/Documents/DevOps
```

Edit the file:

```text
Experiment-3-Jenkins/index.html
```

Add the following HTML comment at the bottom and save the file:

```html
<!-- Testing Jenkins Poll SCM -->
```

### B. Commit and push the changes

Execute the following commands:

```bash
git add Experiment-3-Jenkins/index.html
git commit -m "Test Jenkins Poll SCM"
git push origin main
```

These commands stage the modified file, create a Git commit, and push the commit to GitHub.

### C. Verify automatic execution

1. Wait approximately one or two minutes.
2. Open Jenkins at http://localhost:8080.
3. Open the `Experiment-3-GitHub` project.
4. Check **Build History** for a newly triggered build.
5. Open the latest build and select **Console Output**.

A successful build after pushing the new commit demonstrates that Poll SCM is working.

## Result

Jenkins was integrated with the GitHub repository using a Freestyle project. A manual build was executed successfully, and Poll SCM was configured with a one-minute polling interval. The automatic build triggered after a new commit was pushed to GitHub, demonstrating successful continuous integration.

## Important Commands

| Command | Purpose |
|---|---|
| `docker logs -f jenkins` | View Jenkins startup logs |
| `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword` | Retrieve the initial password |
| `git add <file>` | Stage file changes |
| `git commit -m "message"` | Create a commit |
| `git push origin main` | Push commits to GitHub |

## Conclusion

The experiment demonstrates the integration of Jenkins with GitHub, execution of a Freestyle build, and automatic build triggering using Poll SCM when changes are pushed to the repository.


## Result
The application repository is prepared for Jenkins integration.