# DevOps-Project : Milestone 1 (M1)
## Introduction ##

We have used Jenkins as the Build Server for this Milestone. We have used a Digital Ocean Droplet - Ubuntu 14.04 x64 to host Jenkins Server. We used a sample Apache Maven Project with a single Junit Test for Build task.

Our github repo has two branches which would be tracked by two different Jenkins jobs. Whenever a local commit is pushed to a remote branch, the build job corresponding to that branch will be triggered in Jenkins via Git Service Hooks configured in the GitHub repo.

We have configured two jobs to display two scenarios:
* The first branch called mavenSuccess would be tracked by a Jenkins job called M1-mavenSuccess. This job is
configured such that the build would be triggered by the Service Hook when a commit is pushed to mavenSuccess branch. For the post build task, we have sent email notifications to a list of users with custom subject and content indicating 'Successful Build' if the end result of the build was 'Success'. 
* The second branch called mavenFailure would be tracked by a Jenkins job called M1-mavenFailure. This job is configured such that the build would be triggered by the Service Hook when a commit is pushed to mavenFailure branch. For the post build task, we have sent email notifications to a list of users when the build status is either 'Failure' or 'Fixed'.

For the ability to send email notifications, we configured a SMTP Server on our Digital Ocean Droplet by following this [link] (https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-14-04)

```
NOTE:
1. The code and config files for two jobs will be in two other branches of this repo i.e. mavenSuccess and mavenFailure which are being tracked by two jenkins jobs
2. Please refer to TEAM.md for Team information and Task distribution
```

##Build##

#### Create a Digital Ocean Droplet for hosting the Jenkins Server ####
* ssh into the image and install the pre-requisites mentioned below:
  * Install jdk <br/> `sudo apt-get install openjdk-7-jdk`
  * Install git <br/> `sudo apt-get install git`
  * Install maven <br/> `sudo apt-get install maven`
  * Install mailutils [link] (https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-as-a-send-only-smtp-server-on-ubuntu-14-04) <br/>
  * Install Jenkins using the following commands<br/> 
    ```
    wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add - 
    sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt-get update
    sudo apt-get install jenkins
    ```
  * Test if Jenkins server is up at the following URL<br/> `http://<droplet_ip>:<jenkins_port>`
#### Configuring Jenkins ####
   * Manage Jenkins --> Configure Global Security --> Enable security --> Select Jenkin's own user database under Security Realm --> Allow users to sign up --> Matrix based security in Authorization --> Add user and give it all permissions --> Apply
   * Go to Jenkin's home and sign up (Provide details like username, password, email, etc)
   * Go to Manage Jenkins --> Configure Global Security --> Uncheck 'Allow users to sign up' 
   * Manage Jenkins --> Manage Plugins --> Install GitHub plugin
   * Manage Jenkins --> Manage Plugins --> Email Extension plugin
   * Manage Jenkins --> Configure System --> Add JDK with a Name like 'OpenJDK' and the path to your jdk home --> Save
   * Manage Jenkins --> Configure System --> Enter an email in System Admin e-mail address under Jenkins Location --> Save
   * Manage Jenkins --> Configure System --> Add Maven installation with a Name like 'Apache Maven 3.0.5' and the path to your Maven home --> Save
   * Manage Jenkins --> Configure System --> Add localhost as the SMTP Server in Email Notification --> Save
  
#### Configuring GitHub Web Hook and Services ####
   * Go to GitHub Settings --> Webhooks & Services --> Add Service - Jenkins (GitHub plugin) --> Enter Jenkins Hook URL as <br/>
`http://<droplet_ip>:<jenkins_port>/github-webhook/`

#### Configuring Jenkins Job ####
   * Go to Jenkins Home --> New Item --> Enter a name and select Maven project 
   * Select Git under Source Code Management --> Enter your GitHub Repo --> Specify branchname in 'Branches to Build'
   * Select 'Build when a change is pushed to GitHub ' under Build Triggers
   * Add the Root POM and Goals i.e. `clean install` in Build
   * Add Post-Build action --> Editable Email Notification --> Configure email options as needed
 
 #### Testing Jenkins Build Server ####
   * Clone the repo
   * Checkout to the concerned branch
   * Make changes in either pom.xml or .java file 
   * Add, Commit and Push the changes
   * Build will be triggered in Jenkins
   * Go to the job tracking this branch and check the console output, status and email

## Screencast Link ##

[Demo] (https://youtu.be/BcmmR_9KW-g)

