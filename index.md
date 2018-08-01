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

    
## Bandit Level 30 → Level 30

Again, we start by setting up a new folder (or any place where we can clone the repo). This time we have a clue: No passwords in production! what does that mean? probably that passwords were used in development. Let's dig into the logs:
`git log`
nothings there. Let's read packed-refs (Git compresses necessary refs into this file). We have a ref for development, after checking it out, we can read the README.md. We have the password there:
- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf
