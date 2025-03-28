# macOS Development Environment <!-- omit from toc -->

## Table of Contents <!-- omit in toc -->
- [1. macOS Configuration](#1-macos-configuration)
- [2. Visual Studio Code](#2-visual-studio-code)
- [3. Podman Desktop](#3-podman-desktop)
- [4. Git](#4-git)
- [5. Python Development Tools](#5-python-development-tools)
  - [5.1. Install uv](#51-install-uv)
  - [5.2. Install Python](#52-install-python)
  - [5.3. Run JupyterLab](#53-run-jupyterlab)
- [6. Java Development Tools](#6-java-development-tools)
  - [6.1. Install SDKMAN!](#61-install-sdkman)
  - [6.2. Install Java Development Kit (OpenJDK - Eclipse Temurin)](#62-install-java-development-kit-openjdk---eclipse-temurin)
  - [6.3. Install Apache Maven](#63-install-apache-maven)
- [7. React Development Tools](#7-react-development-tools)
  - [7.1. Install Node.js](#71-install-nodejs)

## 1. macOS Configuration
* [iTerm2](https://iterm2.com/downloads.html)
* [Homebrew](https://brew.sh)
  - Install Homebrew
    ```sh
    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
  - Run these commands in your terminal to add Homebrew to your PATH:
    ```sh
    $ echo >> ~/.zprofile
    $ echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
    $ eval "$(/opt/homebrew/bin/brew shellenv)"
    ```
  - Run the following command to confirm your installation
    ```sh
    $ brew help
    ```

## 2. Visual Studio Code
* [Download VS Code](https://code.visualstudio.com/)
* Insall VS Code Extenstions:
  - [Python](vscode:extension/ms-python.python)
  - [Extension Pack for Java](vscode:extension/vscjava.vscode-java-pack)
  - [Spring Boot Extension Pack](vscode:extension/vmware.vscode-boot-dev-pack)
  - [Prettier - Code formatter](vscode:extension/esbenp.prettier-vscode)
  - [Rainbow CSV](vscode:extension/mechatroner.rainbow-csv)
  - [GitLens — Git supercharged](vscode:extension/eamodio.gitlens)
  - [Markdown All in One](vscode:extension/yzhang.markdown-all-in-one)
  - [Polacode](vscode:extension/pnp.polacode)

## 3. Podman Desktop
* [Podman Desktop for macOS](https://podman-desktop.io/downloads/macos)
  ```sh
  $ brew install podman-desktop
  ```
  Brew will also install the Podman Engine along with the Podman Desktop application, in case you don't have it installed yet.
* Open Podman Desktop Application within the `Applications` directory, onboarding for the following extensions:
  - Podman
  - kubectl CLI
  - Compose
* Run a very basic httpd server (named basic_httpd) that serves only its index page.
  ```sh
  $ podman run --name basic_httpd -dt -p 8080:80/tcp docker.io/nginx
  ```
* Open in browser: http://localhost:8080
* Stop and remove the httpd container:
  ```sh
  $ podman stop basic_httpd
  $ podman rm basic_httpd
  ```

## 4. Git
* [Download for macOS](https://git-scm.com/download/mac)
   ```sh
  $ brew install git
   ```
* Check the current version of Git:
  ```sh
  $ git -v
  ```
* [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
  - View all Git settings:
    ```sh
    $ git config --list --show-origin
    ```
  - Setup Identity:
    ```sh
    $ git config --global user.name "Your Name"
    $ git config --global user.email username@mail.com
    ```
* [Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)
  - Generating a new SSH key:
    ```sh
    $ ssh-keygen -t ed25519 -C "username@mail.com"
    ```
  - When you're prompted to "Enter a file in which to save the key", you can press `Enter` to accept the default file location.
  - At the prompt, type a secure `passphrase`.
  - Start the ssh-agent and add SSH private key to the agent:
    ```sh
    $ eval "$(ssh-agent -s)"
    $ ssh-add ~/.ssh/id_ed25519
    ```
* [Adding SSH key to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)
  - First, check to see if your `~/.ssh/config` file exists in the default location.
    ```console
    $ open ~/.ssh/config
    The file /Users/kevin/.ssh/config does not exist.
    ```
  - If the file doesn't exist, create the file.
    ```sh
    $ vi ~/.ssh/config
    ```
  - Modify the file to contain the following lines.
    ```text
    Host github.com
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
    ```
  - Add your SSH private key to the ssh-agent and store your passphrase in the keychain.
    ```sh
    $ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
    ```
* [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
  - Copy the SSH public key to the clipboard:
    ```sh
    $ pbcopy < ~/.ssh/id_ed25519.pub
    ```
  - [GitHub > Settings > SSH and GPG keys](https://github.com/settings/keys), click `New SSH key` or `Add SSH key`.
    - Title: `Mac Studio`
    - Key type: `Authentication Key`
    - Key: `[paste your public key]`
  - Test the SSH key:
    ```sh
    $ ssh -T git@github.com
    ```

## 5. Python Development Tools
### 5.1. Install uv
* [Install uv with standalone installers](https://docs.astral.sh/uv/#installation)
  ```sh
  $ curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
* Restart your shell to check the current version of uv:
  ```sh
  $ uv -V
  ```
* [Upgrading uv](https://docs.astral.sh/uv/getting-started/installation/#upgrading-uv)
  ```sh
  $ uv self update
  ```
* [Shell autocompletion](https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion)
  ```sh
  $ echo $SHELL
  $ echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
  $ echo 'eval "$(uvx --generate-shell-completion zsh)"' >> ~/.zshrc
  ```
* Restart shell for the changes to take effect:
  ```sh
  $ exec $SHELL
  ```
* If the message `command not found: compdef` appear, add the following to the beginning of the ~/.zshrc file and restart the shell again:
  ```sh
  autoload -Uz compinit
  compinit
  ```

### 5.2. Install Python
* [Install the latest Python version](https://docs.astral.sh/uv/guides/install-python/#getting-started)
  ```sh
  $ uv python install
  ```
* [Viewing Python installations](https://docs.astral.sh/uv/guides/install-python/#viewing-python-installations)
  ```sh
  $ uv python list
  ```

### 5.3. Run JupyterLab
* [Run JupyterLab as a standalone tool](https://docs.astral.sh/uv/guides/integration/jupyter/#using-jupyter-as-a-standalone-tool)
  ```sh
  $ uvx jupyter lab
  ```


## 6. Java Development Tools
### 6.1. Install SDKMAN!
* [Installation](https://sdkman.io/install)
  ```sh
  $ curl -s "https://get.sdkman.io" | bash
  $ source "$HOME/.sdkman/bin/sdkman-init.sh"
  $ sdk version
  ```

### 6.2. Install Java Development Kit (OpenJDK - Eclipse Temurin)
* List the available version of Java:
  ```sh
  $ sdk list java
  ```
* [Install Eclipse Temurin™ 21 - LTS](https://sdkman.io/jdks#tem)
  ```sh
  $ sdk install java
  ```
* Check the current version of Java
  ```
  $ sdk current
  $ java -version
  ```
* Switch to Java version in this shell.
  ```sh
  $ sdk use java 21.0.6-tem
  ```
* Configure default Java version for all shells.
  ```sh
  $ sdk default java 21.0.6-tem
  ```
* Locate where JDK is installed:
  ```sh
  $ readlink -f $(which java)
  ```

### 6.3. Install Apache Maven
* Install Apache Maven:
  ```sh
  $ sdk install maven
  ```
* Check the current version of Maven:
  ```sh
  $ sdk current
  $ mvn -v
  ```

## 7. React Development Tools
### 7.1. Install Node.js
* [Download Node.js](https://nodejs.org/en/download)
  ```sh
  # Download and install nvm:
  $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash

  # in lieu of restarting the shell
  $ \. "$HOME/.nvm/nvm.sh"

  # Download and install Node.js:
  $ nvm install --lts

  # Verify the Node.js version:
  $ node -v
  $ nvm current

  # Download and install Yarn:
  $ corepack enable yarn

  # Verify Yarn version:
  $ yarn -v
  ```
