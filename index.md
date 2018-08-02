# overthewire.org bandit challanges (27-34) solutions

Since more levels were added and nobody posted solutions for them, I decided to do it by myself.

## Bandit Level 26 → Level 27

Here we basically continue with the same elusive shell. Once more, we should open our shell in the smallest size possible (allowing more to take action). The only tool we have is Vim and our hint is the "ls" command. Searching in /etc/passwd won't tell us anything, as well as trying to open /etc/bandit_pass/bandit27 or editing other files. Trying to execute "ls" command in Vim, or any other command won't work. Vim has an option to set a shell with :set command, so now we can set it to bash and work our way to the key.
in the main screen, press 'v' to enter Vim and then we execute:
`:set shell=/bin/bash`

now we have set up a bash shell. Let's run `:!ls -la`. Now we have found we have a file named "bandit27-do". This files gives us the ability to run files with bandit27 premissions. Let's run `:!./bandit27-do cat /etc/bandit_pass/bandit27` and walla!, we have the pass:
![bandit27](https://github.com/int3rsys/Bandit27-34/blob/master/Images/bandit27.png)

The password is: 3ba3118a22e93127a4ed485be72ef5ea

## Bandit Level 27 → Level 28

Now we need to work with git. We receive an address to clone the repository throghh ssh. Up until now, I worked with PuTTY but signing in with it to bandit27 or bandit27-git won't work (not for me,at least). I switched to puttygen (working on windows) and connected to bandit27. `get clone ssh://bandit27-git@localhost/home/bandit27-git/repo` won't work in the home folder, therefore I switched the folder to /tmp, then I cloned it there with adding .git extension in the end, i.e: `get clone ssh://bandit27-git@localhost/home/bandit27-git/repo.git/`. In `/tmp/repo/` we have a file called README. the password for next level is: 0ef186ac70e04ea33b4c1853d2526fa2

## Bandit Level 28 → Level 29

Okay, we continue with learning about git. Now, as we did before, will clone the repo in a a temporary folder:
`
mkdir /tmp/xxx
cd xxx
get clone ssh://bandit28-git@localhost/home/bandit28-git/repo`

a bit of searching will reveal a file called "README.md", but the password is missing out. Searching in .git folders won't give us anything. After reading git manuals, we can check the project's logs with `git log`. We can see some important changes, hence we can pull them and see if we can exctract the password:
>[33mcommit 196c3edc79e362fe89e0d75cfeef079d8c67beef[m[m Author: Morla Porla <morla@overthewire.org>[m Date:   Sun Jul 22 14:47:13 2018 +0200[m [m add missing data[m

seems interesting. Let's get it:
`git checkout 196c3edc79e362fe89e0d75cfeef079d8c67beef`
(**_196c3edc79e362fe89e0d75cfeef079d8c67beef_** is the commit id)
...and Walla! now the password is inside README.md:
- username: bandit29
- password: bbc96594b4e001778eee9975372716b2

    
## Bandit Level 29 → Level 30

Again, we start by setting up a new folder (or any place where we can clone the repo). This time we have a clue: No passwords in production! what does that mean? probably that passwords were used in development. Let's dig into the logs:
`git log`
nothings there. Let's read packed-refs (Git compresses necessary refs into this file). We have a ref for development, after checking it out, we can read the README.md. We have the password there:
- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf

## Bandit Level 30 → Level 31

Clone the given repo, again. Now, we don't have a pass at all, but we have an interesting ref. Trying to checkout the ref (of branch secret) won't work here, because it's broken. Trying to comit, fetch or other actions won't work as well. The way to get the pass is to read the file itself. That can be done by the following methods:
`git cat-file -p f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea`
or
`git show f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea`
both ways will output our pass:
47e603bb428404d265f59c42920d81e5

## Bandit Level 31 → Level 32

Like in previous levels, we clone our repo and read the README.md file. It's said that we need to push a file called key.txt. Let's create one: `echo "May I come in?">key.txt` in the root directory
Now we need to upload this file to the git repository:
`git add -f key.txt`
`git commit -m key.txt`
`git push origin master`
and Walla! we have the password:
>remote: ### Attempting to validate files... ####[K
>remote:
>remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.[K
>remote:
>remote: Well done! Here is the password for the next level:[K
>remote: 56a9bf19c63d650ce78e6ec0354ee45e[K
>remote:
>remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.[K
>remote:

## Bandit Level 32 → Level 33

At last we don't have to deal with git. We first login into our home directory and find out that we have an ELF file called uppershell. Running it will give us a strange behaviour. It seems that we cannot execute any command, but we have a clue: *sh*, so after googling for sh shell, I have found a manual for it: http://heirloom.sourceforge.net/sh/sh.1.html
inserting `$0` will give us the interactive bash, now we can actually do something. We want privileged shell, therefore we type:
`set -p`
and now all we have to do is to read our password:
`cat /etc/bandit_pass/bandit23`
and we have:
c9c3199ddf4121b10cf581a98d51caee

Apparently it's the last level, thank you overthewire.org team for creating this challanges!
>Congratulations on solving the last level of this game!

>At this moment, there are no more levels to play in this game. However, we are constantly working
>on new levels and will most likely expand this game with more levels soon.
>Keep an eye out for an announcement on our usual communication channels!
>In the meantime, you could play some of our other wargames.

>If you have an idea for an awesome new level, please let us know!

until next time.
