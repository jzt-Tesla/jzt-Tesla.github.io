---
layout: post
title: 一目了然-git命令三种方式实现Gitlab和Github同时登录
subtitle: git登陆
date: 2021-06-07
author: 公子江小花
header-img: img/post_bg_essay.jpg
catalog: true
tags:
  - Git
---



## 前言介绍：
很多时候，我们都需要去在电脑上去同时使用gitlab和github，但是这样会很容易造成冲突，看了很多文章去解决这个问题，但是我发现大家的方式各有不同，导致看博客的人感觉很困惑，于是我尝试一步一步引导大家用三种方式去真正的实现在一台PC上去同时使用gitlab和github
![pic1](https://img-blog.csdnimg.cn/20190313200918185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)
## 基本要点
- 于我而言，提交公司的代码比较多，所以我配置global为公司（gitlab）使用时候提交的具体信息，配置local为个人（github）使用的具体信息
- global：全局配置（用户名和邮箱） 
- local：当前项目的配置（用户名和邮箱），如果你没有配置，提交代码时候个人信息是global配置的信息。需要注意的是配置local的时候，需要cd指令打开具体项目文件夹，再去配置local

**需要说明的是这两个配置并没有什么大的用处，仅仅只是为了在提交代码的时候，标明到底是哪位同学上传的代码罢了**

-  类似于如下图：
  ![pic2](https://img-blog.csdnimg.cn/20190307192212960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)


```
//查看全局配置(global)
git config --global --list
```
```

//设置global的基本信息，用户名和邮箱
git config --global user.name AllenJ
git config --global user.email jiang@gamil.com

```
```

//查看当前项目的配置（local）
git config --local --list

```
```

//设置local的基本信息，用户名和邮箱
git config --global user.name jzt
git config --global user.email jzt@qq.com

```
## 三种方式

- 由于我设置提交到gitlab是全局global，这个很简单；所以我只演示如何提交到github。

### 方式一：
- 通过Https的方式，不需要生成SSH密钥

![pic3](https://img-blog.csdnimg.cn/20190307192318852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)
```
//配置global
git config --global user.name AllenJ
git config --global user.email jiang@gamil.com

//打开具体需要克隆的文件，克隆项目到该文件夹内
cd C/MyBlog
git clone https://github.com/jzt-Tesla/FileRecyclerViewDemo.git

//配置该项目的local
git config --global user.name jzt
git config --global user.email jzt@qq.com

//自己手动测试一下，随便改一下里面的文件内容

//git命令，提交项目到github
cd C/MyBlog/FileRecyclerViewDemo (克隆的具体项目)
git status
git add 具体改变的文件
git commit -m'注释'
git push

```
- git指令提交如下图：

![pic4](https://img-blog.csdnimg.cn/20190307192417815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)


注意：提交的时候，会弹出一个验证，输入github账号和密码就OK了 ~ 

![pic5](https://img-blog.csdnimg.cn/20190307192527958.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)



### 方式二&方式三：
- 方式二：通过SSH进行验证，但是不配置config文件
- 方式三：通过SSH进行验证，需要配置config文件

![pic6](https://img-blog.csdnimg.cn/20190307192550471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

### 相同步骤：

1.  首先分别生成gitlab和github网站的密钥文件，一共两对四个，公钥和私钥。
```
//gitlab密钥
$ ssh-keygen -t rsa -C "jiang@gmail"
//github密钥
$ ssh-keygen -t rsa -C "jzt@qq.com" -f ~/.ssh/github_id-rsa

```
- -t 密钥类型，默认是 rsa加密
- -C 注释文字，比如用户邮箱 
- -f 密钥文件位置及文件名
3. 将生成的公钥（.pub文件）粘贴到github/gitlab的SSH地址里面

![pic7](https://img-blog.csdnimg.cn/20190307192617188.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

3. 这时候你需要测试一下是否能够连的通gitlab/github
```
ssh -T git@你的地址

//例如如下

ssh -T git@192.168.3.110

ssh -T git@github.com

```
-当测试的时候会发生图下所示，测试生成id_rsa密钥的地址是正常的，而生成github_id_rsa的会 <font color =red>permission denied（publickey）</font> 
意味着无法连通github
![pic8](https://img-blog.csdnimg.cn/20190307193307680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

### 不同步骤：
#### 方式二：
 - 造成上述情况的原因是，默认是使用id_rsa密钥的，如果要使用新密钥，需要将新密钥添加到agent里面去，而且遗憾的是这是临时的，下次使用同样需要再次添加。

 ```
 ssh-agent bash
 ssh-add ~/.ssh/github_id_rsa
 ```
 - 这个时候就可以测试github了，发现是完全OK的。
    ![pic9](https://img-blog.csdnimg.cn/20190307193503821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

- 剩下的步骤和Https方式一致，无非是设置global、local、克隆，推送，提交项目

#### 方式三：
- 方式二已经说明了原因了，就是密钥不一致，那么我们在这里采用的方法是搞一个配置文件config， 其中gitlab地址，一般都是自己公司目所在的ip地址。网上很多人用gitlab.com，如果你保存项目在gitlab，那你就填写这个吧。

```
//生成config 
ssh touch ~/.ssh/config

```
- 直接打开config文件，进行如下配置：
```
Host gitlab

HostName 192.168.3.110

IdentityFile ~/.ssh/id_rsa

Host github

HostName github.com

IdentityFile ~/.ssh/github_id_rsa
```

- 注意的是，大家配置完成后进行测试，就不是像方法二 git -T git@具体地址，而是git -T git@Host的值 ，也就是config里面具体的Host，<font color=red>因为有配置文件config，最后会指向具体的密钥，这样就OK了</font>

- 你可能会出现下图的config配置问题，我当时被这个搞了好久，参考很多人说的，这个是空格问题 ------>[管理git生成的多个ssh key
  ](https://www.jianshu.com/p/f7f4142a1556) ,但是我怎么搞都不行，最后用markdown的工具打开，删除空格和<font color =red>每行之间必须用回车不然也会出错</font>（开始用了txt文件打开，无论怎么都搞不好），每行之间是回车，但是你用txt打开却是空格，所以大家要小心了，实在不行的，也可以参考我后面的github地址，大家自行下载。
  ![pic10](https://img-blog.csdnimg.cn/20190307193610297.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)
- 最后的成功的结果图：
  ![pic11](https://img-blog.csdnimg.cn/20190307193622666.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

- 剩下的步骤和Https方式一致，无非是设置global、local、克隆，推送，提交项目

<font size =4 color =red> 值得注意的一点是：</font>
- 当我们克隆项目到本地的时候，也许会出现第一个里面的错误：
  ![pic12](https://img-blog.csdnimg.cn/20190313194939336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)

- 说我们没有权限去克隆，但是明明都已经与github服务器测试成功了，为什么还是会出现没有权限的错误呢？可以观察到： 
- 第一个clone的地址是：**git clone git@github.com:jzt-Tesla/FileRecyclerViewDemo.git**
- 第二个clone的地址是：**git clone git@github:jzt-Tesla/FileRecyclerViewDemo.git**
- 第一个地址是直接从github里面复制过来的，错误在于我们设置了**config文件**里面的<font color =red>github的Host为github，而不是github.com</font>，从而导致了第一种克隆方式无法找到github密钥的地址，所以也就没有权限了。修改成Host里面的github，使用第二张克隆就可以成功的将项目clone到本地了。


## Others
- 删除本地文件，并提交到云端
  git rm XXX.txt
  git commit -m' 注释'
  git push

- 修改已经设置过的global配置 
```
$  git config --global --replace-all user.email "输入你的邮箱" 
$  git config --global --replace-all user.name "输入你的用户名"
```

- 提交所有修改
```
// 提交未跟踪、修改和删除的文件。
    git add --all

//提交未跟踪和修改的文件，但不能进行文件的删除。
    git add .
```


- 获取当前的clone方式（Http或者Git）
```
git remote -v
```

- SHH验证，成功的话，密钥的颜色会有如下的变化：
  ![pic13](https://img-blog.csdnimg.cn/20190313194552834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)
## 错误总结：

- 如果出现错误 <font size = 4 color =red>Permission denied (publickey)</font> ，那么证明你进行SSH验证的时候，密钥是出现了错误的。

## 总结来说

- 建议大家使用第三种方式，简单一丢丢 ~


### config文件：

- <font size =4 color =red> Github</font>下载地址：！------》 [config文件下载地址](https://github.com/jzt-Tesla/FileRecyclerViewDemo)

![pic14](https://img-blog.csdnimg.cn/20190313200652893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAzMTI5NDk=,size_16,color_FFFFFF,t_70)
**----------------------------------------------------------------------------------------------------**

### 参考：
<font size = 3>[ 常用 Git 命令清单-- 阮一峰](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)</font>

<font size = 3>[ gitlab和github一起使用](https://blog.csdn.net/hxb147542579/article/details/53218348)</font>

<font size = 3>[github账号与gitlab同一电脑下不同SSH Key配置](https://blog.csdn.net/hutianyou123/article/details/77005798)</font>

<font size = 3>[git 多账号 ssh-key 管理（github和gitlab共同使用）](https://blog.csdn.net/qq_30227429/article/details/80229167)</font>

<font size = 3>[ 同时使用两个账号分别操作Github和Gitlab](https://blog.csdn.net/mycafe_/article/details/79231599)</font>

<font size = 3>[ push代码，git@github.com: Permission denied (publickey).报错](https://blog.csdn.net/pan_xi_yi/article/details/83513021)</font>

<font size = 3>[ 同一台电脑同时使用gitlab和github](https://blog.csdn.net/u014296452/article/details/79984867)</font>

<font size = 3>[ 多个sshkey对应多个不同的github账号](https://blog.csdn.net/qq_32376345/article/details/80898883)</font>

<font size = 3>[ 同一台电脑同时使用gitlab和github](https://blog.csdn.net/u014296452/article/details/79984867)</font>

<font size = 3>[ 管理Git生成多个ssh key及报错Bad configuration option解决方法-2018-10-06](https://www.jianshu.com/p/6646933798fd)</font>