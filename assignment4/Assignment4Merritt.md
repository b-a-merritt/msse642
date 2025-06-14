# Git Branching

## Creating a new Branch

```Bash
git checkout -b <feature branch name>
```

![git branch creation](./images/git-branch.png)

## Viewing Branches

```Bash
git branch # see all local branches
git branch -r # see the remote branches
```

![git branch viewing](./images/branches.png)

## Commiting Changes to a Branch

```Bash
git add <File names...> # add files to staging area
git commit -m "<commit message>" # commit the changes 
```

![commit changes](./images/commit.png)

## Creating and Merging a Pull Request

```Bash
git push origin <branch name>
```

![create and merge pull request](./images/pull-request.png)