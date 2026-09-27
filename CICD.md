# 介绍

> CI：持续集成
> 
> CD：持续交付
> 
> git：提供代码管理能力
> 
> jenkins：提供持续集成持续交付能力
> 
> docker：提供代码运行环境

**git、github、gitlab的区别**

> git是分布式版本控制系统
> 
> github是在线的基于git的代码托管服务，github同时提供付费账号和免费账号，只有付费账号可以创建私有仓库
> 
> gitlab解决了上面的问题，可以免费创建私人仓库，同时也提供自托管版本（可以部署在你自己的服务器上），集成了完整的 **CI/CD**

# gitlab



# jenkins

**jenkins位置**

<img title="" src="./pic/cicd/屏幕截图 2026-08-08 141629.png" alt="">

**jenkins+maven+git自动化部署**

> 由于gitlab很重，所以使用GitHub代替

<img title="" src="./pic/cicd/1545491-20251106132553903-280427830.png" alt="">

> * 首先，程序员将本地代码，`git push` 到远程 GitLab 服务器。
> * 然后，Jenkins `git pull` 到 Jenkins 服务器，并用 maven 帮我们打成 jar 包。
> * 最后，Jenkins 将打好的 jar 包通过 SSH Publisher 发布到测试服务器。


