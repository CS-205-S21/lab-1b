# Lab 1b: Git

## Lab goals

In this continuation of Lab 1a, you will be working with Git, which is a fast, scalable, distributed version control system with a rich command set that provides both high-level operations and full access to internals. It lets you share a developing resource to which one or more people can contribute over time. Through specific commands, it stores things as a sequence of changes, allowing you the ability to comment on changes that are being made. If two developers are contributing to the same piece of code, then commit (submit) these pieces of code, Git will facilitate combining (merging) the code.

The general structure of a git repository is that there are multiple repositories, where one git repository starts as the origin and additional repositories are "cloned." Clones are called working repositories and belong to the developer working in/on their personal, local environment/machine. This allows each developer to do their own work without being connected to the originating repository, but then occasionally sync up the work being done.



For this lab, you and your partner will clone a git repository from GitHub to your account on the lab machine. Then you will explore Git a bit to help set up for the coming semester. Note — I will be adding myself to your lab account and checking out a copy of your running labs. 

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

You will also practice working with some basic git commands.

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

## Cloning your first repository
For this exercise, you will clone an existing repository onto the lab server from GitHub and make a few changes to the code I have provided you with.

First, let's make a directory to hold this repository. On the remote server run:

```
cd ~          # Make sure we are in our home dir
mkdir labs    # Make a new dir for the repo
cd labs       # Move to the new dir
```

Now follow [these instructions](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository) to clone your team's lab-1b repository. *Make sure to select "ssh" option instead of "https" when copying the link.

The instructions linked above conclude with you running the following command on the remote server:
```
git clone git@github.com:YOUR_USER_NAME_HERE/lab-1b.git
```

To confirm that everything was cloned correctly:
```
cd lab-1b
ls          
>config  README.md  test.sh #You should now see these three files
```

You can also test running the shell script:
```
./test.sh
```

The next step is to make your Git repository aware of who you are, to be able to identify you in the changes you will be making. Both lab partners should execute the following commands with their respective information.

$ git config --global user.name justin                    # Obviously, replace with your name
$ git config --global user.email smithjus@lafayette.edu   # and your email

## Making changes

The next steps will get you started with the main Git commands. Note that from here forward, I will prefix commands with a '$' to differntiate them from output.

First thing that needs to be done is to create some content. One lab partner should execute the following commands on their machine:

```
$ mkdir test
$ git add test
$ cd test/
$ touch file
$ cd ..
$ git add test
$ git commit -m "Added test dir"
  [master (root-commit) 3a4a2f3] Added test dir
  1 file changed, 0 insertions(+), 0 deletions(-)
  create mode 100644 test/file
$ git push
  Counting objects: 4, done.
  Writing objects: 100% (4/4), 247 bytes | 0 bytes/s, done.
  Total 4 (delta 0), reused 0 (delta 0)
 ...
 * [new branch] master -> master
```

The various outputs should appear as you execute the commands. The one new command here is touch, which creates a new file. The three commands following the git command are:
  - add – adds the entire directory to the Git staging area;
  - commit – finalizes the changes to the local repository;
  - push – moves/pushes the repository to the origin repository.

If you forget to include a message (using the -m option) with the commit command, you may find yourself in the vi editor. If you do find yourself here, type in your message and then type the following key combination ":wq". The colon places the editor into command mode, the "w" means write, and the "q" indicates quit.

Once these commands are complete, the other lab partner can execute git pull to retrieve the information stored in the origin repository.

```
$ cd lab_1b/
$ git pull
```

This should result in producing a duplicate directory in the lab partner's directory. Examine the directory to be sure this has happened. The next step is for the lab partner who pulled the information from the origin repository. They should open the file "test/file" with the nano program (or their prefered text editor), modify it, and save the file. 

```
nano test
```

Once they have done this, they should run the following command:

```
$ git status
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
$ git add test/file
$ git status
 On branch master
 Your branch is up-to-date with 'origin/master'.
 Changes to be committed:
 (use "git reset HEAD ..." to unstage)
 modified: test/file
 Untracked files:
 (use "git add ..." to include in what will be committed)
 test/file~
$ git commit -m "Updated the files"
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
$ git push
 Counting objects: 4, done.
 Writing objects: 100% (4/4), 316 bytes | 0 bytes/s, done.
 Total 4 (delta 0), reused 0 (delta 0)
 ...
 d6979f7..db34a9d master -> master
```

Now the other partner should attempt a push.

```
$ git push
 To ...
 ! [rejected] master -> master (fetch first)
 error: failed to push some refs to
 ...
 hint: Updates were rejected because the remote contains work that you do
 hint: not have locally. This is usually caused by another repository pushing
 hint: to the same ref. You may want to first integrate the remote changes
 hint: (e.g., 'git pull ...') before pushing again.
 hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Things seem broken – time to correct them by first pulling the first lab partner's code.

```
$ git pull
 remote: Counting objects: 4, done.
 remote: Total 4 (delta 0), reused 0 (delta 0)
 Unpacking objects: 100% (4/4), done.
 ...
 d6979f7..db34a9d master -> origin/master
 Auto-merging test/file
 CONFLICT (content): Merge conflict in test/file
 Automatic merge failed; fix conflicts and then commit the result.
```

The reason for the conflict will become obvious. To correct it, look in the file and see where the issue is identified.

```
$ cat test/file
 <<<<<<< HEAD
 two I have added something...
 =======
 one I have added something...
 >>>>>>> db34a9d636c6e30324f52cdc8510a355a8b23ec1
```

The correction should come in the form of modifying the file to resolve the differences and removing the three lines that were added by Git. You can see the resolution I took below.

```
$ nano test/file
$ cat test/file
 I'm keeping both. We are a team!!!
 two I have added something...
 one I have added something...
```
Now use git add, commit and push the file.

```
$ git add test/file
$ git commit -m "cleaning up conflict"
 [master d2af95a] cleaning up conflict
$ git push
 Counting objects: 8, done.
 Delta compression using up to 8 threads.
 Compressing objects: 100% (3/3), done.
 Writing objects: 100% (8/8), 647 bytes | 0 bytes/s, done.
 Total 8 (delta 0), reused 0 (delta 0)
 To ...
 db34a9d..d2af95a master -> master
```

The other lab partner should now see the negotiated change by executing a git pull.
Experiment further with the basic operations until you feel comfortable with what is going on. You can find further information via the "git help" command.


## Submission
Submission will be by the instructor examining your repository and your lab account.
