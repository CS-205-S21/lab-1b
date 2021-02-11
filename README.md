# Lab 1b: Git

## Lab goals

In this continuation of Lab 1a, you will be working with Git, which is a fast, scalable, distributed version control system with a rich command set that provides both high-level operations and full access to internals. For this lab, you and your partner will clone a git repository from GitHub to your account on the lab machine. Then you will explore Git a bit to help set up for the coming semester. Note — I will be adding myself to your lab account and checking out a copy of your running labs. 

## Summary of your configuration so far
Having completed Lab 1a, you can now ssh from your local machine to your lab account on the department's server. Congratulations! If you have not completed Lab 1a, please complete that assignment before proceeding.

The following diagram depicts what you have configured so far:
```
---------------------      ssh      ---------------------
| Your Local Machine|   <--------> | user@139.147.9.XXX  |
---------------------               ---------------------
```

By the end of this lab, you will be able to connect to repositories hosted on GitHub using ssh:

```
 -------------------      ssh    --------------------    ssh    --------
| Your Local Machine |  <-----> | user@139.147.9.XXX |  <----> | GitHub |
 -------------------             --------------------           --------
```

You will also [practice working with some basic git commands](## Git'ing started).

## Add your public key to your GitHub account
First, you will need to add your ssh key (id_rsa.pub) to your GitHub account. GitHub provides a handy set of instructions for completing this step. Follow along [here](https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account). Make sure you are adding your key to the GitHub account you are using for this class and not another personal account.

## Testing your configuration
Now let's test to see if GitHub recognizes your ssh key. Run the following command from your local terminal:

```
ssh -T git@github.com
```

You should see:

```
> Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

## Forwarding your ssh agent to the lab server
At this point, you have connected your local machine to the lab server (completed in Lab 1a) and configured GitHub to recognize your ssh key (in the previous step). All that remains is to enable the remote server to use this ssh key to communicate with GitHub! 

To do this, you will need to create (or edit, if one already exists) your ```~/.ssh/config``` file.


### Option 1
```
cd ~/.ssh                                 # Move to the .ssh directory
touch config                              # Make a new file called config
echo "Host 139.147.9.*" >> config         # Append Host 139.147.9.* to that file
echo "    ForwardAgent yes" >> config     # Append     ForwardAgent yes to that file
```

### Option 2
This repository contains a [config](/config) file. Download it and add it to your ~/.ssh folder using your OS's file explorer.

## Testing your configuration
- ssh to the remote lab server ```ssh username@139.147.9.XXX```
- Test the connection to GitHub from the server ```ssh -T git@github.com```
- You should see the same message as before, ```> Hi username! You've successfully authenticated, but GitHub does not provide shell access.```

--------------------------------------------------------------
## Git'ing started

Git is one of the most widely-used version control tools in industry. As you will see in the remainder of this lab assignment, it's easy to get started! However, there are many useful git features that this lab will not cover. I'd strongly encourage you to spend some time familiarizing yourself with git, since you'll be using it for the rest of the semester (and perhaps long into the future). There are quite a few well-done online tutorials:

- [git-it](https://github.com/jlord/git-it-electron/releases)
- [git branching](https://learngitbranching.js.org/)
- [visualizing git](http://git-school.github.io/visualizing-git/)
- And many more...


Some important things to remember:
  1. The repository stored in your lab account should not be directly modified once created. Ask me for further explanation if this is not clear!
  2. As you move forward, your labs will be represented as a sequence of changes.
  3. Commit these changes often (see below).
  4. Each lab will be identified by a "tag," which will indicate a specific point in the sequence of changes, identifying it as your submission of lab 2, 3, 4, etc.

Work through each of the following topics in order:
  1. If you are using a laptop, install Git if it is not already available. You can check availability by executing git --version.
    On Linux use either apt-get or dnf if you are using Ubuntu or Fedora respectively.
    On Windows 10, use this link.
    On macOS, use this link.
  2. Be sure both you and your partner can access the lab account and SSH is properly configured from Lab 1a.
  3. Learn about the basics of Git and create a repository on your lab account by completing the Intro to Git tutorial (see Tutorials section).
  4. For information about the various commands, execute git help
  5. The next step is to make your Git repository aware of who you are, to be able to identify you in the changes you will be making. Both lab partners should execute the following commands with their respective information.

```
elias$ cd repo_member1_member2
elias$ git config --global user.name elias
elias$ git config --global user.email falconie@lafayette.edu
```

  6. This will store the information local to this particular repository. If you are working from a Windows 10 Bash shell you need to run the following:

```
git config --global push.default simple
```

7. The next steps will get you started with the main Git commands:

First thing that needs to be done is to create some content. One lab partner should execute the following commands on their machine:

```
elias$ cd repo_member1_member2/
elias$ mkdir test
elias$ git add test
elias$ cd test/
elias$ touch file
elias$ cd ..
elias$ git add test
elias$ git commit -m "Added test dir"
[master (root-commit) 3a4a2f3] Added test dir
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 test/file
elias$ git push
Counting objects: 4, done.
Writing objects: 100% (4/4), 247 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To ssh://139.147.9.134/home/lab_1_11/repo_member1_member2.git
* [new branch] master -> master
```

The various outputs should appear as you execute the commands. The one new command here is touch, which creates a new file. The three commands following the git command are:
  - add – adds the entire directory to the Git staging area;
  - commit – finalizes the changes to the local repository;
  - push – moves/pushes the repository to the origin repository.

If you forget to include a message (using the -m option) with the commit command, you may find yourself in the vi editor. If you do find yourself here, type in your message and then type the following key combination ":wq". The colon places the editor into command mode, the "w" means write, and the "q" indicates quit.

Once these commands are complete, the other lab partner can execute git pull to retrieve the information stored in the
origin repository.
```
elias$ cd repo_member1_member2/
elias$ git pull
```

This should result in producing a duplicate directory in the lab partner's directory. Examine the directory to be sure this has happened. The next step is for the lab partner who pulled the information from the origin repository. They should open the file "test/file" with the nano program, modify it, and save the file. Once they have done this, they should run the following command:

```
elias$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
(use "git add ..." to update what will be committed)
(use "git checkout -- ..." to discard changes in working directory)
modified: test/file
Untracked files:
(use "git add ..." to include in what will be committed)
test/file~
no changes added to commit (use "git add" and/or "git commit -a")
```

The information displayed is Git's way of helping you figure out what to do. Thus you should follow these steps:

```
elias$ git add test/file
elias$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
(use "git reset HEAD ..." to unstage)
modified: test/file
Untracked files:
(use "git add ..." to include in what will be committed)
test/file~
elias$ git commit -m "Updated the files"
[master d6979f7] Updated the files
1 file changed, 1 insertion(+)
elias$ git push
Counting objects: 4, done.
Writing objects: 100% (4/4), 303 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To ssh://139.147.9.134/home/lab_1_11/repo_member1_member2.git
3a4a2f3..d6979f7 master -> master
```

Complete this step by having the other (first) lab partner execute a git pull.

To find out what is going on, one can always use the "git log" command. Both lab partners should execute this command.

These basic steps are the essentials of working with Git. But there is one situtation that can be tricky, which arises when both lab partners modify the same file(s) simultaneously. To simulate this, both partners should add their name to the first line in the file, both then execute an add and a commit. Then one of the lab partners should do a push, while the other partner waits.

```
elias$ git push
Counting objects: 4, done.
Writing objects: 100% (4/4), 316 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To ssh://139.147.9.134/home/lab_1_11/repo_member1_member2.git
d6979f7..db34a9d master -> master
Now the other partner should attempt a push.
elias$ git push
To ssh://139.147.9.134/home/lab_1_11/repo_member1_member2.git
! [rejected] master -> master (fetch first)
error: failed to push some refs to
'ssh://lab_1_11@139.147.9.134/home/lab_1_11/repo_member1_member2.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Things seem broken – time to correct them by first pulling the first lab partner's code.

```
elias$ git pull
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
From ssh://139.147.9.134/home/lab_1_11/repo_member1_member2
d6979f7..db34a9d master -> origin/master
Auto-merging test/file
CONFLICT (content): Merge conflict in test/file
Automatic merge failed; fix conflicts and then commit the result.
```

The reason for the conflict will become obvious. To correct it, look in the file and see where the issue is identified.

```
elias$ more test/file
<<<<<<< HEAD
two I have added something...
=======
one I have added something...
>>>>>>> db34a9d636c6e30324f52cdc8510a355a8b23ec1
```

The correction should come in the form of modifying the file to resolve the differences and removing the three lines that were added by Git. You can see the resolution I took below.

```
elias$ nano test/file
elias$ more test/file
I'm keeping both!!!
two I have added something...
one I have added something...
```
Now use git add, commit and push the file.

```
elias$ git add test/file
elias$ git commit -m "cleaning up conflict"
[master d2af95a] cleaning up conflict
elias$ git push
Counting objects: 8, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (8/8), 647 bytes | 0 bytes/s, done.
Total 8 (delta 0), reused 0 (delta 0)
To ssh://139.147.9.134/home/lab_1_11/repo_member1_member2.git
db34a9d..d2af95a master -> master
```

The other lab partner should now see the negotiated change by executing a git pull.
Experiment further with the basic operations until you feel comfortable with what is going on. You can find further information via the "git help" command.

8. Now you have done the basics, get some practice with the more interesting features using the interactive tutorial, called [Learning Git Branching](https://learngitbranching.js.org/).

## Submission
Submission will be by the instructor examining your repository.
