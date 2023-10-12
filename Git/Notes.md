## Sync forked repo with original (upstream) repo
```bash
git remote add upstream <original-repo-url>
# fetch latest changes from the original repo
git fetch upstream
# checkout your local branch
git checkout <branch>
# merge the changes from the original repo's master branch into your local branch
git merge upstream/master
```
