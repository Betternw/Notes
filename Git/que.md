1. git init——创建
2. git state——查看当前状态
3. git diff——看到这次和上次相比修改了哪些内容，将修改的部分标注出来
4. git add——把要添加的文件放入暂存区中
5. git commit——将暂存区的文件提及到代码区中
6. git clone——从远程仓库克隆到本地
7. git branch——查看当前分支是什么
8. git checkout——切换分支
9. 冲突解决：
    * git fetch origin //先下拉和并
    * git merge origin/master
    * //手动在本地文件中修改冲突，并去掉<<<<<<< ======= >>>>>>>标志
    * git add .  //上推
    * git commit -m "B change"
    * git push origin
10. clone fetch pull的区别
    * clone：从远程服务器克隆一个一模一样的版本库到本地,复制的是整个版本库，是一个本地从无到有的过
    * pull：相当于是从远程获取最新版本并merge（合并）到本地 git pull = git fetch + git merge，git fetch更安全一些
    * fetch：相当于是从远程获取最新版本到本地，不会自动merge
