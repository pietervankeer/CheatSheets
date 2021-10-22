# Git Bash

| Task                                     | Command                       |
| :--------------------------------------- | :---------------------------- |
| `git status`                             | zijn er changes?              |
| `git fetch`                              | changes fetchen van de server |
| `git pull`                               | changes binnenhalen           |
| `git add . / --all / opdracht02/doc.txt` | change(s) stagen              |
| `git commit -m "je commit boodschap"`    | commit maken                  |
| `git push`                               | changes pushen                |

## Commit workflow

```bash
git pull
git status
git add --all
git commit -m "message"
git push
```

## Commit to new branch

```bash
git pull
git branch -av # show branches that we have
git checkout -b new_branch origin/existing_branch # make new local branch, based on existing branch
git push -u origin new_branch # push new local branch to online repository
```
