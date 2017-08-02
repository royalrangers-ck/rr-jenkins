# Royal Rangers [Jenkins]

We are using basic Jenkins Docker image from [here](https://github.com/jenkinsci/docker)

## Getting started

1) Download and run docker image

```
docker run -p 8080:8080 jenkins/jenkins:lts
```

2) Copy password from console. It will looks like this:

```
Jenkins initial setup is required. An admin user has been created and a password generated. 
Please use the following password to proceed to installation:                               
                                                                                            
e6ba76b405f848678a9f3f65ce858500                                                            
                                                                                            
This may also be found at: /var/jenkins_home/secrets/initialAdminPassword                   
```

3) Navigate to [localhost:8080](http://localhost:8080) and paste the password

> Note, if you're using Docker Toolbox, navigate to [192.168.99.100:8080](http://192.168.99.100:8080)

4) Hit "Install suggested plugin" button

5) Create first admin user by setting next data:

```
Username:	admin
Password:	admin
Full name:	admin
E-mail address:	admin@gmail.com
```

6) Add [ThinBackup]() and [SaveRestart](https://plugins.jenkins.io/saferestart) plugins

7) Clone [rr-jenkins](https://github.com/royalrangers-ck/rr-jenkins) repository

> Note, it might take a while as the Jenkins backup file is quite big

```
 git clone -b master https://github.com/royalrangers-ck/rr-jenkins.git
```

8) Copy backup to the Jenkins container:

```
docker cp c:\workspace\rr-jenkins\backup\ f02f25169709:/tmp/
```

9) Restore backup using ThinBackup plugin

> Note, don't forget to set backup directory to: /tmp/backup

10) Hit "Safe Restart" button

## Manage Jenkins

### Additionally install Jenkins Plugins

- Publish Over SSH [here](http://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin)
- SSH plugin [here](https://wiki.jenkins.io/display/JENKINS/SSH+plugin)

### Configure Jenkins

1. Go to Manage Jenkins > Configure System > Publish over SSH
2. Insert your RSA PRIVATE KEY
3. Add your SSH Server, ex.:

```
name: rr-main
hostname: royalrangers.online
username: root
remote directory: /tmp
```

4. Click **Test Configuration** button to test your config!

### Jenkins Jobs

#### REBUILD_rr-api 
*Freestyle project*

1. Go to ****Build Environment****
2. Check ****Send files or execute commands over SSH before the build starts**** 
3. Add exec command, e.g.:

```
FILE_NAME="rr-api"
SCRIPT_PATH="/usr/src/app/rr-docker/scripts/$FILE_NAME.sh"

sudo chmod +x "$SCRIPT_PATH"
sudo sh "$SCRIPT_PATH"
```

#### REBUILD_rr-web-app 
*Freestyle project*

1. Go to ****Build Environment****
2. Check ****Send files or execute commands over SSH before the build starts**** 
3. Add exec command, e.g.:

```
FILE_NAME="rr-web-app"
SCRIPT_PATH="/usr/src/app/rr-docker/scripts/$FILE_NAME.sh"

sudo chmod +x "$SCRIPT_PATH"
sudo sh "$SCRIPT_PATH"
```

#### HEALTH_CHECK_rr-main 
*Freestyle project*

1. Go to ****Build Environment****
2. Check ****Send files or execute commands over SSH before the build starts**** 
3. Add exec command, e.g.:

```
printf "\nShow list of containers\n"
docker ps -a
echo "Done!"
```

```
printf "\nCheck Available Space\n"
df -h
echo "Done!"
```

```
printf "\nCheck swap & memory\n"
free -h
echo "Done!"
```
