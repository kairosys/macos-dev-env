# macOS Development Environment

## 1. macOS Configuration
* [Download iTerm2](https://iterm2.com/downloads.html)
* [Install Homebrew](https://brew.sh)
  ```sh
  $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  $ (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/kevin/.zprofile
  $ eval "$(/opt/homebrew/bin/brew shellenv)"
  ```
* [Download Zettlr](https://www.zettlr.com/download)
  ```sh
  $ brew install --cask Zettlr
  ```

## 2. Visual Studio Code
* [Download VS Code](https://code.visualstudio.com/)
* Insall VS Code Extenstions:
  - [Extension Pack for Java](vscode:extension/vscjava.vscode-java-pack)
  - [Spring Boot Extension Pack](vscode:extension/vmware.vscode-boot-dev-pack)
  - [Python](vscode:extension/ms-python.python)
  - [ES7+ React/Redux/React-Native snippets](vscode:extension/dsznajder.es7-react-js-snippets)
  - [Prettier - Code formatter](vscode:extension/esbenp.prettier-vscode)
  - [GitLens — Git supercharged](vscode:extension/eamodio.gitlens)

## 3. Podman Desktop
* [Podman Desktop for macOS](https://podman-desktop.io/downloads/macos)
  ```sh
  $ brew install podman-desktop
  ```
  Brew will also install the Podman Engine along with the Podman Desktop application, in case you don't have it installed yet.
* Open Podman Desktop Application within the `Applications` directory, onboarding for the following extensions:
  - Compose
  - kubectl CLI
  - Podman
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
    - Title: `Macbook Air`
    - Key type: `Authentication Key`
    - Key: `[paste your public key]`
  - Test the SSH key:
    ```sh
    $ ssh -T git@github.com
    ```

## 5. Java Development Tools
### 5.1. Install SDKMAN!
* [Installation](https://sdkman.io/install)
  ```sh
  $ curl -s "https://get.sdkman.io" | bash
  $ source "$HOME/.sdkman/bin/sdkman-init.sh"
  $ sdk version
  ```

### 5.2. Install Java Development Kit (OpenJDK - Eclipse Temurin)
* List the available version of Java:
  ```sh
  $ sdk list java
  ```
* [Install Eclipse Temurin™ 21 - LTS](https://sdkman.io/jdks#tem)
  ```sh
  $ sdk install java 21.0.3-tem
  ```
* Check the current version of Java
  ```
  $ sdk current
  $ java -version
  ```
* Switch to Java version in this shell.
  ```sh
  $ sdk use java 21.0.3-tem
  ```
* Configure default Java version for all shells.
  ```sh
  $ sdk default java 21.0.3-tem
  ```
* Locate where JDK is installed:
  ```sh
  $ readlink -f $(which java)
  ```

### 5.3. Install Apache Maven
* Install Apache Maven:
  ```sh
  $ sdk install maven
  ```
* Apply the setting to the bash terminal:
  ```sh
  $ source ~/.bashrc
  ```
* Check the current version of Maven:
  ```sh
  $ sdk current
  $ mvn -v
  ```

## 6. React Development Tools
### 6.1. Install Node Version Manager
* [Download Node.js](https://nodejs.org/en/download/package-manager)
* Install Node Version Manager (NVM):
  ```sh
  $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  ```
* Activate NVM:
  ```sh
  $ source ~/.zshrc
  ```
* Check the current version of NVM:
  ```sh
  $ nvm -v
  ```

### 6.2. Install Node.js
* Install the latest LTS version of Node:
  ```sh
  $ nvm install --lts
  ```
* Make the default LTS version as NVM:
  ```sh
  $ nvm alias default "lts/*"
  ```
* Check the installed version:
  ```sh
  $ node -v
  ```

## 7. Python Development Tools
### 7.1. Install Pyenv
* [Installing with Homebrew](https://github.com/pyenv/pyenv?tab=readme-ov-file#homebrew-in-macos)
  ```sh
  $ brew update
  $ brew install pyenv
  ```
* [Set up shell environment for Pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#set-up-your-shell-environment-for-pyenv)
  ```sh
  $ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
  $ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
  $ echo 'eval "$(pyenv init -)"' >> ~/.zshrc
  ```
* Restart shell for the `PATH` changes to take effect:
  ```sh
  $ exec "$SHELL"
  ```
* Check the current version of Pyenv:
  ```sh
  $ pyenv --version
  ```

### 7.2. Install Python
* [Install the build-time dependencies](https://typeon.medium.com/python-lzma-extension-was-not-compiled-mac-195df65f2aac)
  ```sh
  $ brew install xz
  ```
* List the available version of Python:
  ```sh
  $ pyenv install --list | grep '3.12'
  ```
* [Install Python 3](https://www.python.org/)
  ```sh
  $ pyenv install 3.12.4
  ```
* Set global Python version:
  ```sh
  $ pyenv global 3.12.4
  ```
* Check the current version of Python
  ```sh
  $ pyenv versions
  $ python -V
  ```

### 7.3. Install JupyterLab
* [Installing Jupyter](https://jupyter.org/install)
  ```sh
  $ pip install jupyterlab
  ```
* Once installed, launch JupyterLab with:
  ```sh
  $ jupyter lab
  ```
