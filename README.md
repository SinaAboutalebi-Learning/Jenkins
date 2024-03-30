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


## Understanding Workspace in Jenkins Pipelines 📁

In Jenkins pipelines, the workspace directory is a designated location where Jenkins stores files related to the current build. It serves as the working directory for all build steps and provides a clean environment for performing build tasks.

Here's how you can utilize the workspace directory in your Jenkins pipeline:

1. **Accessing Workspace**: Within your pipeline script, you can reference the workspace directory using the `WORKSPACE` environment variable. For example, `${WORKSPACE}/src` refers to the `src` directory within the workspace.

2. **Manipulating Files in Workspace**: You can perform various file operations, such as creating files, within the workspace directory. These operations ensure that your build artifacts are stored in a centralized location.

Let's demonstrate how to add a step to manipulate files in the workspace:

1. **Navigate to Pipeline Configuration**: Open your pipeline configuration by clicking on the pipeline name and then selecting "Configure" from the left menu.

2. **Add Build Step**: In the build section, add an "Execute shell" build step by clicking on "Add build step" and selecting "Execute shell" from the dropdown.

3. **Manipulate Files**: In the script area, enter commands to manipulate files in the workspace directory. For example, you can echo something to a file and then list the contents of the workspace. Here's an example:
    ```bash
    # Echo "Hello, Jenkins!" to a file named "message.txt" in the workspace
    echo "Hello, Jenkins!" > ${WORKSPACE}/message.txt
    
    # List the contents of the workspace
    ls ${WORKSPACE}
    ```

    ![Build Step 3](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/BuildStep3.png?raw=true)

4. **Save Configuration**: Save the pipeline configuration to apply the changes.

5. **Run Pipeline**: Once the configuration is saved, trigger the pipeline execution by clicking on "Build Now".

After the pipeline execution completes, you can view the contents of the workspace directory and see the manipulated files.

![Console Output 2](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/ConsoleOutput2.png?raw=true)

### Viewing Workspace in Pipeline Menu

After running the pipeline, you can view the workspace directory in the pipeline menu. Simply click on the pipeline job name, and you'll find the "Workspace" link in the left menu. Clicking on this link will take you to the workspace directory for the specific build, where you can explore the files generated during the build process.

![WorkSpace View](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/WorkSpace.png?raw=true)

Understanding and utilizing the workspace directory effectively allows you to organize build artifacts and perform build tasks efficiently in Jenkins pipelines.


## Exploring Jenkins File System 📁

Jenkins provides a file system within its Docker container where various configurations, jobs, and workspace directories are stored. Here's how you can explore the Jenkins file system:

1. **Connect to Jenkins Docker Container**: Open your terminal and run the following command to connect to the Jenkins Docker container and get a bash command-line interface inside the container:
   ```bash
   docker exec -it jenkins bash
   ```
2. **Navigate to Jenkins Home Directory**: Once you're inside the container's bash interface, use the cd command to navigate to the Jenkins home directory:
    ```bash
    cd /var/jenkins_home
    ```
3. **List Directory Contents**: Run the `ls` command to view the contents of the Jenkins home directory. You'll see various directories and files related to Jenkins configurations and jobs.

4. **Explore Workspace Directory**: One important directory within Jenkins is the workspace directory, where build-related files and artifacts are stored. Navigate to the workspace directory using the following command:
    ```bash
    cd workspace
    ```
5. **List Pipeline Directory**: Within the workspace directory, you'll find directories named after your pipeline jobs. Navigate into the directory corresponding to your pipeline job using the `cd` command.

6. **View Contents**: Once inside the pipeline directory, run the `ls` command to view its contents. You'll typically find build artifacts, logs, and any files generated during the build process.

7. **View File Content**: If you've created files as part of your pipeline job, you can use the `cat` command to view their contents. For example, to view the content of a file named `message.txt`, run:
    ```bash
    cat message.txt
    ```

    ![Jenkins Terminal](https://github.com/SinaAboutalebi-Learning/Jenkins/blob/main/images/jenkinsTerminal.png?raw=true)

Exploring the Jenkins file system gives you insight into where Jenkins stores its data and how pipelines interact with the workspace directory.
