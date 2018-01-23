### 一、为什么要做持续集成
1. 解放了重复性劳动

自动化部署工作可以解放了集成、测试、部署等重复性劳动，而且机器集成的频率明显可以比手工的高很多。

2. 更快地修复问题

由于持续集成更早的获取变更，更早的进入测试，也就能更早的发现问题，解决问题的成本显著下降。

3. 更快地交付成果

及早集成、及早测试减少了缺陷遗留到部署环节的机会。在某些情况下，更早地查找错误还会减少解决错误所需的工作量。

如果集成服务器对代码进行构建过程中发现错误，可以及时发送邮件或者短信提供给开发人员进行修复。

如果集成服务器在部署环节发现当前版本有问题不可用，集成服务器会将部署回退到上一个版本。这样服务器上始终都会有一个可用的版本。

4. 减少手工的错误

人与机器的一个最大的区别是，在重复性动作上，人容易犯错，而机器犯错的几率几乎为零。所以，当我们搭建完成集成服务器后，以后的事就交给集成服务器来打理吧。

5. 减少了等待时间

持续集成缩短了从开发、集成、测试、部署各个环节的时间，从而也就缩短了中间可以出现的等待时间。持续集成，意味着开发、集成、测试、部署也得以持续。

6. 更高的产品质量

集成服务器往往提供 Code review、代码质量检测等功能。对代码不规范或者有错误的地方会进行标识，也可以设置邮件、短信等进行告警。而开发人员通过 Code review 也可以持续提高编程的能力。





### 二、环境总览

1. 环境规划

![](http://p20idpaa6.bkt.clouddn.com/2018-1-23/2018-01-23-1.png)

2. CI/CD时序图

![](http://p20idpaa6.bkt.clouddn.com/2018-1-23/2018-01-23-2.png)


### 三、CI说明

本次持续集成采用Rancher + Kuberntes + Docekr + Jenkins + Sonarqube + Nexus + Gogs 等工具来实现。

本次集成核心围绕Jenkins，采取定时任务模式触发自动化打包（不采用钩子，以避免频繁提交代码，自动构建频繁触发，产生大量不必要的资源损耗），届时，打包流程下：

 - 研发人员提交代码到Gogs代码仓库
 - Jenkins自动打包任务定时触发（在需要的场景下，可由运维或研发人员登陆Jenkins手动执行打包任务）
 - Jenkins自动从Gogs仓库拉取应用代码
 - Jenkins自动触发Maven打包应用，生产Docker镜像
 - Jenkins自动触发Sonarqube执行代码检视
 - Jenkins自动移除旧应用的Docker镜像，上传新的Docker镜像到Nexus构建的Docker镜像仓库
 - Jenkins自动调用Kuberntes集群，从Docker镜像仓库拉取新的应用镜像，并自动部署









# 环境展示

<div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Rancher：全栈化容器管理平台
        <a href="http://58.250.168.239:8080" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Rancher</a>
    </div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Kuberntes：仪表板入口
        <a href="http://58.250.168.239:8080/env/1a5933/kubernetes/dashboard" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Kuberntes</a>
    </div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Nexus：Maven、Docker仓库
        <a href="http://58.250.168.239:8181/" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Nexus</a>
    </div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Gogs：代码仓库
        <a href="http://58.250.168.239:1008/" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Gogs</a>
    </div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Jenkins：自动打包部署平台
        <a href="http://58.250.168.239:8183" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Jenkins</a>
    </div>
    <div style="display: inline-block;margin:10px;border-width:2px;height:100px;width:200px;">
        Sonarqube：代码质量检视平台
        <a href="http://58.250.168.239:9000" style="display: block; 
            height: 34px; width: 160px;line-height: 2; text-align: center; 
            background: #00558b; color: #FFFFFF; font-size: 14px; font-weight: bold; 
            text-decoration: none; padding-top: 3px;" target="_blank">Sonarqube</a>
    </div>
</div>
