\# Docker-Based LaTeX CV Builder



This guide explains how to build your LaTeX CV using Docker, eliminating the need to install LaTeX locally.



\## Advantages of Docker-Based Builds



\- \*\*Consistency\*\*: Same build environment regardless of host OS

\- \*\*Zero LaTeX Installation\*\*: No need to install and maintain LaTeX locally

\- \*\*Cross-Platform\*\*: Works identically on Windows, macOS, and Linux

\- \*\*Dependency Management\*\*: All required packages are pre-installed in the container

\- \*\*Isolation\*\*: Build process doesn't affect your local system



\## Prerequisites



\- Docker and Docker Desktop installed on your system

\- For Windows, WSL 2 is required



\## Building Methods



\### Option 1: Using the Python Script (Cross-Platform)



The Python script works on all operating systems and handles error checking:



```bash

python docker\_build.py

```



\### Option 2: Using the Shell Script (Unix Systems)



For Unix-based systems (Linux, macOS), the shell script provides a native alternative:



```bash

./docker\_build.sh

```



\### Option 3: Using Docker Compose Directly



For manual control over the build process:



```bash

\# Build the Docker image

docker compose build



\# Run the container to generate the PDF

docker compose run --rm cv-builder

```



\### Option 4: Using Docker CLI Directly



If Docker Compose is unavailable:



```bash

\# Build the Docker image

docker build -t latex-cv-builder .



\# Run the container to generate the PDF

docker run --rm -v $(pwd):/latex latex-cv-builder

```



All methods will output the PDF to the current directory as `main.pdf`.



\## VSCode Integration with DevContainer



For the best development experience, you can use VSCode's DevContainer feature to edit and build your CV in a containerized environment with full LaTeX support.



\### DevContainer Setup



1\. Install the required VSCode extensions:

&#x20;  - \[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

&#x20;  - \[LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)



2\. Run our configuration script to set up the DevContainer:

&#x20;  ```bash

&#x20;  python config\_vscode\_devcontainer.py

&#x20;  ```



3\. Open the repository in VSCode



4\. Click the green button in the bottom-left corner (or press F1 and select "Dev Containers: Reopen in Container")



5\. VSCode will:

&#x20;  - Build the Docker container

&#x20;  - Connect to it

&#x20;  - Configure LaTeX Workshop inside the container



6\. Now you can:

&#x20;  - Edit LaTeX files with full syntax highlighting and autocomplete

&#x20;  - Save to automatically trigger builds

&#x20;  - View the PDF preview directly in VSCode

&#x20;  - Get real-time error feedback



\## Customization



\### Dockerfile Customization



To add additional LaTeX packages:



1\. Edit the Dockerfile:

&#x20;  ```dockerfile

&#x20;  # Add packages to this line

&#x20;  RUN tlmgr install \\

&#x20;      moderncv \\

&#x20;      ulem \\

&#x20;      xcolor \\

&#x20;      # Add your packages here

&#x20;  ```



2\. Rebuild the Docker image:

&#x20;  ```bash

&#x20;  docker compose build

&#x20;  ```





\## Troubleshooting



\### Docker Issues



\- \*\*Docker Not Running\*\*: Make sure Docker Desktop is running

&#x20; ```bash

&#x20; # Check if Docker is running

&#x20; docker info

&#x20; ```



\- \*\*Permission Issues\*\*: On Linux, you may need to add your user to the docker group

&#x20; ```bash

&#x20; sudo usermod -aG docker $USER

&#x20; # Log out and back in for changes to take effect

&#x20; ```



\- \*\*Windows-Specific\*\*: Ensure WSL 2 is properly configured for Docker Desktop



