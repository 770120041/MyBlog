---
layout: post
title:  "Merge Repos into subdirectory keeing history"
categories: blah
---
How to merge repos into subdirectory of a new repo(or an existing repo)

#Steps:
## Make a new Repo
Optional: steps if you are merging to a new repo
Create a new folder, then init git and make an initial commit 

```
mkdir new_repo
git init
touch sth
git commit -m "initial commit"
```

## Fetch old repo
Add remote repo, fetch and merge

Remeber to add "--allow-unrelated-history" for merging
``` 
git add remote old_repo sshurl
git fetch old_repo
git merge old_repo/master master
```

## Copying files into new subfolder
create a new folder, copying all files to subfolder
```
mkdir oldrepo
FOR %F IN (*) DO IF NOT %F == oldrepo git mv %F oldrepo
FOR /D %D IN (*) DO IF NOT %D == oldrepo  git mv %D oldrepo
```

## Commit and all done
Commit the changes, push all done.
