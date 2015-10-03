# DevOps-Project : Milestone 1 (M1)
### Introduction ###

We have used Jenkins as the Build Server for this Milestone. We have used a Digital Ocean Droplet - Ubuntu 14.04 x64 to host Jenkins Server. We used a sample Apache Maven Project with a single Junit Test for Build task.

Our github repo has two branches which would be tracked by two different Jenkins jobs. Whenever a local commit is pushed to a remote branch, the build job corresponding to that branch will be triggered in Jenkins via Git Service Hooks configured in the GitHub repo.

We have configured two jobs to display two scenarios:
1. The first branch called mavenSuccess would be tracked by a Jenkins job called M1-mavenSuccess. This job is configured such that the build would be triggered by the Service Hook when a push is made to mavenSuccess branch. For the post build task, we have sent email notifications to a list of users with custom subject and content indicating 'Successful Build' if the end result of the build was 'Success'.
2. The second branch called mavenFailure would be tracked by a Jenkins job called M1-mavenFailure. This job is configured such that the build would be triggered by the Service Hook when a push is made to mavenFailure branch. For the post build task, we have sent email notifications to a list of users when the build status is either 'Failure' or 'Fixed'.

For the ability to send email notifications, we configured a SMTP Server on our Digital Ocean Droplet.

### Build ###
#### Setup Steps ####

* Install jdk
* Install git
* Install maven
* Install mailutils
* Install Jenkins using the following commands<br/>`wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add - `<br/>
`sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'`<br/>
`sudo apt-get update `<br/>
`sudo apt-get install jenkins`
* Test if Jenkins server is up at the following URL<br/> `http://localhost:8080`
* Configuring Jenkins
  1. Manage Jenkins --> Configure Global Security --> Enable security --> Select Jenkin's own user database under Security Realm --> Allow users to sign up --> Matrix based security in Authorization --> Add user and give it all permissions --> Apply
  2. Go to Jenkin's home and sign up (Provide details like username, password, email, etc)
  3. Go to Manage Jenkins --> Configure Global Security --> Uncheck 'Allow users to sign up' 
  4. Set Maven path
  5. Set user email
  6. Set jdk path
  7. Manage Jenkins --> Plugins --> GitHub plugin
