# nvm

To install Node.js and npm without using `sudo` by using Node Version Manager (nvm) on Linux Mint, follow these instructions:

1. Open a Terminal.

2. Install nvm:
   - Run the following command to download and install the latest version of nvm:
     ```bash
     curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
     ```

   - After the installation completes, you may need to restart the Terminal or run the command `source ~/.bashrc` to make nvm available in your current Terminal session.

3. Verify nvm installation:
   - Run the following command to check if nvm is installed correctly:
     ```bash
     command -v nvm
     ```

4. Install Node.js using nvm:
   - Run the following command to list available Node.js versions:
     ```bash
     nvm ls-remote
     ```

   - Choose the Node.js version you want to install. For example, to install Node.js version 14.x, run the following command:
     ```bash
     nvm install 14
     ```

   - nvm will download and install the selected Node.js version. This may take a few moments.

5. Verify Node.js and npm installation:
   - Run the following commands to ensure Node.js and npm are installed correctly and display their versions:
     ```bash
     node --version
     npm --version
     ```

   - The installed versions of Node.js and npm should be displayed.

6. Set the default Node.js version (optional):
   - If you want to set the installed Node.js version as the default version to be used in new Terminal sessions, run the following command:
     ```bash
     nvm alias default <node_version>
     ```

     Replace `<node_version>` with the desired Node.js version (e.g., `14`).

   - This step is optional, and you can skip it if you prefer to manually switch Node.js versions using nvm.

Now you have installed Node.js and npm without using `sudo` by utilizing nvm on your Linux Mint computer. You can easily switch between different Node.js versions and manage your Node.js environment using nvm.
