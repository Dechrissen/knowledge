# git tips

### https vs. ssh
If git asks for your password when trying to push, you're using the https clone of the repo.  

Solution: Switch to ssh  

`git remote set-url origin git@github.com:username/repo.git`

### revert a commit
this will revert a commit (and create a new one with the changes)
`git revert <commit hash>`

