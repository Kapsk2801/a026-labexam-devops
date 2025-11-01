# Jenkins Configuration Guide

This guide walks you through setting up Jenkins for this Spring Boot Maven project.

## Prerequisites
- Jenkins server installed and running
- Java Development Kit (JDK) installed on Jenkins server
- Maven installed on Jenkins server (or configured through Jenkins)

---

## 1. Configure Maven as a Global Tool in Jenkins

### Steps:
1. **Login to Jenkins** dashboard
2. Navigate to **Manage Jenkins** → **Tools** (or **Global Tool Configuration**)
3. Scroll down to the **Maven** section
4. Click **Add Maven**
5. Configure:
   - **Name**: `Maven-3.9.6` (or your preferred name)
   - **Install automatically**: Check this box
   - **Version**: Select `3.9.6` (or latest stable version)
   - **Install from Apache**: Select this option
6. Click **Save**

### Alternative: Manual Maven Installation
If you prefer to use a manually installed Maven:
- **Name**: `Maven-3.9.6`
- **MAVEN_HOME**: `/path/to/maven` (path to your Maven installation)
- Do NOT check "Install automatically"

**Note**: The Jenkinsfile references `Maven-3.9.6` - ensure the name matches your configuration.

---

## 2. Create the First Build Job

### Option A: Freestyle Project
1. Click **New Item** on Jenkins dashboard
2. Enter a name (e.g., `a026-jenkins-build`)
3. Select **Freestyle project**
4. Click **OK**
5. Configure:
   - **Source Code Management**: Configure if using Git/SVN
   - **Build** → **Add build step** → **Invoke top-level Maven targets**
     - **Maven Version**: Select `Maven-3.9.6`
     - **Goals**: `clean compile`
   - **Post-build Actions** → **Archive the artifacts**
     - **Files to archive**: `target/**/*.jar`
6. Click **Save**
7. Click **Build Now** to test

---

## 3. Create Test Job and Delivery Pipeline

### Using Pipeline Job:
1. Click **New Item**
2. Enter name: `a026-jenkins-pipeline`
3. Select **Pipeline**
4. Click **OK**
5. In **Pipeline** section:
   - **Definition**: Select **Pipeline script from SCM**
   - **SCM**: Select your version control (Git, etc.)
   - **Repository URL**: Your repository URL
   - **Script Path**: `Jenkinsfile`
6. Click **Save**
7. Click **Build Now**

### Pipeline Stages:
- **Build**: Compiles the application
- **Test**: Runs JUnit tests and publishes results

---

## 4. Pipeline as Code - Scripted Pipeline

The `Jenkinsfile.scripted` file contains a scripted pipeline example using Groovy DSL.

**To use:**
- Copy contents of `Jenkinsfile.scripted` into a Pipeline job's script section
- Or reference it in SCM with the script path set to `Jenkinsfile.scripted`

**Features:**
- Uses Groovy scripting syntax
- More flexible for complex logic
- Direct node() and stage() calls

---

## 5. Pipeline as Code - Declarative Pipeline

The `Jenkinsfile` (root directory) contains a declarative pipeline.

**Features:**
- Cleaner, more readable syntax
- Predefined structure (pipeline { })
- Better error handling and validation
- Recommended approach for most projects

**To use:**
- Ensure the file is named `Jenkinsfile` in your repository root
- Jenkins will automatically detect and use it when configured with "Pipeline script from SCM"

---

## 6. Running the Pipeline

### From Jenkins UI:
1. Navigate to your Pipeline job
2. Click **Build Now**
3. Click on the build number to view progress
4. Click **Console Output** to see detailed logs

### From Git (with webhook):
1. Configure webhook in your Git repository pointing to Jenkins
2. Push changes to trigger automatic builds

---

## Troubleshooting

### Maven not found:
- Verify Maven tool name matches in Jenkinsfile (`Maven-3.9.6`)
- Check Maven is configured in Global Tool Configuration
- Ensure Maven path is correct

### Build fails:
- Check Java version compatibility (project uses Java 25)
- Verify all dependencies are available in Maven repository
- Check console output for specific error messages

### Tests not running:
- Ensure `pom.xml` has test dependencies
- Check `target/surefire-reports` directory exists after test execution
- Verify test class naming conventions (ends with `Test` or `Tests`)

---

## Additional Configuration Options

### Adding More Stages:
```groovy
stage('Package') {
    steps {
        sh 'mvn package'
    }
}

stage('Deploy') {
    steps {
        echo 'Deploying application...'
        // Add deployment steps
    }
}
```

### Environment Variables:
```groovy
environment {
    JAVA_HOME = '/usr/lib/jvm/java-25'
    BUILD_NUMBER = "${env.BUILD_NUMBER}"
}
```

---

## References
- [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)
- [Maven Plugin Documentation](https://plugins.jenkins.io/maven-plugin/)
- [Declarative Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)

