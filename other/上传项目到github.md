# 上传项目到github

* 登录github，通过New repository创建一个新的仓库
* 进入你的本地项目目录
* 通过git init将此目录设为git可管理仓库，之后会多出一个隐藏的目录.git
* 通过git status来查看当前项目的状态
* 新建一个.gitgnore文件，将项目中要忽略的文件/文件夹添加到里面
* 通过git add .将所有文件添加到仓库中
* 通过git commit -m "注释"提交所有被add的文件
* 上传项目需要SSH KEY，通过ssh-keygen -t rsa -C "你的邮箱地址"创建，之后一路回车后可以在c:/username/.ssh/id_rsa.pub中找到SSH KEY
* 返回github，打开Settings，打开SSH and GPG keys，点击New SSH key添加SSH key，Title随便填，Key则为id_rsa.pub文件中的内容
* 通过git remote add origin [仓库地址]将项目与仓库关联
* 通过git push -u origin master将项目上传到仓库