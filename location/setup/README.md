[Our OpenWebUI Location Projects](../)
### Open WebUI Install
<!--Pinecone -->

If you already have an "open-webui" Docker container, your OpenWebUI server may already be running at [localhost:3000](http://localhost:3000) (since Docker restarts it when you start your computer).  You can now [train with a web page](train).


You can use the <a href="https://docs.openwebui.com">Quick Start with Docker (recommended)</a> to install locally. This will automatically include Ollama in your Docker install.
<br>


# Open WebUI Install for Local Dev

### To edit and build our "projects" fork locally

You can try use our [Docker Setup for Editing (Experimental)](docker) in which we use Docker with a fork of OpenWebUI and extract files to edit in a Webroot. It currently only extracts the backend files for editing.

TO DO: How would we change the config setting to avoid installing Ollama locally?

<div style="border:1px solid #ccc; padding:20px 20px 25px 30px; border-radius:20px;" >

<b>Issue: Storage exceeded when installing Ollama locally</b><br><br>

I had Ollama working fine from the QuickStart above, but since our goal is to build locally, I tried installing Ollama to use with the local build process described below.<br><br>

My install of Ollama locally did not complete due to a storage limitation. I increased the allowed storage, but then my machine was overwhelmed.<br><br>

So now one goal is to install Ollama externally, using the "Different Server" command here: <a href="https://docs.openwebui.com">docs.openwebui.com</a>

</div><br>


## Steps for Local Build

You won't need these time-intensive install steps if you instead run a [local Docker instance](https://docs.openwebui.com/).  

These steps are for programmers who will be editing and building locally.  

Clone with a command or use GitHub Desktop to pull to your computer from [our projects repo](https://github.com/modelearth/projects/).

	git clone https://github.com/modelearth/projects.git
	cd projects/

Here are the commands from [Open WebUI Getting Started](https://docs.openwebui.com/getting-started/) for building locally.  

As of July 2024 python 3.12 has issues with OpenWebUI, so use python 3.11.  
Check [your local python version and install nvm](../../../io/coders/python/) to host multiple version of python.

This is optional. We haven't confirmed the install works when running this first.

	python3.11 -m venv env
	source env/bin/activate

<!--
	# Copying required .env file
	cp -RPp .env.example .env
-->

Check that you have npm installed. If not, [install node project manager](../../../io/coders/python/)

	npm -v

View a list of your conda environments.
If none are found, [download from Anaconda.com](https://www.anaconda.com/download)

	conda env list  

The follow two options may take about 2 hour to run.

We have a [start.sh script](https://github.com/ModelEarth/projects/blob/main/location/setup/script/start.sh) which will run the comands below for you. Run this in the root of the "projects" folder. It invokes python3.11.
	
	bash location/setup/script/start.sh

Or, alternatively, run the following. (These are contained in the command above.)

	# Building Frontend Using Node
	npm i
	npm run build


	# Serving Frontend with the Backend
	cd ./backend
	pip install -r requirements.txt -U

	bash start.sh

Once either of the above is complete, the site is visible at:

[http://localhost:8080](http://localhost:8080)


A RAM error quickly shut down the local site: [1 leaked semaphore](https://github.com/lllyasviel/Fooocus/discussions/2690)  
The CPU was not running when this occurred.

<!--
The following restarted the frontend at [localhost:5173](http://localhost:5173/)
After a couple minutes you'll see "Open WebUI Backend Required"

	npm run dev
-->

Build to apply changes from "src".

	npm run build

Don't use `npm run dev` it only hosts the frontend.

<!--
Running the pre-existing bash start.sh results in:

Loading WEBUI_SECRET_KEY from file, not provided as an environment variable.
Loading WEBUI_SECRET_KEY from .webui_secret_key
start.sh: line 23: ${USE_OLLAMA_DOCKER,,}: bad substitution
start.sh: line 25: ${USE_CUDA_DOCKER,,}: bad substitution
start.sh: line 52: exec: uvicorn: not found

Is there a fast way to reopen the conda instance?
-->

**Restart server**

Only takes a couple minutes this time, same command as above.  
Choose the existing conda environment by saying "no" when asked to reinstall.

	bash location/setup/script/start.sh


## Edit in our Open WebUI "projects/location" folder

The "projects/location" folder is where we'll edit enhancements to the "src" folder.  
We'll merge our enhancments into "src-merged" so we don't have sync issues with open-webui.

We also edit index.html and active.md in our "projects" fork root since we added those files.

<span style="color:red; display:none">
We've temporarily deactivated the following while we move it to another repo. It seems that the large size of the Docker container may have filled our storage space, preventing other pages in the model.earth repos from being deployed. Old pages were stuck in the cache.
</span>

You can either fork and clone our [projects repo](https://github.com/ModelEarth/projects)  
Or you can use our [datascape Docker container image](https://github.com/users/datascape/packages/container/package/projects), which is created from the same modelearth/projects repo.

## Other attempt for local build


### You may need to have Ollama installed

This may exceed your capacity or CPU if you don't have a fast graphics card.

<!--You might need to increase your storage allocation in Docker-->

If Ollama is installed, you should see it here:
[http://localhost:11434](http://localhost:11434)

<!-- Also saw this in Settings > Conections:  http://ollama:11434 -->

**If you don't have Ollama installed yet**
Start with the [Open WebUI Documentation](https://docs.openwebui.com/)

<!--(If you already have an "open-webui" container in Docker, delete or rename it.)-->

<!--
You can run the following in your local projects folder.  

	docker compose up -d --build
-->

<!-- If you already have Ollama running in Docker,
	the above command my exceed the avalable allocated memory. 

Tried again after changind in Docker > Settings > Resources > Advanced
CPU was already at 16
Increased memory limit from 8GB to 24GB
Increase Swap from 1GB to 3GB
-->

Otherwise, here are the steps [if you already have Ollama](https://docs.openwebui.com/)
Or if you're retaining an [existing open-webui container](https://docs.openwebui.com/getting-started/)
<!--
, and using GPU Support, then run:

	docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
-->

## Install with Pip and start your server (Beta)

This pip install did not work on Loren's Mac.
No error, but there's no backend running.

Takes about 10 minutes. (Docker is a faster option if you are not coding.)

	python3 -m venv env
	source env/bin/activate
	pip install open-webui
	open-webui serve

## Run the Build

Takes another 10 minutes.

	npm run build

Neither of the following works after the above.

Open your local build at [localhost:3000](http://localhost:3000)
View at [localhost:8080]( http://localhost:8080/)

Config changes are needed to install the backend without Ollama.
