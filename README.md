# Arcium-Testnet
# Step 1: Download and run rustup-init script
echo "Downloading rustup-init script and starting installation..."
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Step 2: After running the script, it will prompt you.
# By default, press ENTER to proceed with the default installation (recommended).

# Wait for the script to finish...
# When done, either restart your shell or source your profile:
echo "Configuring environment variables..."

# Step 3: Add Cargo's bin directory to your PATH for the current session
export PATH="$HOME/.cargo/bin:$PATH"

# Optionally, add this to your shell startup file for persistence
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc  # or ~/.zshrc if using zsh

# Step 4: Source the shell configuration to apply changes now
source ~/.bashrc  # or source ~/.zshrc

# Step 5: Verify the installation
echo "Verifying installation..."
rustc --version
cargo --version
rustup --version

echo "Rust installation completed successfully!"
Full WSL2 Script for Solana Development Setup
Save this to a file like solana_setup.sh and run it using bash solana_setup.sh, or paste it directly into your terminal.

bash
Copy
Edit
#!/bin/bash

set -e  # Exit on any error

echo "==> Installing Rust..."
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
. "$HOME/.cargo/env"

echo "==> Verifying Rust installation..."
rustc --version

echo "==> Installing Solana CLI..."
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"

# Add Solana CLI to PATH (for Bash or Zsh)
if [[ "$SHELL" == *"bash"* ]]; then
    echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
    source ~/.bashrc
elif [[ "$SHELL" == *"zsh"* ]]; then
    echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.zshrc
    source ~/.zshrc
fi

echo "==> Verifying Solana CLI installation..."
solana --version

echo "==> Installing Node.js (using NodeSource)..."
curl -fsSL https://deb.nodesource.com/setup_23.x | sudo -E bash -
sudo apt-get install -y nodejs

echo "==> Verifying Node.js installation..."
node -v

echo "==> Installing Yarn..."
npm install --global yarn

echo "==> Verifying Yarn installation..."
yarn -v

echo "==> Installing Anchor Version Manager (AVM)..."
cargo install --git https://github.com/coral-xyz/anchor avm --force

echo "==> Verifying AVM installation..."
avm --version

echo "==> Installing latest Anchor CLI using AVM..."
avm install latest
avm use latest

echo "==> Setup complete! Installed versions:"
echo "- Rust: $(rustc --version)"
echo "- Solana CLI: $(solana --version)"
echo "- Anchor CLI: $(anchor --version)"
echo "- Node.js: $(node -v)"
echo "- Yarn: $(yarn -v)"
⚠️ Requirements
WSL2 with Ubuntu (or other Linux distro)

Internet connection

Run this with a user that has sudo privileges

Install Yarn
Installation
Install Corepack, an intermediary tool that will let you configure your package manager version on a per-project basis:
npm install -g corepack

Then initialize a new project:
yarn init -2
Updating Yarn
Any time you'll want to update Yarn to the latest version, just run:

yarn set version stable
yarn install
Yarn will then configure your project to use the most recent stable binary.

tip
Yarn also frequently ships Release Candidate builds. Use yarn set version canary should you need a feature not released on the stable channel yet. Those builds are very stable, the only difference with the regular channel being a more staggered migration between major as we implement new breaking changes.

Installing the latest build fresh from master
You may want to test a version of Yarn so recent it hasn't been released in a Release Candidate yet, or even not merged. The following command will clone, build, and install Yarn in your project, straight from our repository:

yarn set version from sources
It accepts a --branch flag which you can use to test specific PRs:

yarn set version from sources --branch 1211

Install Anchor
Quick Installation
On Mac and Linux, run this single command to install all dependencies.

Terminal

curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
Windows Users: You must first install WSL (see Install Dependencies). Then run the command above in the Ubuntu (Linux) terminal.

After installation, you should see output similar to the following:


Installed Versions:
Rust: rustc 1.85.0 (4d91de4e4 2025-02-17)
Solana CLI: solana-cli 2.1.15 (src:53545685; feat:3271415109, client:Agave)
Anchor CLI: anchor-cli 0.31.0
Node.js: v23.9.0
Yarn: 1.22.1
Installation complete. Please restart your terminal to apply all changes.
If the quick installation command above doesn't work, please refer to the Install Dependencies section below for instructions to install each dependency individually.

If the quick install command runs successfully, skip to the Solana CLI Basics and Anchor CLI Basics sections below.

Install Dependencies
The instructions below will guide you through installing each dependency individually.

Windows users must first install WSL (Windows subsystem for Linux) and then install the dependencies specified in the Linux section below.
Linux users should first install the dependencies specified in the Linux section below.
Mac users should start with the Rust installation instructions below.
Windows Subsystem for Linux (WSL)
Linux
Install Rust
Solana programs are written in the Rust programming language.

The recommended installation method for Rust is rustup.

Run the following command to install Rust:

Terminal

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
You should see the following message after the installation completes:

Successful Rust Install Message
Run the following command to reload your PATH environment variable to include Cargo's bin directory:

Terminal

. "$HOME/.cargo/env"
To verify that the installation was successful, check the Rust version:

Terminal

rustc --version
You should see output similar to the following:


rustc 1.84.1 (e71f9a9a9 2025-01-27)
Install the Solana CLI
The Solana CLI provides all the tools required to build and deploy Solana programs.

Install the Solana CLI tool suite using the official install command:

Terminal

sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
You can replace stable with the release tag matching the software version of your desired release (i.e. v2.0.3), or use one of the three symbolic channel names: stable, beta, or edge.

If it is your first time installing the Solana CLI, you may see the following message prompting you to add a PATH environment variable:


Close and reopen your terminal to apply the PATH changes or run the following in your existing shell:
export PATH="/Users/test/.local/share/solana/install/active_release/bin:$PATH"
Linux
Mac
If you are using a Linux or WSL terminal, you can add the PATH environment variable to your shell configuration file by running the command logged from the installation or by restarting your terminal.

Terminal

export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
To verify that the installation was successful, check the Solana CLI version:

Terminal

solana --version
You should see output similar to the following:


solana-cli 2.0.26 (src:3dccb3e7; feat:607245837, client:Agave)
You can view all available versions on the Agave Github repo.

Agave is the validator client from Anza, formerly known as Solana Labs validator client.

To later update the Solana CLI to the latest version, you can use the following command:

Terminal

agave-install update
Install Anchor CLI
Anchor is a framework for developing Solana programs. The Anchor framework leverages Rust macros to simplify the process of writing Solana programs.

There are two ways to install the Anchor CLI and tooling:

Anchor Version Manager (AVM) - Recommended installation method
Without AVM - Install directly from GitHub
AVM
Without AVM
The Anchor version manager (AVM) allows you to install and manage different Anchor versions on your system and easily update Anchor versions in the future.

Install AVM with the following command:

Terminal

cargo install --git https://github.com/coral-xyz/anchor avm --force
Check that AVM was installed successfully:

Terminal

avm --version
Install the latest version of Anchor CLI using AVM:

Terminal

avm install latest
avm use latest
Alternatively, you can install a specific version of Anchor CLI by specifying the version number:

Terminal

avm install 0.31.0
avm use 0.31.0
Don't forget to run the avm use command to declare which Anchor CLI version should be used on your system.

If you installed the latest version, run avm use latest.
If you installed the version 0.31.0, run avm use 0.31.0.
To verify that the installation was successful, check the Anchor CLI version:

Terminal

anchor --version
You should see output similar to the following:


anchor-cli 0.31.0
When installing the Anchor CLI on Linux or WSL, you may encounter this error:


error: could not exec the linker cc = note: Permission denied (os error 13)
If you see this error message, follow these steps:

Install the dependencies listed in the Linux section at the top of this page.
Retry installing the Anchor CLI.
Node.js and Yarn
Node.js and Yarn are required to run the default Anchor project test file (TypeScript) created with the anchor init command. (Rust test template is also available using anchor init --test-template rust)

Node Installation
Yarn Installation
When running anchor build, if you encounter the following errors:

error: not a directory
lock file version 4 requires `-Znext-lockfile-bump
After applying the solution above, attempt to run anchor build again.

When running anchor test after creating a new Anchor project on Linux or WSL, you may encounter the following errors if Node.js or Yarn are not installed:


Permission denied (os error 13)

No such file or directory (os error 2)
Solana CLI Basics
This section will walk through some common Solana CLI commands to get you started.

Solana Config
To see your current config:


solana config get
You should see output similar to the following:


Config File: /Users/test/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com
WebSocket URL: wss://api.mainnet-beta.solana.com/ (computed)
Keypair Path: /Users/test/.config/solana/id.json
Commitment: confirmed
The RPC URL and Websocket URL specific the Solana cluster the CLI will make requests to. By default this will be mainnet-beta.

You can update the Solana CLI cluster using the following commands:


solana config set --url mainnet-beta
solana config set --url devnet
solana config set --url localhost
solana config set --url testnet
You can also use the following short options:


solana config set -um    # For mainnet-beta
solana config set -ud    # For devnet
solana config set -ul    # For localhost
solana config set -ut    # For testnet
The Keypair Path specifies the location of the default wallet used by the Solana CLI (to pay transaction fees and deploy programs). The default path is ~/.config/solana/id.json. The next step walks through how to generate a keypair at the default location.

Create Wallet
To interact with the Solana network using the Solana CLI, you need a Solana wallet funded with SOL.

To generate a keypair at the default Keypair Path, run the following command:


solana-keygen new
You should see output similar to the following:


Generating a new keypair
For added security, enter a BIP39 passphrase
NOTE! This passphrase improves security of the recovery seed phrae NOT the
keypair file itself, which is stored as insecure plain text
BIP39 Passphrase (empty for none):
Wrote new keypair to /Users/test/.config/solana/id.json
===========================================================================
pubkey: 8dBTPrjnkXyuQK3KDt9wrZBfizEZijmmUQXVHpFbVwGT
===========================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
cream bleak tortoise ocean nasty game gift forget fancy salon mimic amazing
===========================================================================
If you already have a file system wallet saved at the default location, this command will NOT override it unless you explicitly force override using the --force flag.

Once a keypair is generated, you can get the address (public key) of the keypair with the following command:


solana address
Airdrop SOL
Once you've set up your local wallet, request an airdrop of SOL to fund your wallet. You need SOL to pay for transaction fees and to deploy programs.

Set your cluster to the devnet:


solana config set -ud
Then request an airdrop of devnet SOL:


solana airdrop 2
To check your wallet's SOL balance, run the following command:


solana balance
The solana airdrop command is currently limited to 5 SOL per request on devnet. Errors are likely due to rate limits.

Alternatively, you can get devnet SOL using the Solana Web Faucet.

Run Local Validator
The Solana CLI comes with the test validator built-in. Running a local validator will allow you to deploy and test your programs locally.

In a separate terminal, run the following command to start a local validator:


solana-test-validator
Make sure to update the Solana CLI config to localhost before commands.


solana config set -ul
Anchor CLI Basics
This section will walk through some common Anchor CLI commands to get you started. For more information on the Anchor CLI, see the Anchor documentation.

Initialize Project
To create a new Anchor project, run the following command:

Terminal

anchor init <project-name>
For example, to create a project called my-project, run:

Terminal

anchor init my-project
This command creates a new directory with the project name and initializes a new Anchor project with a basic Rust program and TypeScript test template.

Navigate to the project directory:

Terminal

cd <project-name>
See the Anchor project's file structure.

Build Program
To build your project, run the following command:

Terminal

anchor build
The compiled program can be found in the /target/deploy directory.

Deploy Program
To deploy your project, run the following command:

Terminal

anchor deploy
This command will deploy your program to the cluster specified in the Anchor.toml file.

Test Program
To test your project, run the following command:

Terminal

anchor test
This command builds, deploys, and runs the tests for your project.

When using localnet as the cluster in Anchor.toml, Anchor automatically starts a local validator, deploys your program, runs tests, and then stops the validator.

Shell Completions
Shell completions can be generated for bash, fish and zsh.

Bash
Terminal

mkdir -p $HOME/.local/share/bash-completion/completions
anchor completions bash > $HOME/.local/share/bash-completion/completions/anchor
avm completions bash > $HOME/.local/share/bash-completion/completions/avm
exec bash
Fish
Terminal

mkdir -p $HOME/.config/fish/completions
anchor completions fish > $HOME/.config/fish/completions/anchor.fish
avm completions fish > $HOME/.config/fish/completions/avm.fish
source $HOME/.config/fish/config.fish
Zsh
First ensure the following is in your .zshrc file. If using oh-my-zsh this step can be skipped.

Terminal

autoload -U compinit
compinit -i
Next run:

Terminal

anchor completions zsh | sudo tee /usr/local/share/zsh/site-functions/_anchor
avm completions zsh | sudo tee /usr/local/share/zsh/site-functions/_avm
exec zsh

Docker & Docker Compose
Go here to install Docker and here to install Docker Compose. To test correct installation, make sure docker compose (and not docker-compose) is installed by running docker compose --version.

Installing using the Arcium version manager (arcup)
arcup is a tool for managing versioning of the arcium tooling (including the cli and MPC node). More info on it can be found here. It will require the same dependencies as building from source.

Install arcup. We currently support 4 pre-built targets, listed below. We do not support Windows at the moment.

aarch64_linux

x86_64_linux

aarch64_macos

x86_64_macos

You can install it by replacing <YOUR_TARGET> with the target you want to install, and running the following command:

Copy
TARGET=<YOUR_TARGET> && curl "https://bin.arcium.com/download/arcup_${TARGET}_0.1.46" -o ~/.cargo/bin/arcup && chmod +x ~/.cargo/bin/arcup
Install the latest version of the CLI using arcup:

Copy
arcup install
Verify the installation.

Copy
arcium --version
Issues
Installation might fail due to a variety of reasons. This section contains a list of the most common issues and their solutions, taken from anchor's installation guide.

Missing dependencies
On Linux systems you may need to install additional dependencies. E.g. on Ubuntu:

Copy
sudo apt-get update && sudo apt-get upgrade && sudo apt-get install -y pkg-config build-essential libudev-dev libssl-dev
Incorrect $PATH
Rust binaries, including arcup and arcium, are installed to the ~/.cargo/bin directory. Since this directory is required to be in the PATH environment variable, Rust installation tries to set it up automatically, but it might fail to do so in some platforms.

To verify that the PATH environment variable was set up correctly, run:

Copy
which arcium
the output should look like (with your username):

Copy
/home/user/.cargo/bin/arcium
