# macOS Development Environment

## 1. macOS Configuration
* [Download iTerm2](https://iterm2.com/downloads.html)
* [Install Homebrew](https://brew.sh)
  ```console
  $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  $ (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/kevin/.zprofile
  $ eval "$(/opt/homebrew/bin/brew shellenv)"
  ```
* [Download Zettlr](https://www.zettlr.com/download)
  ```console
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
  ```console
  $ brew install podman-desktop
  ```
  Brew will also install the Podman Engine along with the Podman Desktop application, in case you don't have it installed yet.
* Open Podman Desktop Application within the `Applications` directory, onboarding for the following extensions:
  - Compose
  - kubectl CLI
  - Podman
* Run a very basic httpd server (named basic_httpd) that serves only its index page.
  ```console
  $ podman run --name basic_httpd -dt -p 8080:80/tcp docker.io/nginx
  ```
* Open in browser: http://localhost:8080
* Stop and remove the httpd container:
  ```console
  $ podman stop basic_httpd
  $ podman rm basic_httpd
  ```

## 4. Git
* [Download for macOS](https://git-scm.com/download/mac)
   ```console
   $ brew install git
   ```
* Check the current version of Git:
  ```console
  $ git -v
  ```
* [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
  - View all Git settings:
    ```console
    $ git config --list --show-origin
    ```
  - Setup Identity:
    ```console
    $ git config --global user.name "Your Name"
    $ git config --global user.email username@mail.com
    ```
* [Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)
  - Generating a new SSH key:
    ```console
    $ ssh-keygen -t ed25519 -C "username@mail.com"
    ```
  - When you're prompted to "Enter a file in which to save the key", you can press `Enter` to accept the default file location.
  - At the prompt, type a secure `passphrase`.
  - Start the ssh-agent and add SSH private key to the agent:
    ```console
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
    ```console
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
    ```console
    $ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
    ```
* [Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
  - Copy the SSH public key to the clipboard:
    ```console
    $ pbcopy < ~/.ssh/id_ed25519.pub
    ```
  - [GitHub > Settings > SSH and GPG keys](https://github.com/settings/keys), click `New SSH key` or `Add SSH key`.
    - Title: `Macbook Air`
    - Key type: `Authentication Key`
    - Key: `[paste your public key]`
  - Test the SSH key:
    ```console
    $ ssh -T git@github.com
    ```