[Install Steps](../)
# Docker Setup for Editing (Experimental)

Since Docker restarts local containers when your computer restarts, you may already have 
[http://localhost:3000](http://localhost:3000) running if you've added to Docker previously. The port and ghcr.io source is visible under details within Docker.

Our staging Docker container resides at [https://github.com/modelearth/projects/pkgs/container/projects](https://github.com/modelearth/projects/pkgs/container/projects)

Our production Docker container is in our [Github datascape account packages](https://github.com/users/datascape/packages/container/package/projects).


## Edit files pulled down from our Docker package

<!-- your project/location  -->
Once you get the following installed, see our [Location Projects for Open WebUI](../).

**ghcr.io** stands for GitHub Container Registry, which is a service provided by GitHub for hosting and managing container images.

<!--
Replaced open-webui/open-webui with modelearth/projects
Replaced open-webui-container with projects-container
-->

Here are commands to locally install [the Docker package](https://github.com/modelearth/projects/pkgs/container/projects) created from our "[projects](https://github.com/modelearth/projects)"  fork of open-webui.

<!--
The main Open WebUI container is at:
[https://github.com/orgs/open-webui/packages?repo_name=open-webui](https://github.com/orgs/open-webui/packages?repo_name=open-webui)

We're not an org, so this was not our URL: 
[https://github.com/orgs/modelearth/packages?repo_name=projects](https://github.com/orgs/modelearth/packages?repo_name=projects)
-->

Our Docker container live in our datascape repo.
This allows us to minimize the storage use in modelearth.

First [update your Docker, Node, Pip, Python and Conda](../../io/coders/python/)

**Run to pull the "projects" Docker container into your Webroot:**
<!-- ran in Documents/Webroot. Did not have 
	a projects or projects-container folder beforehand. -->

	docker pull ghcr.io/datascape/projects:main
	docker create --name projects-container ghcr.io/datascape/projects:main
	docker start projects-container
	docker exec -it projects-container /bin/bash

The above does the following:
1.) Pull the docker image
2.) Create a container called projects-container
3.) Run the docker container
4.) cd into the container

<!-- No folder is there yet, and the prompt now says: /app/backend# -->

IMPORTANT: Now open a new terminal window.

5.) Copy the files from the container into your webroot. If you aleady have a projects folder in your webroot, rename it.
The ~ in the command works for all machines types. On a PC you'll need to use powershell. 

The first line will create the folders if they do not yet exist.

	mkdir -p ~/Documents/Webroot/projects
	docker cp projects-container:/app/backend "Documents/Webroot/projects"

Run in projects folder to try downloading all. (Doesn't work yet.)
Currently just the Backend folder is set in the "Dockerfile" config settings.

	docker cp projects-container:/app

Note that we do not use \~/ in the command above since it already runs in your computer's [username] folder. 

Docker set-up contributors: Dinesh B, Loren, Yifeng, Yuxin<!--user download-->

## Using git inside of docker

**Code in Dockerfile to install Git while creating a Container**
	apt-get install -y --no-install-recommends curl jq git
	apt-get install -y --no-install-recommends pandoc gcc netcat-openbsd curl jq git

**Running Git inside container**
Once you've cloned the repo make a docker image and then create a container
	docker build -t openwebui:latest .
	docker create --name openwebui-container openwebui:latest
	docker start openwebui-container

**Initialize new Git repository**
	git init

**Add a remote repository**
	git remote add origin https://github.com/ModelEarth/projects.git

**Fetch the latest data from the remote repository**
	git fetch origin

**List remote branches to verify the correct branch name**
	git branch -r

**Reset your current branch to match the remote branch exactly**
	git reset --[branch name]

**Create a new Branch**
	git checkout -b my-new-feature

**Stage the changes**
	git add .

**Commit the changes**
	git commit -m "Add new feature"

**Push the new branch to the remote repository**
	git push origin my-new-feature


## Installing git while creation of docker image

To run git commands above inside our docker container,
 the following command was used:

	apt-get install -y --no-install-recommends curl jq git

**Areas of future sync issues:**
We updated our [DockerFile with this PR](https://github.com/ModelEarth/projects/pull/2/files) - Dinesh

## How we created a Docker "projects" container on GitHub

Advice on [package setup provided by ChatGPT](https://chatgpt.com/share/2200ae05-4f33-4b1c-a1f9-57be4d18257b)

This only needed to be done once by our site admin.  (You can skip this.) 

In our datascape repo, go to [Personal access tokens (classic)](https://github.com/settings/tokens)
Settings > Developer settings > Personal access tokens > Tokens (classic)

Click "Generate new token" button and in the note put 'CR_PAT for Container Registry'  
Choose write:packages (chooses read:packages) and delete:packages  
Copy and save your token. Starts with ghp_

Run the following command to log in to the GitHub Container Registry using your personal access token (starts with ghp_):

	echo your_personal_access_token | docker login ghcr.io -u your_github_username --password-stdin


### We built from modelearth/projects and pushed to datascape

We built Docker locally from modelearth/projects. Run this in your local "projects" folder
(The first command takes about 10 minutes for the build.)

	docker build -t modelearth/projects:main .
	docker tag modelearth/projects:main ghcr.io/datascape/projects:main
	echo your_personal_access_token | docker login ghcr.io -u your-username --password-stdin

	docker push ghcr.io/datascape/projects:main

The command above will create the container image if it does not exist yet.
Alternatively, you can use "latest" instead of "main" in the first two commands if you have a newer version than the main one.

The container image then appears at: [https://github.com/users/datascape/packages/container/package/projects](https://github.com/users/datascape/packages/container/package/projects)

Make the new container image under the settings icon in the lower right.

<!--
Probably won't be using this since 
(This allows us to avoid using too much storage space in modelearth account.)

	docker build -t ghcr.io/modelearth/projects:main .
	docker push ghcr.io/datascape/projects:main

Our container then appeared at: [https://github.com/datascape/projects/pkgs/container/projects](projects/pkgs/container/projects)
-->

To delete a package in GitHub, click on the "Package settings" gear icon in the lower-right.  
Scroll down to the bottom and click the Delete package button.

To delete locally, run `docker rmi ghcr.io/datascape/projects:main`
To remove all unused Docker images, containers, networks, and volumes locally: `docker system prune -a`



