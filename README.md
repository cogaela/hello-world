# hello-world
Just another repository


Detailed Instructions

For setting up Jenkins to build GitHub projects. This assumes some ability to manage Jenkins, use the command line, set up a utility LDAP account, etc. Please share or improve this Gist as needed.

Install Jenkins Plugins

get both the git and github plugin
http://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin
http://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin
Make a Utility Account in the GitHub (optional)

This would keep your identity separate from your Jenkins server
Helpful if Jenkins runs under a different user
On the Jenkins server (optional)

If Jenkins does run as different user, set this up on your server
Set git user.name and user.email global config options.
'git config --global user.email JENKINS_USERNAME@WHATEVER_HOSTNAME'
'git config --global user.name JENKINS_USERNAME'
This should match the GitHub utility account username and email address
Generate rsa key pair on your Jenkins server

Log into the server as the user that Jenkins runs under
Using command line, change directory to ~/.ssh
Generate key with 'ssh-keygen -t rsa -C 'JENKINS_USERNAME@WHATEVER_HOSTNAME''
Note that -C comment is optional, just helps you identify the public key later
Do NOT use a passphrase for the keygen (at least, I could not make this work)
If there's a way to use a key pair with passphrase, please comment and let me know?
Init with 'ssh -v git@git.corp.adobe.com'
Reply 'yes' to add known host
Copy contents of public key
There's more reading about this on GitHub https://help.github.com/articles/set-up-git https://help.github.com/articles/generating-ssh-keys https://help.github.com/articles/error-permission-denied-publickey
Update the GitHub Account for Jenkins

Register utility user with git.corp.adobe.com
Log in the utility account
Go to Account Settings > SSH Keys
Add the contents of the public key from your server
Configure Jenkins

Make sure the Manage Jenkins > Configure System has the right path to git
Set the global git user.name and user.email to match your global config options
Configure GitHub Web Hook to Manually manage hook urls
Click the (?) icon next to the manual option and copy the hook URL you see there
Optionally set the service account email as the Jenkins sender email address
Configure Github Project

Log into Github as the owner or collaborator of a repo
Click the Admin button for that repo
Select 'Service Hooks' in the left column
Select 'Jenkins (Github plugin)' in the Available Service Hooks column
Add the service hook URL for your server eg http://yourjenkinsmachine.com:8080/github-webhook/ you might find the URL in the GitHub Web Hook > Manual setting (?) help text
Check the 'Active' checkbox
Create a Jenkins job

In the general options, add the URL to the github project eg 'http://github.com/tehfoo/build-test/'
Under Source Code Management, select Git
For the repository URL, enter the SSH URL for your project this is found by toggling the HTTP/SSH button to the right of the URL field on the repo home page in GitHub eg 'git@github.com:ixab/build-test.git'
Under Build Triggers, check the "Build when a change is pushed to GitHub"
Add other build task (running Grunt, or Ant, or scary Maven stuff, or whatever)
