# Version Control

## What is Version Control?

Version control is like a ‘history’ of your code. It records and stores the changes you made to your code over time. It also allows you to save some changes and start a new feature without losing the progress by using a different branch. This enables functionalities like comparing your code and changes to the “original” code you started with, revert (some of) your changes or merge changes. This gets increasingly important if you develop software as a team! Different developers can work on different features without getting into each others ways.

## Why should you use Version Control?

A change log of your code is always handy! It helps with collaboration, prevents errors when multiple developers work on the same project, prevents data loss. It’s also used to review changes and have other people read through your changes before you want to merge them into the project, this is called “Code Review”.

## What tools can you use?

There are many tools available that help you to version your code properly. [Git](https://git-scm.com/) has by far the highest market share with almost 90%, so let’s just go with that. The git command line interface (CLI) can be a bit overwhelming at first, but it’s worth having a closer look at. Ever though there are a lot of graphical user interfaces (GUI) that show what is happening in a more visual way, I would recommend to learn the basic CLI commands non the less.

## How to use git?

To get started you want to either use the `git init` command to start out a new project or use the `git clone [URL]` command to download an existing project and start participating right away.

You will find yourself in the ‘main’ branch after that. To not interfere with other people working on this project, you should start out by creating your own branch from here by using `git branch [Branch Name]` followed by `git checkout [Branch Name]`. While you created a branch with the first command, you switched to it in the second command. To save some time you can also use the shorthand `git checkout -b [Branch Name]` the `-b` flag switches and creates the specified branch.

Now that you are on your own branch, you can start developing and make some changes. Whenever you want to specify files that you want to track, use `git add [File Name]` to track it. You can also specify many files or even folders by using `*` but to be honest, you will most likely use `git add .` most, to track ALL files in that path including files in folders.

When you are done with your changes you want to either commit them by using `git commit -m "[Commit Message]"`. This will bundle all changes into a commit with a short but descriptive message that tells others what you’ve done. If you do not want to commit your changes, you might want to use

- `git diff` to see what has been changed

- `git reset` [file] to unstage your changes

- `git stash push` to push those changes into a stash that you can come back to later by using `git stash pop` with will load all changes from the stash into your current state.

All this happens on your local machine, if you want to share your code with the others you want `git push` those changes.

To see what others did you can use `git fetch`, this will load all branches from the remote state. If you want to fetch commits and their changes and merge them into your current state use `git pull`.

Lastly there is `git merge [Branch Name]` which will try to merge changes from another branch into your current state. This might result into merge conflicts if both your changes and the ones from the branch you merged target the same file / lines of code.

These are the basics! Since git is so popular, you can easily google your way to the solution needed. You can almost never lose code, unless you use `reset` commands or flags like `--force` or `--hard` which you should not use without extra care!

## Common Workflows

Many teams use git, and many teams have many conventions about how to work with it. There might be conventions about branch names, commit messages or pull requests. But what most of them have in common, are Merge or Pull Requests, which is the same thing - different name. It’s the process of requesting to merge your branch into the main (or any other) branch. Someone will then most likely have a look at what your changes do and decide on whether or not the changes are okay or not - this is called a Code Review. After the merge happened you start all over :)
