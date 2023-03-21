## Git config


Klipper
```
root@spad-1168:/usr/share/klipper# cat .git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[submodule]
        active = .
[remote "origin"]
        url = git@172.23.88.26:lidonghong/klipper-new.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```
