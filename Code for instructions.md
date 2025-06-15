Code for instructions

frdel/agent-zero
docs/installation.md

## Windows, macOS and Linux Setup Guide

1. **Install Docker Desktop:**

- Docker Desktop provides the runtime environment for Agent Zero, ensuring consistent behavior and security across platforms
- The entire framework runs within a Docker container, providing isolation and easy deployment
- Available as a user-friendly GUI application for all major operating systems
1.1. Go to the download page of Docker Desktop [here](https://www.docker.com/products/docker-desktop/). If the link does not work, just search the web for "docker desktop download".

1.2. Download the version for your operating system. For Windows users, the Intel/AMD version is the main download button.
<img src="res/setup/image-8.png" alt="docker download" width="200"/>
<br><br>
> [!NOTE]
> **Linux Users:** You can install either Docker Desktop or docker-ce (Community Edition).
> For Docker Desktop, follow the instructions for your specific Linux distribution [here](https://docs.docker.com/desktop/install/linux-install/).
> For docker-ce, follow the instructions [here](https://docs.docker.com/engine/install/).
>
> If you're using docker-ce, you'll need to add your user to the `docker` group:
>
> ```bash
> sudo usermod -aG docker $USER
> ```
>
> Log out and back in, then run:
>
> ```bash
> docker login
> ```

1.3. Run the installer with default settings. On macOS, drag and drop the application to your Applications folder.

<img src="res/setup/image-13.png" alt="docker installed" height="100"/>
<br><br>
> [!IMPORTANT]  
> **macOS Configuration:** In Docker Desktop's preferences (Docker menu) → Settings →
> Advanced, enable "Allow the default Docker socket to be used (requires password)."
![docker socket macOS](res/setup/macsocket.png)

2. **Run Agent Zero:**

- Note: Agent Zero also offers a Hacking Edition based on Kali linux with modified prompts for cybersecurity tasks. The setup is the same as the regular version, just use the frdel/agent-zero-run:hacking image instead of frdel/agent-zero-run.

2.1. Pull the Agent Zero Docker image:

- Search for `frdel/agent-zero-run` in Docker Desktop
- Click the `Pull` button
- The image will be downloaded to your machine in a few minutes

![docker pull](res/setup/1-docker-image-search.png)

> [!TIP]
> Alternatively, run the following command in your terminal:
>
> ```bash
> docker pull frdel/agent-zero-run
> ```

2.2. Create a data directory for persistence:

- Choose or create a directory on your machine where you want to store Agent Zero's data
- This can be any location you prefer (e.g., `C:/agent-zero-data` or `/home/user/agent-zero-data`)
- This directory will contain all your Agent Zero files, like the legacy root folder structure:
  - `/memory` - Agent's memory and learned information
  - `/knowledge` - Knowledge base
  - `/instruments` - Instruments and functions
  - `/prompts` - Prompt files
  - `/work_dir` - Working directory
  - `.env` - Your API keys
  - `settings.json` - Your Agent Zero settings

> [!TIP]
> Choose a location that's easy to access and backup. All your Agent Zero data

![docker containers](res/setup/4-docker-container-started.png)

> [!TIP]
> Alternatively, run the following command in your terminal:
>
> ```bash
> docker run -p $PORT:80 -v /path/to/your/data:/a0 frdel/agent-zero-run
> ```
>
> - Replace `$PORT` with the port you want to use (e.g., `50080`)
> - Replace `/path/to/your/data` with your chosen directory path

2.4. Access the Web UI:

- The framework will take a few seconds to initialize and the Docker logs will look like the image below.
- Find the mapped port in Docker Desktop (shown as `<PORT>:80`) or click the port right under the container ID as shown in the image below

![docker logs](res/setup/5-docker-click-to-open.png)

- Open `http://localhost:<PORT>` in your browser
- The Web UI will open. Agent Zero is ready for configuration!
![docker ui](res/setup/6-docker-a0-running.png)

> [!TIP]
> You can also access the Web UI by clicking the ports right under the container ID in Docker Desktop.
> [!NOTE]
> After starting the container, you'll find all Agent Zero files in your chosen
> directory. You can access and edit these files directly on your machine, and
> the changes will be immediately reflected in the running container.

3. Configure Agent Zero

- Refer to the following sections for a full guide on how to configure Agent Zero.

## Settings Configuration

Agent Zero provides a comprehensive settings interface to customize various aspects of its functionality. Access the settings by clicking the "Settings"button with a gear icon in the sidebar.

### Agent Configuration

- **Prompts Subdirectory:** Choose the subdirectory within `/prompts` for agent behavior customization. The 'default' directory contains the standard prompts.
- **Memory Subdirectory:** Select the subdirectory for agent memory storage, allowing separation between different instances.
- **Knowledge Subdirectory:** Specify the location of custom knowledge files to enhance the agent's understanding.

![settings](res/setup/settings/1-agentConfig.png)

### Chat Model Settings

- **Provider:** Select the chat model provider (e.g., Ollama)
- **Model Name:** Choose the specific model (e.g., llama3.2)
- **Temperature:** Adjust response randomness (0 for deterministic, higher values for more creative responses)
- **Context Length:** Set the maximum token limit for context window
- **Context Window Space:** Configure how much of the context window is dedicated to chat history

![chat model settings](res/setup/settings/2-chat-model.png)

### Utility Model Configuration

- **Provider & Model:** Select a smaller, faster model for utility tasks like memory organization and summarization
- **Temperature:** Adjust the determinism of utility responses

### Embedding Model Settings

- **Provider:** Choose the embedding model provider (e.g., OpenAI)
- **Model Name:** Select the specific embedding model (e.g., text-embedding-3-small)

### Speech to Text Options

- **Model Size:** Choose the speech recognition model size
- **Language Code:** Set the primary language for voice recognition
- **Silence Settings:** Configure silence threshold, duration, and timeout parameters for voice input

### API Keys

- Configure API keys for various service providers directly within the Web UI
- Click `Save` to confirm your settings

### Authentication

- **UI Login:** Set username for web interface access
- **UI Password:** Configure password for web interface security
- **Root Password:** Manage Docker container root password for SSH access

![settings](res/setup/settings/3-auth.png)

### Development Settings

- **RFC Parameters (local instances only):** configure URLs and ports for remote function calls between instances
- **RFC Password:** Configure password for remote function calls
Learn more about Remote Function Calls and their purpose [here](#7-configure-agent-zero-rfc).

> [!IMPORTANT]
> Always keep your API keys and passwords secure.
>
# Choosing Your LLMs

The Settings page is the control center for selecting the Large Language Models (LLMs) that power Agent Zero.  You can choose different LLMs for different roles:

> If you have issues loading your settings, you can try to delete the `/tmp/settings.json` file and let Agent Zero generate a new one.
> The same goes for chats in `/tmp/chats/`, they might be incompatible with the new version

2. **Update Process (Docker Desktop)**

- Go to Docker Desktop and stop the container from the "Containers" tab
- Right-click and select "Remove" to remove the container
- Go to "Images" tab and remove the `frdel/agent-zero-run` image or click the three dots to pull the difference and update the Docker image.

![docker delete image](res/setup/docker-delete-image-1.png)

- Search and pull the new image if you chose to remove it
- Run the new container with the same volume settings as the old one

> [!IMPORTANT]
> Make sure to use the same volume mount path when running the new
> container to preserve your data. The exact path depends on where you stored
> your Agent Zero data directory (the chosen directory on your machine).

> [!TIP]
> Alternatively, run the following commands in your terminal:
>
> ```bash
> # Stop the current container
> docker stop agent-zero
>
> # Remove the container (data is safe in the folder)
> docker rm agent-zero
>
> # Remove the old image
> docker rmi frdel/agent-zero-run
>
> # Pull the latest image
> docker pull frdel/agent-zero-run
>
> # Run new container with the same volume mount
> docker run -p $PORT:80 -v /path/to/your/data:/a0 frdel/agent-zero-run
> ```

3. **Full Binaries**

frdel/agent-zero
docker/run/Dockerfile

# Use the pre-built base image for A0

# FROM agent-zero-base:local

FROM frdel/agent-zero-base:latest

# Check if the argument is provided, else throw an error

ARG BRANCH

RUN bash /ins/post_install.sh $BRANCH

# Expose ports

EXPOSE 22 80 9000-9009

RUN chmod +x /exe/initialize.sh /exe/run_A0.sh /exe/run_searxng.sh /exe/run_tunnel_api.sh

frdel/agent-zero
README.md

![Multi-agent](docs/res/physics.png)
![Multi-agent 2](docs/res/physics-2.png)
4. **Completely Customizable and Extensible**

- Almost nothing in this framework is hard-coded. Nothing is hidden. Everything can be extended or changed by the user.
- The whole behavior is defined by a system prompt in the **prompts/default/agent.system.md** file. Change this prompt and change the framework dramatically.
- The framework does not guide or limit the agent in any way. There are no hard-coded rails that agents have to follow.
- Every prompt, every small message template sent to the agent in its communication loop can be found in the **prompts/** folder and changed.
- Every default tool can be found in the **python/tools/** folder and changed or copied to create new predefined tools.

![Prompts](/docs/res/prompts.png)

## 👀 Keep in Mind

1. **Agent Zero Can Be Dangerous!**

- With proper instruction, Agent Zero is capable of many things, even potentially dangerous actions concerning your computer, data, or accounts. Always run Agent Zero in an isolated environment (like Docker) and be careful what you wish for.

2. **Agent Zero Is Prompt-based.**

- The whole framework is guided by the **prompts/** folder. Agent guidelines, tool instructions, messages, utility AI functions, it's all there.

frdel/agent-zero
requirements.txt

a2wsgi==1.10.8
ansio==0.0.1
beautifulsoup4==4.12.3
browser-use==0.1.40
docker==7.1.0
duckduckgo-search==6.1.12
faiss-cpu==1.8.0.post1
fastmcp==2.3.4
flask[async]==3.0.3
flask-basicauth==0.2.0
flaredantic==0.1.4
GitPython==3.1.43
inputimeout==1.0.4
langchain-anthropic==0.3.3
langchain-community==0.3.19
langchain-google-genai==2.0.8
langchain-groq==0.2.2
langchain-huggingface==0.1.2
langchain-mistralai==0.2.4
langchain-ollama==0.2.2
langchain-openai==0.3.1
openai-whisper==20240930
lxml_html_clean==0.3.1
markdown==3.7
mcp==1.9.0
newspaper3k==0.2.8
paramiko==3.5.0
playwright==1.52.0
pypdf==4.3.1
python-dotenv==1.1.0
pytz==2024.2
sentence-transformers==3.0.1
tiktoken==0.8.0
unstructured==0.15.13
unstructured-client==0.25.9
webcolors==24.6.0
nest-asyncio==1.6.0

## Detailed Docker Installation Instructions for Agent Zero

Here are comprehensive step-by-step instructions to install **Agent Zero** using Docker after cloning the repository to your hard drive.

---

### Prerequisites
1. **Install Docker Desktop** – Download and install Docker Desktop for your operating system from the official website.
2. **Clone the Repository** – Ensure the `agent-zero` repository is cloned to your local machine.

---

### Installation Steps

#### Step 1 – Install Docker Desktop
* Download Docker Desktop from the official Docker website and install it with the default settings.
* **macOS users**: open Docker Desktop ➔ *Settings* ➔ *Advanced* and enable **Allow the default Docker socket to be used (requires password)**.
* **Linux users**: you may install either Docker Desktop or *docker-ce* (Community Edition). If using *docker-ce*, add your user to the `docker` group:
  ```bash
  sudo usermod -aG docker $USER
  ```
  Then log out / back in and run:
  ```bash
  docker login
  ```

#### Step 2 – Pull the Agent Zero Docker Image
You have two options:

**Option A – Use the Pre-built Image (recommended)**
```bash
docker pull frdel/agent-zero-run
```

**Option B – Build from Source**
If you prefer to build the image yourself from the cloned repository, follow the Docker instructions in `docker/run/Dockerfile`.

#### Step 3 – Create a Data Directory for Persistence
Choose or create a directory where Agent Zero will store its data, e.g. `C:/agent-zero-data` or `/home/<user>/agent-zero-data`.

This directory will hold:
* `/memory` – Agent's memory and learned information
* `/knowledge` – Knowledge base
* `/instruments` – Instruments and functions
* `/prompts` – Prompt files
* `/work_dir` – Working directory
* `.env` – Your API keys
* `settings.json` – Agent Zero settings

#### Step 4 – Run the Docker Container
Run the container with port mapping and volume mounting:
```bash
docker run -p 50001:80 -v /path/to/your/data:/a0 frdel/agent-zero-run
```
Replace `/path/to/your/data` with the directory you created in Step 3. The image exposes ports **22**, **80**, and **9000-9009**; you may map additional ports as needed.

#### Step 5 – Access the Web UI
Once the container finishes initialising (a few seconds), open:
```
http://localhost:50001
```
The Agent Zero Web UI should appear.

#### Step 6 – Configure Agent Zero Settings
In the Web UI, open **Settings** and configure:

* **Agent Configuration** – choose prompt, memory, and knowledge sub-directories.
* **Model Settings** –
  * *Chat Model*: provider, model name, temperature, context length.
  * *Utility Model*: smaller, faster model for utility tasks.
  * *Embedding Model*: embedding model for retrieval.
* **API Keys** – add keys for providers (OpenAI, Anthropic, etc.).
* **Authentication** – set Web UI username/password and container root password.

#### Step 7 – Verify Installation
* Confirm Agent Zero files appear inside your data directory.
* Test the chat interface.
* Verify model responses with your chosen LLM.

---

### Alternative – Hacking Edition
For cybersecurity tasks, pull the Kali-based edition instead:
```bash
docker pull frdel/agent-zero-run:hacking
```
Run it with the same volume mapping and port settings as above.

### Updating Agent Zero
1. Stop and remove the current container (data remains in the mounted volume).
2. Remove the old image.
3. Pull the latest image.
4. Run a new container with the **same** volume mount to preserve data.

---

## Notes
* **Security** – Agent Zero can execute powerful actions. Always run it in an isolated Docker environment.
* **Customization** – All prompts and tools live in the `prompts/` and `python/tools/` directories—edit freely.
* **Persistence** – Data and settings stored in your host volume survive container updates.
* **Multi-platform** – Docker ensures consistent behaviour across Windows, macOS, and Linux.
