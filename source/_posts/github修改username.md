---
title: github修改username
date: 2018-06-13 16:04:59
tags: 
  - github
---

#### 1.首先在github官网上修改
  Settings -> Account -> Change username
  修改 username 会有一些想不到的副作用
  ![warning](https://raw.githubusercontent.com/Hetty0/Hetty0.github.io/master/uploads/warning.png)

  <!-- more -->
#### 2.修改本地项目git仓库地址
  ```
    列出现有地址：git remote -v
    设置远程地址：git remote set-url origin 远程仓库地址
    检查设置是否成功：git remote –v，查看列出的地址是否正确
    设置项目的user.name跟github上的username一致（本人配置了github和gitlab，所以只设置此项目的local name）
      git config --local user.name "github上的username"

    设置好了之后使用git push测试一下，发现需要输入Username和password，输入正确后，就可以正确使用了！✌️✌️✌️
  ```

#### 3.修改hexo博客仓库地址
  好像可以自动重定向，估计是刚改的名字，还没有更新。
  在_config.yml中修改
  ```
    deploy:
      repo: 新的仓库地址（不要忘了将博客仓库的名字改成对应的新名字）
  ```
  注：
  修改git仓库名称
    进入需要修改名字的仓库，点击settings，首项Options中就可以看到Rename。

#### 4.域名DNS修改
  我使用的是godaddy上的域名，修改DNS配置
  类型：CNAME
  名称：www
   值：yourusername.github.io

  这样，使用你的域名和http://yourusername.github.io都可以访问你的博客网站啦！