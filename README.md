![1_Cf-zPiRY_0_NW9LpCYB6dA](https://user-images.githubusercontent.com/66811679/85195340-b4677280-b28e-11ea-8649-03af2630c340.png)

## Devops task 3 :

## PREREQUISITE:

1.RHEL 8 (GUI/CLI)

2.Docker-CE

3.Git

4.Jenkins

5.Kubernetes (Should be configured on Windows and RHEL)

We will start by creating a repository on GitHub. We will use web-hook in GitHub so that whenever developer pushes any update, Job 1 will automatically starts building on Jenkins

![Capture](https://user-images.githubusercontent.com/66811679/85364826-3d75e800-b4e1-11ea-8579-40d6c973ca8d.PNG)

Now we have to build a docker image which will have Kubernetes installed and running on top of Cent OS. We also need to install Git, Java, SSH-Server and some certificates which are required in config file of Kubectl.

![0](https://user-images.githubusercontent.com/66811679/85364904-72823a80-b4e1-11ea-9a52-afa61c7848bd.png)

![2](https://user-images.githubusercontent.com/66811679/85365115-eae8fb80-b4e1-11ea-8fd8-dacf11a62375.PNG)

We need to allow Docker daemon to be accessed remotely. Since docker is isolated. For that we need to edit  [/usr/lib/systemd/system/docker.service.]

Here 0.0.0.0 means that you can denote it with any IP given to the DOCKER HOST and you can assign this IP with any available port.

![3](https://user-images.githubusercontent.com/66811679/85371166-0efe0a00-b4ed-11ea-8b22-2212e1caaed1.png)

After this process, Restart the services

1.[systemctl daemon-reload
2.[systemctl restart docker.service
Now we need to export DOCKER_HOST. Use command

1.[export DOCKER_HOST=<your IP address>:<port you assigned>]
2.[echo $DOCKER_HOST]
Now we need to setup Docker Cloud on Jenkins. Before that we need to download some plugins on Jenkins

1.[DOCKER]
2.[GITHUB]
3.[WORKSPACE CLEANUP]
Now to setup Docker cloud on Jenkins. Go to following section.

Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Add a new cloud

![4](https://user-images.githubusercontent.com/66811679/85371307-3d7be500-b4ed-11ea-8682-aecc655def91.png)

Docker Host URI is the remote docker host where all the containers run.


## [JOB1:]

In this job it will download the files from GitHub using repository URL. The GitHub repository contains Dockerfile 2 and HTML file. The workspace will be cleaned before every build and then it will pull repository. Job 2 will build after the successful build Job 1.

In the Build action we are using Build / Publish Docker Image. In this directory is set to default workspace. You need to give name of docker cloud which you assigned in the managed nodes and clouds. Give your docker image name and give credentials of Docker Hub and then enable push image.

## [Dockerfile 2]

![5](https://user-images.githubusercontent.com/66811679/85371377-5ab0b380-b4ed-11ea-9757-e6f784dc356e.png)

![6](https://user-images.githubusercontent.com/66811679/85371438-76b45500-b4ed-11ea-9206-76fb0920ae1d.png)

## [OUTPUT OF JOB 1:]

![7](https://user-images.githubusercontent.com/66811679/85371520-95b2e700-b4ed-11ea-927b-470a33f3dc37.png)

As you can see my image is successfully pushed to Docker Hub.

![8](https://user-images.githubusercontent.com/66811679/85371560-ab281100-b4ed-11ea-9ced-74b853cd70fc.png)

The files used in Job 1 are present in this GitHub repository.

Config file used in Dockerfile 1 for kubectl


![9](https://user-images.githubusercontent.com/66811679/85371605-c1ce6800-b4ed-11ea-8b75-07db9282b161.png)



Now we will add Docker template in the Docker Cloud settings. In Docker template we need to give a label which we will use during configuration of Job 2 and we need to give right path of our Dockerfile 1. In container settings we need to give right path of the config file which is present in your RHEL.

[Container Settings > Volumes]

[/root/task3:/root/.kube/]

**Right side is the path where we have mounted config file that is to be used in Docker container. Left side is the path where we have mounted config file in RHEL.

![11](https://user-images.githubusercontent.com/66811679/85371708-ede9e900-b4ed-11ea-8542-b99af4cc456d.png)

## [JOB 2:]

This job will be build after successful build of Job 1. This job has to run on dynamic container slave for that we have already added a template on Docker cloud. This job will create a new deployment (if it does not exist) or else we will roll out update to the existing deployment. Since we our building this job for first time, So it will create a new deployment.

![12](https://user-images.githubusercontent.com/66811679/85371779-107c0200-b4ee-11ea-892a-3fba4541cae3.png)

It will use label which we have created on Docker cloud.

![13](https://user-images.githubusercontent.com/66811679/85371856-2d183a00-b4ee-11ea-8c62-98733483431e.png)

To check whether kubectl is configured properly or not, We will use command "kubectl --help" and then we will write the code which will help in creating new deployment.

**Before running this job, Make sure to start minikube from windows command line and kubectl should be properly configured on RHEL and make sure you have good internet speed.


## [Output of JOB 2:]


![14](https://user-images.githubusercontent.com/66811679/85372227-c47d8d00-b4ee-11ea-8ee5-c05dda52b9f8.png)


Now we have to check on minikube, Whether deployment created successfully, web server is exposed and replicas of deployment is created



![16](https://user-images.githubusercontent.com/66811679/85372381-fbec3980-b4ee-11ea-93f9-8601207ccfde.png)


Web page is also running successfully.


![18](https://user-images.githubusercontent.com/66811679/85378307-9d778900-b4f7-11ea-8091-46a9aace484a.png)


![22](https://user-images.githubusercontent.com/66811679/85381909-ae29fe00-b4fb-11ea-9294-bd35c0a1f76d.PNG)



