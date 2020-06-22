# Meeting Seven (Github Fundamentals and Gitlab)

## Question That Will Be Answered
* What is Git and how do we use it properly?
* What is Gitlab?
* What are some Git commands I will use often? 
* What is the difference between a fork and a branch?
* How to use SSH keys with Git?
* What is Gitlab runner and how do I install it?
* How do I do basic CI/CD with GitLab?

## Github Commands
* git config
* git clone
* git add
* git commit
* git branch
* git checkout
* git merge
* git remote
* git push
* git pull
* git stash

## Github Best Practices
* Never commit to master without review and set up branch protection (Master is our source of truth)
* Who is tho code owner and who can authorize merges to main branch
* Develop in separate branch and merge to develop
* Commit Frequently and often
* DON'T COMMIT SECRETS TO GITHUB EVERRRRRRRRRRRRRRRRRRRRRR!!!!! 
* Make sure you use .gitignore to ignore secret files
* Comment Comment Comment you commits

## Github Examples

* Will do this if people are not comfortable with Git.

## Difference Between Branch and Fork
Branching and forking provide two ways of diverging from the main code line. Both Mercurial and Git have the concept of branches at the local level. A repository code branch, like a branch of a tree, remains part of the original repository. The code that is branched (main trunk) and the branch know and rely on each other. Like a tree trunk's branch, a code branch knows about the trunk (original code base) it originated from.

## Setting up Our SSH Keys

I will walk everyone through this part

## Creating a Project with Our Website
Lets start by making sure that we have Git installed on our machines

    sudo apt install git

Create a Git repo with the name you would like. Now lets go to the folder where the container for you website is. Now we have to make this into a Git repo and connect it to our remote repository. 

* git init
* git add README.md (If you want a read me)
* git commit -m "first commit"
* git remote add origin <url>
* git push -u origin master


## So What is This Origin Thing?
In Git, "origin" is a shorthand name for the remote repository that a project was originally cloned from. More precisely, it is used instead of that original repository's URL - and thereby makes referencing much easier.

Note that origin is by no means a "magical" name, but just a standard convention. Although it makes sense to leave this convention untouched, you could perfectly rename it without losing any functionality.

## How to Use .gitignore
This file allows one to automatically ignore files that are specified here. To create this create the file within the root of your Git repo.
    
    touch .gitignore

Now within the file we can do something like this

    # ignore all logs
    *.log
    # ignore all secrets
    *.secret
    # ignore file pattern
    foo\[01\].txt

You can also define these within directories of the repo, allowing shared repos to ignore file types in certain components 


## What is GitLab?
GitLab is a complete DevOps platform, delivered as a single application. This makes GitLab unique and makes Concurrent DevOps possible, unlocking your organization from the constraints of a pieced together toolchain. Join us for a live Q&A to learn how GitLab can give you unmatched visibility and higher levels of efficiency in a single application across the DevOps lifecycle.

## Short Exercise 
* Go to my Gitlab server at gitlab.anthonyhanna.com and add the Docker container website onto the server
* Add SSH keys
* Pull Project using the SSH keys
* Create a .gitignore and a Readme
* Commit changes
