# Royal Rangers [Jenkins]

We are using basic Jenkins Docker image from [here](https://github.com/jenkinsci/docker)

## Usage
```
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

## Additionally install Jenkins Plugins

- Publish Over SSH [here](http://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin)
- SSH plugin [here](https://wiki.jenkins.io/display/JENKINS/SSH+plugin)

## Manage Jenkins

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

## Jenkins Jobs

### REBUILD_rr-api 
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

### REBUILD_rr-web-app 
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

### HEALTH_CHECK_rr-main 
*Freestyle project*

1. Go to ****Build Environment****
2. Check ****Send files or execute commands over SSH before the build starts**** 
3. Add exec command, e.g.:

```
printf "\Show list of containers\n"
docker ps -a
echo "Done!"
```

```
printf "\nCheck Available Space\n"
df -h
echo "Done!"
```

```
printf "\Check swap & memory\n"
free -h
echo "Done!"
```




