# BFG Repo Cleaner

Amazing tool that can delete old/sensitive data from a git repo. Rewrites git history.

Download the .jar file here: https://rtyley.github.io/bfg-repo-cleaner/

Follow the instructions to clone a bare repo of your repo (new location on local machine vs. the old one, but the old one can be updated with the changes after this process is done).

Then, for example, if you want delete all instances of file starting with 'secret' and ending in `.txt`, just run:

```
java -jar <path to .jar file> --delete-files secret*.txt  my-repo.git 
```

Done! Sensitive files deleted from git history.

Then,

```
cd my-repo.git
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```
Then push with

```
git push
```
Done!

Now you can return to the location of your original clone on your local machine and run:

```
git pull origin master
```

or whatever the branch is named.


You might need to set config to rebase branches, e.g.

```
git config pull.rebase true
```

Now the repository history is cleaned of those sensitive files.