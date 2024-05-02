# jenkins_pipeline
### Continuous Integration with Jenkins and Git Flow
This project demonstrates how to set up continuous integration (CI) using Jenkins and apply the Git Flow branching model for managing the development process.

### Tools Needed:
     - Jenkins
     - GitHub account
     - Git installed on the Jenkins server
     - AWS EC2 instance

### Setup Instructions:

## GitHub Repository Setup:
    1.Create a GitHub Repository:
      Go to GitHub and create a new repository. Let's name it my-project.

    2.Ensure the repository contains at least two branches:
      - develop for ongoing development.
      - master (or main) for stable releases

    3.Apply the Git Flow model:
      -Install Git Flow tools if not already installed:
      ```bash
     git clone -b feature/https-remote
      https://github.com/jgonggrijp/gitflow.git
     curl -OL
      https://raw.github.com/jgonggrijp/gitflow/develop/contrib/git
       flow-installer.sh
    chmod +x gitflow-installer.sh
       sudo ./gitflow-installer.sh
    ```

4.Clone the Repository:
  
    ```bash
    git clone https://github.com/your-username/my-project.git
    ls
    cd <repo-name>
   
    ```
# initialize git flow
    ```bash
    git flow init
    ```
 # make sure that you are on the develop branch
     ```bash
     git branch  
    ```

5.Add a Jenkinsfile:

-Create a file named "Jenkinsfile" in the root of your project with the following content:
 
  ```bash
       pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
  ```

6.Launch an AWS EC2 instance:
  # Make this configurations
    
    -Create new key
    -security:   # after you launch the instance 
    inbound rule:
     - custom tcp
     -port:8080
     -source:0.0.0.0/0
 
  # Install Jenkins on EC2:
    
    1.Open the terminal
    2.Connect to your Ec2 instance:
     ```bash
         ssh -i  <The path of the key file>  ec2-user@<EC_instance_public_ip> 
     ```
   
### Jenkins Setup:

    1.Install Jenkins
      ```bash
          sudo yum install java-17-amazon-corretto-devel -y
         sudo wget -O /etc/yum.repos.d/jenkins.repo \
          https://pkg.jenkins.io/redhat-stable/jenkins.repo
         sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
         sudo dnf upgrade
         sudo dnf install jenkins
         sudo systemctl daemon-reload
        # start and enable the service 
        systemct enable jenkins.service
        systemctl start jenkins.service
        systemctl status jenkins.service
    ```

    2.Configure Jenkins:
    Open Jenkins in your web browser (usually at http://localhost:8080).
    Log in and navigate to "Manage Jenkins" > "Manage Plugins" > "Available".
    Install the following plugins:
       -GitHub Integration Plugin
       -Pipeline
   
    3.Configure GitHub Integration:
     -Go to "Manage Jenkins" > "Manage Credentials" > "Global credentials" and add your GitHub credentials.
     -Go to "Manage Jenkins" > "Configure System" > "GitHub" section and add your GitHub credentials.
   
    4.Create a New Pipeline Job:
      Click on "New Item" on the Jenkins dashboard.
     Enter a name for your job (e.g., "My Project Pipeline") and select "Pipeline" as the type.
      In the configuration page:
         -Select "Pipeline script from SCM" under "Pipeline".
         -Choose "Git" as the SCM.
         -Enter your GitHub repository URL (e.g., https://github.com/your-username/my-project.git).
         -Specify the branch to build (e.g., */develop).
         -Click "Save" to create the job.
    
     5.Set Up Webhook:
       In your GitHub repository, go to "Settings" > "Webhooks".
       Click on "Add webhook".
       Set "Payload URL" to http://<your-jenkins-url>/github-webhook/.
       Set "Content type" to application/json.
       Select "Just the push event".
       Click "Add webhook" to save.

  ### Testing and Validation:

    1. Push a change to the `develop` branch of your GitHub repository.
    2. Verify that Jenkins automatically triggers a build and processes according to the steps defined in the Jenkinsfile.
    3. Check the output in Jenkins to ensure the build and test stages are executed successfully.
