---
title: Git初学
tags: git
abbrlink: 8879abe1
---



# git操作

![image-20231026124027011](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231026124027011.png)

## 1.git命令操作

### 1.1.配置身份

```shell
git config --global user.name gaomu

git config --global user.email jack964270261@gmail.com
```

### 1.2.初始化创建git仓库

```shell
git init
# 处于主分支状态
echo "版本1" > lao.md

查看git状态
git status
```

### 1.3.其他git操作

```shell
# Untracked 未追踪状态
# tracked 已追踪
# 添加到暂存区
git add lao.md

```

```shell
D:\tmp\gitobj>git status
On branch master # 位于主分支

No commits yet # 未提交状态
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   lao.md
```

```shell
# 提交到本地仓库
git commit 
# 进入到vim编辑器 编辑版本描述信息，保存即提交
# 快速commit指令
D:\tmp\gitobj>git commit -m "版本2和版本3"
[master 5562bff] 版本2和版本3
 1 file changed, 6 insertions(+), 1 deletion(-)

//-a 参数设置修改文件后不需要执行 git add 命令，直接来提交
//$ git commit -a
 
```

```shell
# 查看版本信息
D:\tmp\gitobj>git log
commit 5562bff94b91be8e33839c92159ed592d765f568 (HEAD -> master)
Author: gaomu <jack964270261@gmail.com>
Date:   Thu Oct 26 12:54:42 2023 +0800

    版本2和版本3

commit f21ae17c9b8ba002550347332b9b5a6f98eaec1a
Author: gaomu <jack964270261@gmail.com>
Date:   Thu Oct 26 12:47:39 2023 +0800

    版本1
```

```shell
# 忽略不需要上传的文件,创建  .gitignore 文件
# 在文件中添加不需要推送的文件路径
# echo "1.png" > .gitignore

```

![image-20231026130347781](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231026130347781.png)

### 1.4.创建分支

![image-20231030092837271](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030092837271.png)

```shell
# 创建分支
git branch bad-project
# 查看分支
git branch
D:\tmp\gitobj>git branch
  bad-project
* master

# 切换分支
git checkout bad-project
D:\tmp\gitobj>git checkout bad-project
Switched to branch 'bad-project'

git commit -a -m "删库"
# 删除分支
D:\tmp\gitobj>git branch -d bad-project
Deleted branch bad-project (was 5562bff).

# 创建临时分支,并切换
git checkout -b temp
D:\tmp\gitobj>git checkout -b temp
Switched to a new branch 'temp'

git checkout master
# 合并分支
git merge temp# 当前为主分支
D:\tmp\gitobj>git merge temp
Updating 724c228..9ad9d5b
Fast-forward
 lao.md | 2 ++
 1 file changed, 2 insertions(+)
```

### 1.5.远程git 拉取

1.创建一个新仓库

![image-20231030101919216](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030101919216.png)

2.设置仓库名

![image-20231030101958720](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030101958720.png)

3.点击创建仓库

4.创建新文件

![image-20231030102102984](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030102102984.png)

5.设置文件名和文件内容

![image-20231030102154126](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030102154126.png)

6.点击commit changes 相当于执行了 add 和 commit命令

![image-20231030102235680](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030102235680.png)

7.将远程仓库，拷贝cline链接，clone到本地（clone包含.git文件夹，download zip不包含）

![image-20231030102605943](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030102605943.png)

8.克隆到本地

![image-20231030102713665](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030102713665.png)

### 1.6.远程git 推送

1.修改内容

![image-20231030103302257](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030103302257.png)

2.commit到本地仓库

![image-20231030103349399](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030103349399.png)

3.查看本地仓库与远程仓库的联系

![image-20231030103601442](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030103601442.png)

origin:远程仓库的名字 

4.推送到远程仓库(验证身份)

![image-20231030103807332](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030103807332.png)

5.git connect 连接失败需要配置 窗口git代理

![image-20231030104917653](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030104917653.png)

```shell
#http代理
git config --global http.proxy 'http://127.0.0.1:1080'
#https代理
git config --global https.proxy 'http://127.0.0.1:1080'
#http代理
git config --global http.proxy 'socks5://127.0.0.1:1080'
#https代理
git config --global https.proxy 'socks5://127.0.0.1:1080'
 
#取消http代理
git config --global --unset http.proxy
#取消https代理
git config --global --unset https.proxy
```

6.推送成功。

![image-20231030105112358](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030105112358.png)

### 1.7.版本对比,更新本地仓库版本

```shell
# 从远程仓库拉去到本地
git fetch
# 比较区别 远程仓库名/分支名
git diff origin/main
# 红色表示远程版本新增内容
# 确定没问题之后则 git pull 从远程仓库拉去到工作区
```

![image-20231030105744322](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030105744322.png)

### 1.8.VSCODE git

1.git init 初始化仓库

![image-20231030110326407](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030110326407.png)

2.U 代表未追踪状(untracked)

![image-20231030110442260](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030110442260.png)

3.点击 +号， git add, （取消则点击 -号）

![image-20231030110536791](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030110536791.png)

4.A表示已添加状态

![image-20231030110633340](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030110633340.png)

5.输入版本信息，点击提交则commit到本地仓库了

![image-20231030110833545](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030110833545.png)

6.新建分支

![image-20231030111041551](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111041551.png)

![image-20231030111116973](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111116973.png)

7.修改新分支

![image-20231030111213573](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111213573.png)



8.点击文件名进行对比，相当于diff

![image-20231030111345578](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111345578.png)

9.弱确定没问题则点击加号+，并且提交信息即可。

10.左下角显示当前分支名，点击可切换当前分支。

![image-20231030111238253](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111238253.png)

![image-20231030111707560](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111707560.png)

11.合并分支

![image-20231030111801275](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111801275.png)

12.合并

![image-20231030111922947](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030111922947.png)

13.push到远程仓库,点击发布 Branch，同时授权登录

![image-20231030112053425](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030112053425.png)

![image-20231030112034969](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030112034969.png)

14.发布成功。

![image-20231030112137465](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030112137465.png)

![image-20231030112226910](C:\Users\ChenLei\Desktop\周报\1029第21周报陈磊\image-20231030112226910.png)