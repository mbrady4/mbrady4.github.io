+++
title = "Git Basics"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++

### Basics
- Use git status to monitor submissions ('git status')
- Always add files before committing ('git add .')
- Commit files after they are added ('git commit -m 'MESSAGE')
- Push to finalize changes/send to Github ('git push origin master')

[workflow-of-version-control-eacfb967-d491-4570-8f7f-ac200856b43c.pdf](workflow-of-version-control-eacfb967-d491-4570-8f7f-ac200856b43c.pdf)

[git-cheatsheet-PT-grey-be54dae1-6c34-476c-83d2-0b847d05f50d.pdf](git-cheatsheet-PT-grey-be54dae1-6c34-476c-83d2-0b847d05f50d.pdf)

## Branches

`git checkout -b 'YOUR_BRANCH_NAME'` — Create a branch

`git push origin YOUR_BRANCH_NAME` — Push branch to GitHub

`git checkout master` — Switch back to master

`git merge 'YOUR_BRANCH_NAME'` — Merge a branch

`git branch -a` — view all branches

### Add and Commit Changes simaltaneously

    git commit -am 'Commit Message'

[workflow-of-version-control.pdf](workflow-of-version-control-eacfb967-d491-4570-8f7f-ac200856b43c.pdf)

[git-cheatsheet-PT-grey.pdf](git-cheatsheet-PT-grey-be54dae1-6c34-476c-83d2-0b847d05f50d.pdf)