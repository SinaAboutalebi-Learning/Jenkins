# Jenkins Playground

Welcome to my Jenkins Playground! This repository serves as a tutorial and documentation hub for learning and experimenting with Jenkins.

## Installation

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
