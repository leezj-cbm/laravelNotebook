# Git Commands

## Dangerous


### Reset to remote repo Head

https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head

I needed to do (the solution in the accepted answer):

```
git fetch origin
git reset --hard origin/main
```

This cleans out all uncommited files
```
git clean -f
```

To see what files will be removed (without actually removing them):
```
git clean -n -f
```
