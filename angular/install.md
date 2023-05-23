# Install the develop environment

## Install nodejs & npm

To install Node.js on Ubuntu, you can follow these steps:

1. Open a terminal window on your Ubuntu machine.

2. Update the package lists for upgrades and new package installations by running the following command:
   ```
   sudo apt update
   ```

3. Install Node.js using the package manager `apt` by running the following command:
   ```
   sudo apt install nodejs
   ```

4. To verify that Node.js was installed successfully, you can check the installed version by running the following commands:
   ```
   node --version
   npm --version
   ```

   The first command (`node --version`) will display the installed version of Node.js, while the second command (`npm --version`) will display the installed version of npm (Node Package Manager).

5. Optionally, you can install the build tools package `build-essential` to compile and install native add-ons for Node.js by running the following command:
   ```
   sudo apt install build-essential
   ```

This will install Node.js and npm on your Ubuntu machine. You can now start using Node.js for your development needs.

## Install nodejs & npm using nvm

To install Node.js on Ubuntu using NVM (Node Version Manager), you can follow these steps:

1. Open a terminal window on your Ubuntu machine.

2. Install NVM by running the following command:
   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   ```

   This command downloads the NVM installation script from the official NVM repository on GitHub and executes it.

3. Close and reopen the terminal window, or run the following command to apply the changes to your current session:
   ```
   source ~/.bashrc
   ```

4. Verify that NVM was installed successfully by running the following command:
   ```
   nvm --version
   ```

   This command should display the installed version of NVM.

5. Install the desired version of Node.js using NVM. You can install the latest LTS (Long-Term Support) version by running the following command:
   ```
   nvm install --lts
   ```

   Alternatively, you can install a specific version of Node.js by replacing `--lts` with the desired version number (e.g., `nvm install 14.17.0`).

6. Set the installed version of Node.js as the default version by running the following command:
   ```
   nvm alias default <version>
   ```

   Replace `<version>` with the installed version number (e.g., `nvm alias default 14.17.0`).

7. Verify that Node.js was installed successfully by running the following commands:
   ```
   node --version
   npm --version
   ```

   The first command (`node --version`) should display the installed version of Node.js, and the second command (`npm --version`) should display the installed version of npm (Node Package Manager).

By following these steps, you have successfully installed Node.js on Ubuntu using NVM. NVM allows you to manage multiple versions of Node.js and switch between them as needed for your development projects.

## Install Angular CLI

To install the Angular CLI (Command Line Interface) on Ubuntu Linux, you can follow these steps:

1. Open a terminal window on your Ubuntu machine.

2. Verify that Node.js and npm were installed successfully by running the following commands:
   ```
   node --version
   npm --version
   ```

   The first command (`node --version`) should display the installed version of Node.js, and the second command (`npm --version`) should display the installed version of npm.

3. Install the Angular CLI globally by running the following command:
   ```
   sudo npm install -g @angular/cli
   ```

   This command uses npm to install the Angular CLI package (`@angular/cli`) globally on your system. The `-g` flag indicates a global installation.

4. Verify that the Angular CLI was installed successfully by running the following command:
   ```
   ng version
   ```

   This command should display the installed version of the Angular CLI.

With these steps, you have successfully installed the Angular CLI on your Ubuntu Linux machine. You can now use the `ng` command to create new Angular projects, generate components, services, and more.

## Install VSC in my home 

To install Visual Studio Code (VSC) in your home directory, you can follow these steps:

1. Open a web browser and navigate to the official Visual Studio Code website at https://code.visualstudio.com/.

2. On the website, click on the "Download" button. It will detect your operating system and offer the appropriate download option.

3. Once the download is complete, open your terminal window.

4. Change to your home directory by running the following command:
   ```
   cd ~
   ```

5. Extract the downloaded package. The filename may vary based on the version you downloaded. Replace `<filename>` in the command below with the actual name of the downloaded file:
   ```
   tar -xvf <filename>.tar.gz
   ```

6. Move into the extracted directory. Replace `<directory>` with the actual name of the extracted directory:
   ```
   cd <directory>
   ```

7. Now you can run Visual Studio Code from this location by executing the `code` command. However, it's more convenient to add it to your system's PATH so that you can run it from anywhere. To do this, run the following command:
   ```
   echo 'export PATH="$HOME/<directory>/bin:$PATH"' >> ~/.bashrc
   ```

   This command appends a line to your `~/.bashrc` file, ensuring that the Visual Studio Code binary is added to your PATH when you open a new terminal window.

8. Close the terminal window and open a new one to apply the changes.

9. You can now run Visual Studio Code by simply typing `code` in the terminal.

With these steps, you have installed Visual Studio Code in your home directory. You can start using VSC for your code editing and development tasks.

## VSC plugins

Here are some popular Visual Studio Code (VSC) extensions/plugins that are commonly used for developing Angular projects. While the "best" plugins can vary based on individual preferences, the following extensions are widely recommended by the Angular community:

1. Angular Language Service:
   - Provides enhanced Angular IntelliSense, autocompletion, and type checking.
   - Enables navigation to Angular component definitions and templates.
   - Helps with error checking and code refactorings specific to Angular.

2. Angular Snippets:
   - Offers a collection of Angular code snippets for faster development.
   - Provides shortcuts for frequently used Angular components, directives, and code patterns.
   - Saves time by reducing manual typing and improving productivity.

3. Prettier:
   - Automatically formats your code to maintain consistent code style.
   - Supports various languages, including HTML, CSS, TypeScript, and more.
   - Configurable to match your preferred code formatting rules.

4. ESLint/TSLint:
   - Linter extensions that enforce code quality and style rules.
   - Provides real-time feedback on coding errors, potential bugs, and code smells.
   - Supports customization through configuration files to align with your project's coding standards.

5. GitLens:
   - Enhances the Git integration within VSC.
   - Displays line-wise Git blame annotations to show the author and commit details for each line.
   - Provides quick access to commit history, comparisons, and other Git functionalities.

6. Debugger for Chrome:
   - Enables debugging of Angular applications running in Google Chrome.
   - Allows breakpoints, stepping through code, inspecting variables, and more.
   - Essential for troubleshooting and identifying issues in your Angular application.

7. Angular Console:
   - Provides a graphical user interface for common Angular CLI commands.
   - Simplifies the process of generating components, services, modules, and other Angular artifacts.
   - Offers additional features like project scaffolding, code generation, and dependency management.

8. Angular Inline HTML and TypeScript:
   - Provides support for embedding HTML templates and TypeScript code within component files.
   - Improves code organization and ease of navigation within single-file components.
   - Enhances productivity by eliminating the need for separate template and component files.

These are just a few popular VSC extensions for Angular development. Remember to explore the Visual Studio Code marketplace to discover more extensions that suit your specific needs and preferences.

## Hello World

To create a new Angular "Hello World" project using the Angular CLI, follow these steps:

1. Open a terminal window on your Ubuntu machine.

2. Ensure that you have the Angular CLI installed by running the following command:
   ```
   ng version
   ```

   This command should display the installed version of the Angular CLI. If the command is not recognized, please refer to the previous instructions to install the Angular CLI.

3. Navigate to the directory where you want to create your Angular project. For example, to create the project in your home directory, you can run the following command:
   ```
   cd ~
   ```

4. Generate a new Angular project using the Angular CLI by running the following command:
   ```
   ng new hello-world
   ```

   This command creates a new Angular project named "hello-world" in a directory called "hello-world" in the current location. The Angular CLI will automatically set up the project structure, install dependencies, and generate initial files.

5. Once the project generation is complete, navigate into the project directory:
   ```
   cd hello-world
   ```

6. Start the development server and open the application in a web browser by running the following command:
   ```
   ng serve --open
   ```

   This command compiles the Angular application and starts a development server. The `--open` flag automatically opens the application in your default web browser.

7. In your web browser, you should now see the "Hello World" Angular application running at `http://localhost:4200`. The page will display the default Angular welcome message.

Congratulations! You have successfully created a new Angular "Hello World" project using the Angular CLI. You can now modify the project files, create components, and build your Angular application.

