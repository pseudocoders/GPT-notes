# GIT basics

## Installation

To download Git and set it up on a Linux Ubuntu machine, you can follow these steps:

Step 1: Open a terminal:
   - Press `Ctrl + Alt + T` to open a terminal window or search for "Terminal" in the applications menu.

Step 2: Update package lists (optional):
   - It's a good practice to update your package lists before installing new software. Run the following command in the terminal:
     ```
     sudo apt update
     ```

Step 3: Install Git:
   - Run the following command in the terminal to install Git:
     ```
     sudo apt install git
     ```

Step 4: Verify the installation:
   - Once the installation is complete, you can verify that Git is installed correctly by checking its version. Run the following command:
     ```
     git --version
     ```
   - You should see the Git version number displayed in the terminal, indicating that Git is successfully installed.

Step 5: Configure Git:
   - Before you start using Git, it's a good idea to configure your name and email address. This information is used to identify the author of commits. Run the following commands, replacing the placeholders with your own name and email address:
     ```
     git config --global user.name "Your Name"
     git config --global user.email "your.email@example.com"
     ```

Step 6: Additional Git configurations (optional):
   - There are various additional configurations you can set up, such as default text editor, line ending preferences, color output, etc. These configurations are optional but can enhance your Git experience. You can explore the Git documentation or other resources to learn more about these options.

That's it! You have successfully downloaded and set up Git on your Linux Ubuntu machine. You can now start using Git for version control and managing your projects.

## Creating a new Git repository

To create a new Git repository, you can follow these steps:

Step 1: Navigate to the desired project directory:
   - Open a terminal or command prompt.
   - Use the `cd` command to navigate to the directory where you want to create the Git repository.
   - For example, if you want to create the repository in the "Projects" directory, run:
     ```
     cd /path/to/Projects
     ```

Step 2: Initialize a new Git repository:
   - Once you are in the desired directory, run the following command to initialize a new Git repository:
     ```
     git init
     ```

Step 3: Add files to the repository:
   - Place the files you want to include in the repository inside the current directory or create new files.
   - Use the following command to add all the files to the Git repository:
     ```
     git add .
     ```
   - The `.` specifies that you want to add all files in the current directory. If you want to add specific files, you can specify their names instead of using `.`.

Step 4: Commit the changes:
   - After adding the files, you need to commit them to the repository. A commit represents a snapshot of the files at a specific point in time.
   - Run the following command to commit the changes:
     ```
     git commit -m "Initial commit"
     ```
   - The `-m` flag is used to provide a commit message. You can replace "Initial commit" with a meaningful description of the changes made in the commit.

Step 5: Repository setup is complete:
   - Congratulations! You have created a new Git repository. The files in the directory are now being tracked by Git, and the initial commit has been made.

Additional steps (optional):
- If you want to collaborate with others or back up your repository, you can set up a remote repository on platforms like GitHub or GitLab. Refer to their documentation for instructions on creating a remote repository.
- You can also configure additional Git settings, such as ignoring certain files, setting up aliases, or specifying your username and email globally. These configurations can be done using the `git config` command.

## .git

In a Git repository, the `.git` folder is a crucial component that stores all the information related to version control. It contains various important configuration files and directories. Here's an overview of the contents of the `.git` folder:

1. `branches`:
   - This directory stores files that represent the branch heads, which are pointers to the latest commit in each branch.

2. `hooks`:
   - This directory contains scripts that can be executed automatically by Git at certain points, such as pre-commit or post-merge. These scripts allow you to customize and automate certain actions in your Git workflow.

3. `info`:
   - This directory contains a global exclusion file, which specifies patterns of files or directories that Git should ignore globally.

4. `logs`:
   - This directory stores logs and references to commits made in the repository. It includes logs for various actions, such as commits, branch updates, and ref updates.

5. `objects`:
   - This directory is where Git stores the data for each commit, tree, and blob (file). It uses a content-addressable storage model based on the SHA-1 hash of the content.

6. `refs`:
   - This directory contains references to different parts of the repository, such as branches and tags. It includes subdirectories like `heads` (branch references) and `tags` (tag references).

7. `config`:
   - This file stores the repository-specific configuration options, such as user name, email, and remote repository URLs. It can be edited manually or using Git commands like `git config`.

8. `HEAD`:
   - This file points to the currently checked-out branch or commit. It indicates the branch or commit that is currently being worked on.

9. `index`:
   - This file serves as the staging area and maintains information about the files and their current state before committing changes.

10. `README`:
    - This file contains information about the Git repository, such as its purpose, instructions, or documentation. It is not a default file and needs to be created manually.

These are some of the important files and directories within the `.git` folder. They play crucial roles in tracking changes, managing branches, storing data, and configuring the behavior of Git within the repository. It's important to note that modifying or deleting files within the `.git` folder can have severe consequences and should be done with caution.

## Fundamental git workflow

The fundamental Git workflow typically involves the following steps:

1. Initialize a Repository:
   - Create a new Git repository or clone an existing one. If you're starting from scratch, you can initialize a new repository using the command `git init` in the desired directory. If you're cloning an existing repository, you can use the command `git clone <repository URL>`.

2. Work on Files:
   - Create, modify, or delete files in your project directory. Git tracks changes at the file level.

3. Stage Changes:
   - Use the `git add` command to stage the changes you want to include in the next commit. You can stage specific files or use `git add .` to stage all changes in the current directory.

4. Commit Changes:
   - Create a commit to save the staged changes as a new snapshot in the Git history. Use the `git commit` command with a commit message to create a commit. For example, `git commit -m "Add new feature"`.

5. Repeat Steps 2-4:
   - Continue working on your project by modifying files, staging changes, and committing them. Each commit represents a specific set of changes.

6. Review History:
   - Use commands like `git log` to review the commit history. It shows information about each commit, such as the author, date, and commit message.

7. Create Branches:
   - Use branches to isolate different lines of development. Create a new branch with `git branch <branch-name>`, and switch to it using `git checkout <branch-name>`.

8. Merge Branches:
   - Use the `git merge` command to integrate changes from one branch into another. Switch to the branch you want to merge changes into and run `git merge <source-branch>`.

9. Resolve Conflicts:
   - If Git encounters conflicts during a merge, it will notify you. Conflicts occur when changes from different branches affect the same lines of code. Manually resolve the conflicts by editing the conflicting files, then stage and commit the resolved changes.

10. Push to Remote Repository:
    - If you are collaborating with others or want to back up your repository, push your local changes to a remote repository. Use `git push` to send your commits to the remote repository. You may need to specify the remote repository and branch, such as `git push origin main`.

11. Pull from Remote Repository:
    - If others have made changes to the remote repository, you can fetch and incorporate those changes into your local repository using `git pull`. It combines a `git fetch` to get the changes and a `git merge` to merge them into your current branch.

These are the fundamental steps of the Git workflow. By following this workflow, you can effectively track changes, collaborate with others, manage branches, and maintain a history of your project's development.

## Tracking Changes: Adding, committing, and reviewing changes to files

To track changes in files using Git, you can follow these steps:

1. Initialize a Repository:
   - If you haven't already done so, navigate to the project directory in your terminal and initialize a new Git repository with the command `git init`. This sets up Git to track changes in your project.

2. Check the Status:
   - Use the command `git status` to see the current status of your repository. It will display information about modified files, untracked files, and the branch you're on.

3. Stage Changes:
   - Before committing changes, you need to stage them. Staging involves selecting which changes you want to include in the next commit. Use the command `git add <file>` to stage specific files, or `git add .` to stage all changes in the current directory.

4. Review Staged Changes:
   - After staging changes, you can use the command `git diff --staged` to see a diff of the changes you have staged. This allows you to review the modifications before committing them.

5. Commit Changes:
   - Once you are satisfied with the staged changes, commit them using the command `git commit -m "Commit message"`. Replace "Commit message" with a meaningful description of the changes made in this commit. The commit represents a snapshot of the project at a specific point in time.

6. Review Commit History:
   - To review the commit history of your repository, use the command `git log`. It will display a list of commits, including the commit hash, author, date, and commit message. This helps you track the progress of your project and understand the changes made over time.

7. Repeat Steps 2-6:
   - Continue working on your project by modifying files, staging changes, and committing them as needed. You can use `git status` to check the status of your repository, `git add` to stage changes, and `git commit` to commit the staged changes.

These steps allow you to effectively track changes in your project using Git. By staging and committing changes, you create a history of snapshots that represent different stages of your project. The ability to review changes, compare versions, and revert to previous states provides a powerful and flexible way to manage your codebase.

## Branching and merging

Branching and merging are powerful features of Git that allow you to work on different lines of development and integrate changes from one branch to another. Here's how you can create branches, switch between branches, and merge changes in Git:

1. Create a Branch:
   - To create a new branch, use the command `git branch <branch-name>`. Replace `<branch-name>` with the desired name for your new branch. For example, `git branch feature-branch`.

2. Switch to a Branch:
   - Use the command `git checkout <branch-name>` to switch to a specific branch. This allows you to start working on that branch. For example, `git checkout feature-branch`.

3. View Branches:
   - To see a list of branches in your repository, use the command `git branch`. It will display all branches, highlighting the currently checked-out branch with an asterisk (*).

4. Work on the Branch:
   - After switching to a branch, make changes to the files in your project as needed. Git tracks the changes within the context of the currently checked-out branch.

5. Stage and Commit Changes:
   - Use the usual workflow of staging changes with `git add` and committing them with `git commit` to save your changes within the branch.

6. Switch Between Branches:
   - You can switch between branches at any time using the `git checkout` command. This allows you to move from one branch to another and work on different features or fixes.

7. Merge Changes:
   - Once you have made changes in a branch and want to incorporate them into another branch, you can perform a merge. First, switch to the target branch where you want to merge the changes.
   - Run the command `git merge <source-branch>` to merge the changes from the source branch into the target branch. For example, if you want to merge changes from `feature-branch` into the current branch, you would run `git merge feature-branch`.
   - Git will attempt to automatically merge the changes. In case of conflicts, where the same lines of code were modified in both branches, you will need to manually resolve the conflicts.

8. Resolve Merge Conflicts:
   - When a merge conflict occurs, Git will indicate the conflicting files and the specific lines causing the conflict. Edit the conflicted files to resolve the conflicts manually.
   - After resolving the conflicts, stage the modified files using `git add` and then commit the merge changes using `git commit`. Git will create a merge commit that incorporates the changes from both branches.

9. Repeat Steps 2-8:
   - Continue creating branches, switching between them, making changes, and merging as necessary to manage different lines of development in your project.

By utilizing branching and merging, you can work on different features, bug fixes, or experiments in isolated branches and merge them back into the main branch when they are ready. This approach allows for parallel development, effective collaboration, and a clean and organized version control history.

## Resolving conficts

Resolving conflicts that arise during a merge in Git involves manually addressing conflicting changes in the affected files. Here's a step-by-step guide on how to resolve merge conflicts:

1. Identify Conflicted Files:
   - When you run `git merge` and conflicts occur, Git will notify you about the conflicting files. You can use the command `git status` to see the list of files with conflicts.

2. Open Conflicted Files:
   - Open each conflicted file using a text editor or an integrated development environment (IDE). Conflicts are marked within the file with special markers.

3. Understand Conflict Markers:
   - Conflict markers are used by Git to indicate conflicting changes. They have the following format:
     ```
     <<<<<<< HEAD
     // Changes from the current branch (HEAD)
     =======
     // Changes from the branch being merged
     >>>>>>> branch-name
     ```

4. Resolve Conflicts:
   - Review the conflicting changes marked by the conflict markers. Decide which changes to keep, modify, or remove to resolve the conflict.
   - Edit the file manually to address the conflicts, keeping only the desired changes and removing the conflict markers. Ensure that the resulting code is correct and functional.

5. Save the Resolved File:
   - After resolving the conflicts in a file, save the changes.

6. Stage the Resolved Files:
   - Use the command `git add <file>` to stage the resolved file. Replace `<file>` with the path to the resolved file.
   - Repeat this step for each resolved file.

7. Continue the Merge Process:
   - Once all conflicting files are resolved and staged, you can complete the merge process by running `git commit`. Git will automatically create a merge commit with the resolved changes.

8. Verify the Merge:
   - After committing the merge, review the changes to ensure that the conflicts were resolved correctly and the merged code functions as expected.

9. Repeat Steps 1-8 (if necessary):
   - If there are multiple conflicts or subsequent conflicts arise during the merge process, repeat the previous steps until all conflicts are resolved.

It's important to note that during conflict resolution, you have full control over which changes to keep. It allows you to carefully merge conflicting code and make sure the resulting code is correct and coherent. Communication with other developers involved in the merge process can be helpful in understanding conflicting changes and finding appropriate resolutions.

Remember to commit the resolved files and test the merged code to ensure it works as expected before continuing with your project.

## Rebasing

Rebasing is a powerful Git operation that allows you to integrate changes from one branch onto another by moving or replaying commits. Rebasing is particularly useful when you want to incorporate the latest changes from a branch into another branch while maintaining a linear commit history. Let's dive deeper into rebasing in Git:

1. Basic Rebasing:
   - The basic syntax for rebasing is `git rebase <base-branch>`. It moves the commits from the current branch onto the tip of the `<base-branch>`.
   - For example, if you are on a feature branch and want to rebase it onto the `main` branch, you would run `git rebase main`.

2. Interactive Rebasing:
   - Git also provides an interactive mode for rebasing, which offers more control over the process. Use `git rebase -i <base-branch>` to enter interactive rebasing mode.
   - In interactive mode, Git presents you with a list of commits to be rebased. You can choose to modify, reorder, squash, or drop commits during the rebase process.
   - By editing the presented list, you can change the order of commits, combine multiple commits into one, or even exclude certain commits from the final history.

3. Resolving Conflicts during Rebasing:
   - Similar to merging, rebasing can also lead to conflicts when Git encounters incompatible changes between the current branch and the base branch.
   - When conflicts occur during a rebase, Git pauses the process and allows you to resolve the conflicts manually, just like resolving merge conflicts.
   - After resolving the conflicts, use `git add` to stage the resolved changes, and then run `git rebase --continue` to continue the rebasing process.
   - If you want to abort the rebase at any point due to conflicts or other issues, use `git rebase --abort` to return to the state before the rebase.

4. Advantages of Rebasing:
   - Rebasing can create a cleaner and more linear commit history, especially when used to integrate feature branches into the main branch.
   - It avoids creating merge commits, resulting in a cleaner branch history and easier navigation through the commit log.
   - Rebasing can help maintain a more coherent and organized history, which can be beneficial when working on a project with multiple collaborators.

5. Cautionary Notes:
   - Rebasing rewrites commit history, so it should be used primarily on local branches that haven't been pushed to a shared repository.
   - Once you've rebased a branch, avoid pushing it to a public repository or sharing it with other developers. Doing so may cause issues for others who have already based their work on the previous version of the branch.

It's important to understand that while rebasing offers advantages, it should be used judiciously and with consideration for collaboration and shared repositories. It's recommended to communicate and coordinate with other team members when rebasing to avoid disrupting the workflow of others working on the same codebase.

## Customization

Customizing Git allows you to configure various aspects of Git's behavior to suit your preferences and workflow. Git provides several customization options that can enhance your productivity and streamline your development process. Here are some key aspects of customizing Git:

1. Git Configuration:
   - Git configuration can be set at three different levels: system-wide, user-specific, and repository-specific. Use the following commands to configure Git:
     - System-wide configuration: `git config --system`
     - User-specific configuration: `git config --global`
     - Repository-specific configuration: `git config` (inside the repository)

2. User Information:
   - Set your name and email address that will be associated with your commits using the following commands:
     - Global configuration: `git config --global user.name "Your Name"`
     - Global configuration: `git config --global user.email "your.email@example.com"`

3. Aliases:
   - Create custom Git command shortcuts called aliases to save time and reduce typing. Aliases allow you to define custom commands or abbreviations for commonly used Git operations. For example:
     - Global configuration: `git config --global alias.co checkout`
     - Global configuration: `git config --global alias.br branch`

4. Editor:
   - Customize the default text editor used by Git for commit messages and other interactive operations. You can set your preferred editor using the `core.editor` configuration variable. For example:
     - Global configuration: `git config --global core.editor "code --wait"`

5. Ignore Files:
   - Customize the `.gitignore` file to specify files or patterns that Git should ignore. This is useful for excluding build artifacts, temporary files, or sensitive data from being tracked by Git.

6. Hooks:
   - Git allows you to set up hooks, which are scripts that execute automatically at certain points in the Git workflow, such as pre-commit or post-merge. You can create custom hooks to enforce coding standards, perform automated tests, or trigger deployment processes.

7. UI Customization:
   - Git provides various tools and extensions that offer a graphical user interface (GUI) to interact with Git. You can explore Git GUI clients like GitKraken, Sourcetree, or GitExtensions to customize your Git experience visually.

8. Git Config File:
   - You can directly edit the `.git/config` file in a repository to manually customize Git options. This file contains specific settings for that repository and overrides global or system-wide configurations.

Remember that customizing Git is a matter of personal preference and workflow. Experiment with different configurations to find what works best for you and your team. Keep in mind that shared repositories and collaborative workflows may require coordination and agreement on certain customization choices to ensure consistency across the team.

