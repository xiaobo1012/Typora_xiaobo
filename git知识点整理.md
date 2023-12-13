## git常用命令

> **1、设置用户签名**：`git config --global user.name [用户名]`
>
> **2、设置用户邮箱签名：**`git config --global user.email 邮箱`
>
> - - > global表示全局配置，只需要再电脑上配置一次
>     >
>     > git config --global --list 可查看配置好的全局配置
>
> **3、初始化仓库：**`git init`
>
> **4、添加暂存区：**`git add [文件名] / .   `       .。 表示提交当前文件夹下所有的文件
>
> - 删除暂存区中某个文件：`git rm --cached [文件名]`
> - 清空暂存区：``git reset``
>
> **5、提交到本地库：** `git commit  -m "这里写简介"  [文件名]`
>
> **6、设置远程仓库：**`git remote add [仓库别名] [仓库地址]`   ---- **一个文件只用设置一次**
>
> - > **git remote -v **  可以查看有几个远程仓库
>   >
>   > **git remote  rename [old_name] [new_name]**   可修改别名
>   >
>   > **git remote romove [别名]**   删除仓库
>
> 
>
> **7、将代码提交到远程仓库：**`git push  [仓库别名] 【master/其他仓库分支】`
>
> **8、拉取远程仓库到本地：**`git pull [仓库别名] 【master/其他仓库分支】`---------*养成提交后才拉取一次的习惯*



- **查看用户签名和邮箱**：`cat ~/.gitconfig`

- **查看本地库状态：**`git status`

- **克隆远程仓库：**`git clone 【仓库地址】`

  - > 克隆下来的仓库，默认别名就是**origin**



***注：解决git SSL certificate problem: unable to get local issuer certificate保存的问题***

> 这个问题是由于没有配置信任的服务器HTTPS验证。默认，[cURL](https://so.csdn.net/so/search?q=cURL&spm=1001.2101.3001.7020)被设为不信任任何CAs，就是说，它不信任任何服务器验证。
>
> git config --global http.sslVerify false



## git分支操作

https://www.bilibili.com/video/BV1ai4y1M7FN?p=5&spm_id_from=pageDriver&vd_source=0de1ee80a2ec8e9ede67cbdfd17acc85

> 当多个人对同一个项目进行编写的时候，为了将自己的任务和主线程分开，就需要采用的到分支操作
>
> 再组员进行编写代码的时候只能使用其他分支进行提交
>
> master分支只有组长在合并代码的时候使用

- **查看分支：**`git branch -v`

- **创建分支：**`git branch [分支名称]`

- **删除分支：**

  - > ```git
    > // 删除本地分支
    > git branch -d localBranchName
    > 
    > // 删除远程分支
    > git push origin --delete remoteBranchName
    > ```

- **切换分支：**`git checkout [分支名称]`

  - > 这样就可以随意的去修改文件中的数据了，因为我们可以随时去删除这个分支（只要在分支与主线合并之前）
    >
    > **git checkout -b 【分支名称】**------- 创建分支并且切换分支
    >
    > 当切换分支的时候，文件中的代码也会随之切换【本地库保存了分支文件】

- **合并分支：**`git merge 【分支名称】`

  - > 1. 切换到分支上，并且git pull拉取最新分支代码，确认代码后再确认是否需要合并到master分支
    > 2. 切换到master分支
    > 3. 开始合并分支git merge







## 【注】：冲突合并

> 当主线分支与创建的其他分支同时都被修改后，无法进行自动合并分支（需要手动修改）
>
> 文件中会标出两个分支发生冲突的地方（把不需要的代码手动删除就行了）
>
> 当手动删除代码后，还需要将代码重新上传本地库（这时候就不需要带上文件名称了）【只会修改master分支的文件】

![img](https://cdn.jsdelivr.net/gh/xiaobo1012/imgPicGo/imgs/202311221634041.png)



## git远程仓库使用（团队协作）

- 设置远程仓库**【git remote （别名） （仓库地址）】**

- 推送本地库到远程库**【git push （别名）(本地库的哪个分支)】**

- 拉取远程库到本地库**[git pull (远程库别名)  （远程库分支）]**

- 克隆远程库到本地**【git clone （仓库地址）】**

  - 克隆的文件可以直接git push 和 git pull  ，不用输入仓库名和分支名

- ***团队开发多个分支时***

  - > 1. 当队员提交分支后，队长可以**直接切换**到对应分支，而不用去git pull拉取分支下来
    > 2. **在切换到分支后，再使用git pull 拉取分支的最新代码**




- **团队开发**

  - > 1、想要团队开发，则需要在远程库中将所以成员都添加进去（这样才能进行代码的push操作）
    >
    > 2、邀请后将邀请函（也就是一个链接地址），发送被邀请的人
    >
    > 3、被邀请的人打开连接，接受邀请
    >
    > 4、之后被邀请的人的代码就可推送到这个远程仓库了

- **[SSH免密登录](https://www.bilibili.com/video/BV1vy4y1s7k6/?p=26&spm_id_from=pageDriver&vd_source=66441f23d5d3bbb2f332e315c9c9f6f5)**

  - > 1. 在git BUSH中输入`ssh-keygen.exe`
    > 2. 在对应的电脑用户文件夹中找到 C盘—>当前账户—> ssh —> id_rsa.pub  将里面的代码全选复制后去github上保存下来



## git merge 和 git rebase的区别

> **相同点**
>
> - 都是将任意一个分支合并到当前分支

> 不同点
>
> - git merge 在合并时会自动提交到本地库，如果合并时遇见冲突则需要修改后重新commit（这里需要自己手动 add 和 commit  【注】：在commit的时候不用带上文件名）
> - 





## git版本控制

- **查看历史日志：**`git reflog   /   git log`

- **版本迭代：**当版本提交到本地库后被更改，需要重新再次上传本地库，这时就会出现两个版本的代码（使用进行版本穿梭了）
- **版本穿梭：**`git reset --hard  （版本号）`  ***注：版本号在查看日志的时候可以看到