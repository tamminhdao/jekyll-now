---
layout: page
title: The most basic intro to git/gitHub
---


I have started to use version control (git/ gitHub) with more frequency: making commits, submit pull requests for code review, branching, merge approved branches to master. Below is a summary of the git commands I use most frequently and a few note-to-self I've collected along the way. 

The main thing to know about git is: Once you've made a commit of your work, you cannot lose it. You can always go back and retrieve it. In some extremely messed up situation, you might have to ask someone or do some research to find the series of command that will help you retrieve your work. But be assured that there is a copy stashed somewhere.

That brings me to a second note: Make frequent incremental commits with descriptive comments in case you need to retrace your work to a particular point in time.

Last but not least: Don't be afraid to make drastic changes to your file. You can always revert back to your last working version. 


## Create a git repo:
    `cd` into the directory
    `git init`

## Make a commit in git:

    `git add <filename >`
    `git commit -m "descriptive commit message"`
    `git push origin <branch>`

## Useful command:
    `git status` - check if there is any file to be added or committed
    `git log` - show a record of previous commits

## Create a new branch to develop a new feature/ functionality:

    `git branch <branch name>`
    `git checkout <branch name>`

or just use a oneliner:
     `git checkout -b <branch name>`

*Note: always make a branch off master*

## Do a git rebase to put the working branch on top of the newly merged master branch:

    on master: git pull (to sync with the lastest change on github repo)
    on <branch>: `git rebase master`

    If there are conflicts, fix them, then `git rebase --continue`
    In case something went wrong during the conflict resolution, resort to `git reflog` and `git cherry-pick`

## To work on the same repo from two computer, with the same gitHub account:
    push all branches from computer 1 to designated gitHub repo
    On computer 2: `git clone` (this only clone the master branch)
        to pull a particular existing branch from gitHub:
                    'git checkout <branch>' (git will automatically pull the existing branch and switch to that branch)

## What to look for in a pull request
Last week Zack talked to us during Zagaku about his 5 steps to evaluate a PR. I thought it was a good guide:
    1. Does the build pass?
    2. What the PR supposed to do?
    3. Does the PR do what it's supposed to do?
    4. Sweat the small stuff: typo, format, white space
    5. Assess the quality of the code
