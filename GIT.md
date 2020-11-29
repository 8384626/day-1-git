# GIT

#### 特点

1. 直接记录快照,而非差异比较
2. 近乎所有操作都是本地执行
3. 时刻保持数据完整性
4. 多数操作仅添加数据
5. 文件的三种状态

#### git初始化配置

````
git config --global user.name "lastcode"
git config --global user.email ilastcode@163.com
git config --global --unset user.name   //删除
git config --global --unset user.email  //删除
````

#### 区域

- 工作区(沙箱环境)
- 暂存区
- 版本库

#### 对象

- Git对象
- 树对象
- 提交对象

#### 底层命令

- clear  // 清除屏幕
- echo 'xxxx'  //打印信息类似console
- ll  //当前目录下的 `子文件&子孙文件`平铺
- find  //将对应目录下的`子文件&子孙文件`平铺
-  find {目录文件} -test -f  //将对应目录下的`文件`平铺
- rm // 删除文件
- mv //源文件重命名
- cat // 查看对应文件
- vim
  - 按`i`进入插入模式 进行文件编辑
  - 按`es`退出插入模式
  - :wq保存退出
  - q!强制退出 不保存
  - :set nu显示行号

#### git对象(存储内容)

- echo "test content" | git hash-object -w --stdin  (不用记)

  -w选项 指示hash-object 命令存储数据对象 若不指定此选项,则该命令仅返回对应的键值

  --stdin(standard input ) 选项则指示该命令从标准输入读取内容 若不指定此选项,则必须在命令尾部给出代存储文件的路径

  git hash-object -w文件路径

  存文件

  git hash-object 文件路径

  返回对应的文件键值

  git cat-file -p {对应文件名}

#### 树对象

- git ls-files -s //查看树结构
- git update-index --add --cacheinfo 100644 {哈希值}
  - --add首次加入需要加
- git write-tree //将暂存区进行快照

#### 提交对象

- echo "first commit" | git commit-tree {哈希值} -p {第一次哈希值 }

#### 查看缓存区

git ls-files -s

# 高层命令

- git init //初始化仓库
- git add (地址)  //将工作区放到暂存区
- git commit -m ''(注释内容)" //将暂存区提交到版本库

#### 基本流程 :

创建工作目录 对工作目录进行修改

 == > git add . /

​				git hash-object -w 文件名 (修改了多少个工作目录中的文件,此命令就要被执行多少次)

​				git update-index 

​				...

==> git commit -m "注释内容"

​		git write-tree

​		git commit-tree

#### git status 查看文件状态

已修改 

已暂存 git add . (跟踪新文件)

已提交git commit -m "注释内容"

  (已跟踪的三种状态)

#### 查看已暂存和未暂存的更新

- git diff  //查看未暂存的更新
- git diff --cached //已暂存准备下次提交
- git commit -a -m "last code v4"  // 跳过使用暂存区

#### 查看历史记录

- git log  //按q退出
- git reflog : 只要是HEAD有变化 那么git reflog机会记录下来
- git log --pretty=oneline //输出日志以行打印
- git log --oneline //简化git log --pretty=oneline的输出
- git rm 文件名 删除工作目录中对应的文件,在将修改添加到暂存区
- git mv 原文件 新文件 将工作目录中的文件进行重命名,在将修改添加到暂存区

## 分支基础

**什么是分支:指向最新提交对象的指针**

创建分支:`git branch 名称`

切换分支:`git checkout 名称`

显示分支列表:`git branch`

删除分支:`git branch -D 名字`

查看完整的分支图:`git log --oneline --decorate --graph --all`

查看分支:git branch -v

快进合并:git merge 被合并的分支名字

#### 配別名

git config --global alias.wzry "log --oneline --decorate --graph --all"

## 数据的存储

- git stash 
- git stash list //查看存储
- git stash apply stash@{0}  // 如果不指定一个储存,Git认为会指定的是最近的储存
- git stash drop 加上将要移除的储藏名字来移除他
- git stash pop 来应用储藏然后立即从栈上面扔掉他

## 后悔药

工作区

​		如何撤回自己在工作目录中的修改:git restore filename 

暂存区

​		如何撤回自己的暂存: git restore --staged filename

版本库

​		如何撤回自己的提交,

1. 注释写错了: git commit --amend
2. 撤销此次提交:git reset --soft HEAD^

#### reset 三部曲

**第一部**

- git reset --soft HEAD~ (git commit --amend)

  只动HEAD(带着分支一起移动)

**第二部**

- git rest  --mixed HEAD~ 

  动HEAD

  动了暂存区

**第三部**

- git reset --hard HEAD~

  动HEAD

  动了暂存区 

  动工作区

#### checkout

git checkout 分支名 && git reset --hart commithash

**区别**

1. checkout 只动HEAD
2. --hart动HEAD而且带着分支一起走
3. checkout 对工作目录是安全的 --hard是强制覆盖工作目录

**checkout+路径**

- git checkout cmmithash filename  //重置暂存区 重置工作目录 
- git checkout --filename  //重置工作目录

### 打tag

- git tag //列出标签
- git  tag v1.0 //打tag
- git tag -d v1.0 //删除1.0的tag
- git checkout v1.0 //回退到tag 引起头部分离
-  git checkout -b "v1.0" //创建新分支

## 远程操作

**远程分支**

**远程跟踪分支**

**本地分支**



- git remote 别名 仓库地址
- git remote -v显示远程操作使用的git别用与其对应的URL
- git push 别名 head名 
- git fetch 远程仓库别名 ==> git merge 别名/master
- git branch -u 别名/分支名字 =>  git pull  //直接拉下来  =>git push 上传
- git branch -vv  //查看是否已跟踪
- git remote add 新别名  git地址            //修改别名



一个本地分支怎么去跟踪一个远程分支:

1. 当克隆的时候 会自动生成master本地分支(已经跟踪了对应的远程跟踪分支)

2. 在新建其他分支时 可以指定想要跟踪的远程跟踪分支

   ​		git checkout -b 本地分支名 远程跟踪分支名

   ​		git checkout --track 远程跟踪分支名

3. 将一个已经存在的本地分支 改成一个跟踪分支

     		git branch -u  别名/分支名字

#### 流程 

1. 项目经理初始化远程仓库

    一定要初始化一个空的仓库,在github上操作

2. 项目经历创建本地仓库

   git remote 别名 仓库地址(https)

   git init =>将源码复制进来

   修改用户名及邮箱

   git add .

   git commit -m

3. 项目经理推送本地仓库到远程仓库

   清理windows凭据

   git push 别名 分支 (输入用户名 密码 推完以后会生成远程跟踪分支)

4. 项目邀请成员及接受邀请

   在github上操作

5. 成员克隆远程仓库

   git clone 仓库地址(在本地生成.git文件 默认远程仓库配了别名orgin)

6. 成员作出贡献

   修改源码文件

   git add .

   git conmmit -m

   git push 别名 分支(输入用户名 密码 推完以后会生成远程跟踪分支)

7. 项目经理更新修改

   git fetch 本机分支名字

   git merge

#### 使用频率最高的五个命令

1. git status
2. git add .
3. git commit -m ""
4. git pull
5. git pull