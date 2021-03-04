# 使用picGo + github搭建图床



## picGo

一个基于[electron-vue](https://simulatedgreg.gitbooks.io/electron-vue/content/cn/)开发的图床工具。支持插件系统。



## github配置

* 新建github仓库
* 创建访问令牌：访问`Settings > Developer settings > Personal access tokens`，点击`Generate new token`生成token，勾选repo后保存即可



## 配置picGo

包含仓库名、分支名、Token（github配置的访问令牌）等

- Token：即为github配置的访问令牌
- 自定义域名：默认的访问域名为`https://raw.githubusercontent.com`，可以配置为`https://cdn.jsdelivr.net/gh`，通过国内的CDN加速图床

![picGo配置](https://cdn.jsdelivr.net/gh/jackyli1991/Image-Hosting/img/picgo/picgo.png)

