# AzureDevops

# 1. Init
```
$ git init
Initialized empty Git repository in /mnt/myapp/.git/
```

# 2. Create Files
```
$ vi FileA.txt

$ ls
FileA.txt
```

# 3. Git Add/Commit
```
$ git add FileA.txt 

$ git commit -m "2022-06-13 16:18"
[master (root-commit) 23a05e3] 2022-06-13 16:18
 1 file changed, 1 insertion(+)
 create mode 100644 FileA.txt
```
 
# 4. Git Push w/o Branch Policy (not requiring Pull Requests)
```
$ git remote add origin https://xxx@dev.azure.com/xxx/myapp/_git/myapp

$ git push -u origin master
Password for 'https://xxx@dev.azure.com': 
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 239 bytes | 239.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Analyzing objects... (3/3) (5 ms)
remote: Storing packfile... done (95 ms)
remote: Storing index... done (54 ms)
To https://dev.azure.com/.../myapp/_git/myapp
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

# 5. Push w/o Branch Policy (not requiring Pull Requests)
# 5-1. Git Branch
```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

$ git branch developing

$ git checkout developing
Switched to branch 'developing'

$ ls
FileA.txt

$ cat FileA.txt 
2022-06-13 16:18

$ vi FileA.txt 
```
# 5-2. Git Add/Commit
```
$ git add FileA.txt 

$ git commit -m "2022-06-13 20:12(developing)"
[developing af317df] 2022-06-13 20:12(developing)
 1 file changed, 1 insertion(+)

$ git push -u origin --all
 
$ cat FileA.txt 
2022-06-13 16:18
2022-06-13 20:12 (developing)
```

# 6. Push w/ Branch Policy (require Pull Requests)
# 6-1. Git Branch, Add/Commit
```
$ git chekcout developing

$ cat FileA.txt 
2022-06-13 16:18
2022-06-13 20:12 (developing)
2022-06-18 10:58 (developing)

$ git add FileA.txt 

$ git commit -m "2022-06-18 10:58(developing)"
[developing 19d81dd] 2022-06-18 10:58(developing)
 1 file changed, 1 insertion(+), 1 deletion(-)
 
$ git push -u origin developing
```

# 6-2. You need a Pull Request to merge into Master Branch
After doing git push in #6, you can do "pull request" in Azure Devops so that this commit can be merged into Master Branch.

# 6-3. Git Merge on local repositry
You can find the Master Branch is behind the developing Branch even after git push.
```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

$ cat FileA.txt 
2022-06-13 16:18
2022-06-13 20:12 (developing)
```

Then, You should merge as followings:
```
$ git merge developing
Updating 943aa47..19d81dd
Fast-forward
 FileA.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

$ cat FileA.txt 
2022-06-13 16:18
2022-06-13 20:12 (developing)
2022-06-18 10:58 (developing)
```

