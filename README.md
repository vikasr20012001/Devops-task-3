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

We need to allow Docker daemon to be accessed remotely. Since docker is isolated. For that we need to edit /usr/lib/systemd/system/docker.service.

Here 0.0.0.0 means that you can denote it with any IP given to the DOCKER HOST and you can assign this IP with any available port.
