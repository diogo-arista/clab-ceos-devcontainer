# TLDR 

## MacOS
Requirements:
- Git
- VS Code (With devcontainer extension installed and enabled)
- Download cEOS-lab

### Clone the repository to your local machine:
```bash
git clone https://github.com/diogo-arista/clab-ceos-devcontainer.git
cd clab-ceos-devcontainer
```
### Copy/move the cEOS-lab image to the repository directory
```bash
cp ~/Downloads/cEOSarm-lab-4.33.2-EFT3.tar .
```
### Open the repository folder in VS Code:
```bash
code .
```
### Reopen in a Container
VS Code will prompt you if you want to reopen the repository in a container.
    
# Containerlab & AVD Dev Container Environment

This repository provides a ready-to-use development environment for creating and managing network labs with [Containerlab](https://containerlab.dev/), specifically tailored for use with Arista cEOS images and potentially [Arista Validated Designs (AVD)](https://avd.arista.com/).

It leverages a pre-built Docker image (`ghcr.io/aristanetworks/aclabs/lab-base:python3.11-avd-v5.1.0-clab0.60.1-rev1.1`) which includes:

* Python 3.11
* Containerlab v0.60.1
* Arista AVD v5.1.0 (including `pyavd` and `ansible-core`)
* Common development tools
* Containerlab VS Code extension

The environment is designed to be opened using VS Code Dev Containers.

## Prerequisites

1.  **VS Code:** Install Visual Studio Code.
2.  **VS Code Remote - Containers Extension:** Install the `ms-vscode-remote.remote-containers` extension.
3.  **Container Runtime:** Install and run either:
    * Docker Desktop (Windows/macOS)
    * Podman Desktop (v5.0+ recommended, Linux/Windows/macOS)
4.  **Arista cEOS Image:** Download the appropriate cEOS lab image file (e.g., `cEOS-lab-4.xx.x.tar.xz` or `cEOSarm-lab-4.xx.x.tar.xz`) from your Arista account.
    * **IMPORTANT:** Place this file in the **root directory** of this repository *before* launching the Dev Container for the first time. You can do it later, but you will need to manually import the cEOS-lab image into docker using `docker import cEOSarm-lab-4.xx.x.tar.xz ceos:latest`.
    * **Architecture:**
        * If using **macOS with Apple Silicon (M1/M2/M3)**, download the **ARM64 (aarch64)** version (e.g., `cEOSarm-lab...`).
        * If using **Windows, Linux (x86_64), or GitHub Codespaces**, download the standard **x86_64** version (e.g., `cEOS-lab...`).

## How to Launch

1.  **Clone:** Clone this repository to your local machine.
    ```bash
    git clone <your-repository-url>
    cd clab-ceos-devcontainer
    ```
2.  **Add cEOS Image:** Copy the downloaded cEOS `.tar` or `.tar.xz` file into the root of the cloned directory.
3.  **Open in VS Code:** Open the `clab-ceos-devcontainer` folder in VS Code.
4.  **Reopen in Container:** VS Code should detect the `.devcontainer/devcontainer.json` file and prompt you to reopen in a container. Click **"Reopen in Container"**.
    * (If no prompt appears, open the Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`) and run `Remote-Containers: Reopen in Container`).
5.  **Build/Startup:** VS Code will pull the pre-built image (if not already cached) and start the container. The `post-create.sh` script will run automatically to:
    * Configure passwordless `sudo` for the `avd` user.
    * Set up an alias so typing `clab` automatically runs `sudo clab`.
    * Create a default `ansible.cfg` if one doesn't exist.
    * Import the cEOS image file from the workspace into the container's Docker environment (and remove the original file).
Note that all devcontainer creation actions may take few few minutes (depending on the network connection and host performance) to complete.

## Using the Environment

* **Terminal:** Open a terminal manually in VS Code (`Terminal > New Terminal` or `Ctrl+`` / `Cmd+``). The automatic terminal opening task has been disabled.
* **Tools:** `containerlab`, `ansible`, `ansible-playbook`, `git`, etc., are pre-installed and should be available in your PATH.
* **Running Containerlab:**
    * You can run Containerlab commands directly **without typing `sudo`**. An alias automatically runs `sudo clab` for you without needing a password.
    * Example:
        ```bash
        # Deploy a topology
        clab deploy -t your-topology-file.yaml

        # Check status
        clab inspect --all

        # Destroy the topology
        clab destroy -t your-topology-file.yaml --cleanup
        ```
* **cEOS Image:** The `post-create.sh` script imports your `.tar`/`.tar.xz` file and tags it as `ceos:latest` within the container's Docker environment.

## GitHub Codespaces

You can also run this environment in GitHub Codespaces:

1.  **Commit cEOS Image:** Add the correct **x86_64** cEOS image file to the root of the repository and commit/push it.
2.  **Create Codespace:** Go to the repository on GitHub, click `<> Code` -> `Codespaces` -> `Create codespace...`.
3.  The Codespace will build using the `devcontainer.json` and run the `post-create.sh` script, including importing the committed cEOS image.
