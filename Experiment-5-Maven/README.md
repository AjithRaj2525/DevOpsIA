# Experiment 5: Automated Build and Dependency Management Using Apache Maven

## Aim
To create a Java project using Apache Maven, manage dependencies through `pom.xml`, and automate the build process using GitHub Actions.

## Objectives
- Create a standard Java project structure using Maven.
- Manage dependencies through `pom.xml`.
- Compile Java source code and execute unit tests.
- Package the application into a JAR file.
- Automate the build using GitHub Actions.

## Technologies Used
- Java JDK 21
- Apache Maven
- Git and GitHub
- GitHub Actions

## Project Structure

```text
DevOps/
├── .github/
│   └── workflows/
│       └── maven.yml
└── Experiment-5-Maven/
    ├── README.md
    └── maven-demo/
        ├── .mvn/
        ├── src/
        │   ├── main/java/com/devops/App.java
        │   └── test/java/com/devops/AppTest.java
        └── pom.xml
```

## 1. Create the Maven Project

Navigate to the Experiment 5 folder:

```bash
cd ~/Documents/DevOps/Experiment-5-Maven
```

Generate the Maven project:

```bash
mvn archetype:generate \
  -DgroupId=com.devops \
  -DartifactId=maven-demo \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.5 \
  -DinteractiveMode=false
```

Verify the generated files:

```bash
ls maven-demo
```

## 2. Maven Configuration — `pom.xml`

The `pom.xml` file defines project information, dependencies, and build configuration.

To view the file:

```bash
cd ~/Documents/DevOps/Experiment-5-Maven/maven-demo
cat pom.xml
```

To open it in VS Code:

```bash
code pom.xml
```

Maven uses this file to manage dependencies and execute the project's build lifecycle.

## 3. Build the Project Locally

Run the following command from the `maven-demo` directory:

```bash
mvn clean package
```

**Explanation:**
- `clean` — removes previously generated build files.
- `package` — compiles the source code, runs unit tests, and packages the application into a JAR file.

Verify the generated files:

```bash
ls target
```

The output should include a JAR file similar to `maven-demo-1.0-SNAPSHOT.jar`.

A successful build displays:

```text
BUILD SUCCESS
```

## 4. GitHub Actions Workflow — `maven.yml`

The workflow automates the Maven build whenever code is pushed to or a pull request is made against the `main` branch.

**Workflow location:** `.github/workflows/maven.yml` at the repository root.

To open the workflow:

```bash
cd ~/Documents/DevOps
code .github/workflows/maven.yml
```

### Workflow Code

```yaml
name: Java CI/CD Pipeline

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn -f Experiment-5-Maven/maven-demo/pom.xml clean package
```

### Explanation of the Workflow

- `name` — specifies the workflow name.
- `on` — defines when the workflow runs.
- `push` — triggers the workflow when code is pushed to `main`.
- `pull_request` — triggers the workflow for pull requests targeting `main`.
- `runs-on` — selects the Windows runner.
- `actions/checkout@v4` — checks out the repository code.
- `actions/setup-java@v4` — sets up JDK 21.
- `mvn clean package` — builds and packages the Maven project using `pom.xml`.

## 5. Push the Project to GitHub

From the repository root, run:

```bash
cd ~/Documents/DevOps
git add Experiment-5-Maven .github/workflows/maven.yml
git commit -m "Add Experiment 5 Maven project and workflow"
git push origin main
```

If Git reports that there is nothing to commit, your changes may already be committed.

## 6. Verify GitHub Actions

1. Open the GitHub repository.
2. Click the **Actions** tab.
3. Open **Java CI/CD Pipeline**.
4. Select the latest workflow run.
5. Open the `build` job.
6. Verify that the checkout, JDK setup, and Maven build steps complete successfully.

A green check mark indicates that the workflow completed successfully.

## Expected Output

- Maven project created successfully.
- Java source code compiled successfully.
- Unit tests executed successfully.
- JAR artifact generated in the `target/` directory.
- GitHub Actions automatically triggered by a push or pull request.
- Workflow completed successfully.

## Result

The Java project was created and built successfully using Apache Maven. Dependency management and automated build execution were demonstrated using Maven and GitHub Actions.


## Mam code for maven 

name: Java CI/CD Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'

      - name: Build with maven
        run: mvn clean package


## mam code for new.java

public class New {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}