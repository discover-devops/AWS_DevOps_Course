NOTE: AWS has ceased onboarding new customers to AWS CodeCommit as of July 25, 2024. This means that while existing users can continue to use the service, no new accounts can create CodeCommit repositories.

![image](https://github.com/user-attachments/assets/fc7ac995-6cad-4797-8988-311cc6add283)


---

##  ** Continuous Integration (CI) on AWS**

---

###  Why CI/CD?

In traditional development, whenever developers finished writing code, they would manually upload or deploy it to a server. This was slow, error-prone, and often led to bugs slipping into production.

As projects grew larger and involved multiple developers, this approach became unsustainable.

That’s where **Continuous Integration (CI)** comes in:

* Developers frequently **push their code** to a shared repository.
* Each time they push, **automated processes** verify the code.
* If tests pass, the new version moves closer to deployment.

By automating these steps, teams release features faster, with fewer errors.

---

##  **AWS CI/CD Toolchain Overview**

When using AWS for DevOps automation, these are your core tools:

| Tool             | Purpose                                    |
| :--------------- | :----------------------------------------- |
| **CodeCommit**   | Private Git repositories                   |
| **CodeBuild**    | Build and test your code automatically     |
| **CodeDeploy**   | Deploy code to servers (EC2, Lambda, etc.) |
| **CodePipeline** | Orchestrate the entire CI/CD workflow      |

Today, we'll focus on **CodeCommit** and **CodeBuild**.

---

##  Step 1: Setting Up Version Control — *CodeCommit*

**CodeCommit** is AWS’s Git-based repository service.
It provides:

* Private, secure Git repositories.
* Easy integration with other AWS services.
* IAM-based access control.
* Encryption at rest and in transit.

**Example Scenario:**
Let’s say you work for *BrightApps Inc.*, and your team is building an app called *WeatherTrack Pro*. You want to store the app’s code in AWS.

**Steps:**

1. Open the AWS Management Console.

2. Go to **CodeCommit** → **Create Repository** → Name it `weathertrack-pro`.

3. Initialize the repository locally using Git commands:

   ```bash
   git init
   git remote add origin https://git-codecommit.<region>.amazonaws.com/v1/repos/weathertrack-pro
   ```

4. Push your first code files to the repository.

 *Tip:* Use **SSH keys** or **IAM credential helpers** for secure authentication.

---

##  Step 2: Setting Up Automated Builds — *CodeBuild*

**CodeBuild** compiles your code, runs tests, and produces deployable packages.

**Example Scenario:**
Now that `weathertrack-pro` is in CodeCommit, you want to automatically:

* Install dependencies.
* Run tests.
* Package the app.

**Steps:**

1. Open **CodeBuild** → **Create Build Project**.
2. Name: `weathertrack-build`.
3. Source provider: **AWS CodeCommit**.
4. Repository: `weathertrack-pro`.
5. Environment: Choose a managed image (e.g., Ubuntu with standard runtimes).
6. Buildspec file: Use a `buildspec.yml` at the root of your repository.

**Sample `buildspec.yml`:**

```yaml
version: 0.2
phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - npm install
  build:
    commands:
      - npm test
      - npm run build
artifacts:
  files:
    - '**/*'
```

This example assumes you're working with a Node.js app. You can adjust it for Java, Python, or other languages.

7. Start the build. CodeBuild will:

   * Pull the latest code from CodeCommit.
   * Install dependencies.
   * Run unit tests.
   * Package the build artifacts.

---

##  Step 3: Integrating CodeCommit with CodeBuild

To automate the build every time code changes:

1. Go back to **CodeBuild** → Select your project → Edit.
2. Under **Source**, enable **Webhook**.
3. This connects CodeCommit and CodeBuild.
   Whenever a developer (say, *Priya* or *Arjun*) pushes new code, the build starts automatically.

---

##  Step 4: Unit Testing Automation

**Why automate tests?**

* Quickly catch bugs.
* Reduce manual effort.
* Ensure new code doesn’t break existing features (**regression testing**).

**Example:**
In your `package.json` (for Node.js apps), you might have:

```json
"scripts": {
  "test": "jest"
}
```

In the `buildspec.yml`, the command:

```bash
npm test
```

will automatically run these tests during the build phase.

 *If tests pass*: CodeBuild moves to the next steps.
 *If tests fail*: CodeBuild stops the process, preventing bad code from being deployed.

---

##  Summary

| Step                             | Tool                            | Purpose                     |
| :------------------------------- | :------------------------------ | :-------------------------- |
| **Store Code**                   | CodeCommit                      | Version control             |
| **Automated Builds & Tests**     | CodeBuild                       | Compile, test, package code |
| **Trigger Builds Automatically** | CodeCommit Webhooks → CodeBuild | CI workflow                 |
| **Automate Testing**             | CodeBuild (via buildspec.yml)   | Unit tests                  |

---

##  Lab Exercise Suggestion

**Objective:**
Build a complete CI pipeline that:

* Stores code in CodeCommit.
* Automatically triggers CodeBuild on each code push.
* Runs basic tests and packages the application.


---

##  Why is this important for DevOps?

By learning this:

* You automate the **testing and building** of code.
* Developers can focus on writing features instead of worrying about deployment.
* Bugs are caught **early**, reducing production issues.
* You lay the groundwork for **full CI/CD pipelines** — the core of DevOps automation.

---


