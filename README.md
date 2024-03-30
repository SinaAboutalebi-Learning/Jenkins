# Jenkins Playground ✨

Welcome to my Jenkins Playground! This repository serves as a tutorial and documentation hub for learning and experimenting with Jenkins.

## Installation 🛠️

To get started with Jenkins, follow these simple steps:

1. **Pull the Jenkins Docker image**: This command will fetch the latest LTS (Long-Term Support) version of Jenkins with JDK 11 pre-installed.
    ```bash
    docker pull jenkins/jenkins:lts-jdk11
    ```

2. **Create a Jenkins container**: Run the following command to create a Docker container named "jenkins" based on the Jenkins image you just pulled. This command also exposes ports 8080 and 50000, maps a volume for persistent data, and sets the container to restart automatically on failure.
    ```bash
    docker run -itd --name jenkins -p 8080:8080 -p 50000:50000 --restart=on-failure -v jenkins-data:/var/jenkins_home jenkins/jenkins:lts-jdk11
    ```

3. **Access Jenkins Web UI**: Once the container is running, open your web browser and navigate to `http://DockerHostIP:8080`. You will be greeted with the Jenkins unlock page.

    ![Jenkins Unlock Page](https://www.jenkins.io/doc/book/resources/tutorials/setup-jenkins-01-unlock-jenkins-page.jpg)

4. **Get the Initial Admin Password**: To proceed with the installation, you need to retrieve the initial admin password. Execute the following command to obtain it:
    ```bash
    docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
    ```

5. **Follow the Installation Wizard**: Copy and paste the initial admin password obtained in the previous step into the Jenkins unlock page and click "Continue" to proceed with the installation wizard.

6. **Install Recommended Plugins**: In the next step of the installation wizard, you'll be prompted to install recommended plugins. Choose either "Install suggested plugins" to install the default set of plugins or "Select plugins to install" to customize your installation.

7. **Create an Admin User**: After plugin installation, you'll be prompted to set up your admin user account. Provide the required details and click "Save and Finish" to complete the setup.

8. **Start Using Jenkins**: Once the setup is complete, you'll be redirected to the Jenkins dashboard, where you can start creating and managing your projects.


## Creating Your First Pipeline 🚀

In this section, we'll guide you through creating your first pipeline in Jenkins using the web interface:

1. **Navigate to New Item**: On the left menu of the Jenkins dashboard, click on "New Item".
   
   ![New Item](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/NewItem.png?raw=true)

3. **Name Your Pipeline**: Enter a name for your pipeline, then select "Freestyle project" as the project type. For now, we'll start with a simple freestyle project.

   ![New Pipe Line](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/NamePipeline.png?raw=true)

5. **Configure Source Control Management (SCM)**: Under the "Source Control Management" section, choose "Git" and provide the repository URL. Make sure to change the default branch specifier from "*/master" to "*/main" if your repository uses the main branch as the default.

![SCM](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/SCM.png?raw=true)

6. **Build Triggers**: Due to potential difficulties with configuring GitHub webhooks for Jenkins behind a NAT or firewall, we'll opt for the "Poll SCM" option instead. This method periodically checks the source code repository for changes, similar to a cron job, ensuring that Jenkins can detect and trigger builds even in restrictive network environments.

7. **Build Environment**: Consider enabling "Delete workspace before build starts" to ensure a clean build environment for each execution.

8. **Build and Post-Build Actions**:
    - **Build Steps**: Here's where you define the actual build process. Click on "Add build step" and select the appropriate build step according to your project requirements. For example:
        - **Execute Shell**: If your project requires running shell commands, select this option and input the commands you want to execute. For example, `npm install` to install dependencies or `mvn clean install` to build a Maven project.
        - **Invoke Gradle script**: If your project is a Gradle-based project, use this option to invoke Gradle commands such as `gradle build`.
        - **Invoke Ant**: If your project uses Ant for building, use this option to invoke Ant targets.
        - **Invoke Python script**: If your project involves Python scripts, use this option to execute Python scripts.
        - **Windows Batch Command**: For Windows-based projects, use this option to execute batch commands.
    - **Post-Build Actions**: You can configure actions to be performed after the build is completed. This can include archiving artifacts, triggering other builds, sending notifications, etc. For example:
        - **Archive the artifacts**: Use this option to archive build artifacts such as JAR files, WAR files, etc., for later reference.
        - **Email Notification**: Configure Jenkins to send email notifications upon build completion, success, failure, etc.

    ![Build Step 1](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/BuildStep1.png?raw=true)

9. **Save and Build**: Once you've configured your pipeline settings, click "Save" to save the configuration. Then, click on "Build Now" to trigger the first build of your pipeline.

Congratulations! 🎉 You've successfully created and executed your first Jenkins pipeline. You should see the build status and output in the Jenkins dashboard.


## Using Build Environment Variables 💡

Jenkins provides a set of build environment variables that you can use within your pipeline scripts to access information about the current build. These variables can be particularly useful for tasks like versioning, tagging Docker images, or generating reports.

Here are some commonly used build environment variables:

- **BUILD_ID**: The ID number of the current build.
- **BUILD_URL**: The URL of the current build on the Jenkins server.
- **JOB_NAME**: The name of the job being executed.
- **BUILD_NUMBER**: The current build number, which increments with each new build.
- **WORKSPACE**: The absolute path of the workspace directory for the current build.

To use these variables in your Jenkins pipeline, you can reference them within your build steps. Let's demonstrate how to use `BUILD_ID` and `BUILD_URL` in our pipeline:

1. **Navigate to Pipeline Configuration**: Open your pipeline configuration by clicking on the pipeline name and then selecting "Configure" from the left menu.

   ![Pipeline Configuration](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/PipeLineMenu.png?raw=true)

3. **Add Build Step**: In the build section, add an "Execute shell" build step by clicking on "Add build step" and selecting "Execute shell" from the dropdown.

4. **Use Build Environment Variables**: In the script area, enter the following commands to print the build ID and build URL:
    ```bash
    echo "The Build ID of this job is ${BUILD_ID}"
    echo "The Build URL of this job is ${BUILD_URL}"
    ```

    ![Build Step 2](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/BuildStep2.png?raw=true)

5. **Save and Run Pipeline**: Save the pipeline configuration and click on "Build Now" to trigger the pipeline execution.

Once the pipeline runs, you will see the output in the build logs, displaying the build ID and build URL.

![Console Output](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/ConsoleOutput.png?raw=true)

Using build environment variables enhances the flexibility and automation capabilities of your Jenkins pipelines, allowing you to dynamically adapt your pipeline behavior based on the context of each build.
