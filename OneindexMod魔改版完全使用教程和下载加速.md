> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.echoteen.com/oneindex-paged-fast.html

上次二开了大佬的 oneindex 程序，持持续续有大佬在不断的关注，不断的提 bug 和功能，上班的时候不是很有时间，现在趁着假期，重构了部分代码，带来了比较实用性的功能，修改了重要的 bug

本篇文章介绍如何去使用和安装，怎么配置 redis 和 sharePoint 的 CDN 提高下载速度。

项目地址：[https://github.com/david7207/oneindexMod.git](https://www.echoteen.com/wp-content/themes/begin/go.php?url=aHR0cHM6Ly9naXRodWIuY29tL2RhdmlkNzIwNy9vbmVpbmRleE1vZC5naXQ=)

升级安装
----

如果之前用过可以直接删掉代码，重新来一遍就好了，注意保留好 config 目录下的 base.php

里面有 clientid 和密钥，可以复用！

安装（以宝塔面板为 demo）
---------------

材料：office365 A1，E3，或者 E5

没有材料的可以去 [https://office.shax.vip](https://www.echoteen.com/wp-content/themes/begin/go.php?url=aHR0cHM6Ly9vZmZpY2Uuc2hheC52aXA=) 去免费自助获取

拉取代码到服务器目录

```
git pull https://github.com/david7207/oneindexMod.git
```

去网站里新建一个站点

[![](https://img.echoteen.com/upload/2020/05/01/eab634e233b5e.png)](https://img.echoteen.com/upload/2020/05/01/eab634e233b5e.png)

配置好站点域名，php 版本（一定要是 php7.0+）

必要操作：安装 Redis 并开启 redis 服务，php 必须安装 redis 模块

[![](https://img.echoteen.com/upload/2020/05/01/2fced091c550f.png)](https://img.echoteen.com/upload/2020/05/01/2fced091c550f.png)

配置 graph Api
------------

（可以直接在第三步创建，但是建议自己配置好，懒癌患者 pass）

用分配的账号登陆 https://portal.azure.com/

[![](https://img.echoteen.com/upload/2020/05/01/4e85e6c582fd4.png)](https://img.echoteen.com/upload/2020/05/01/4e85e6c582fd4.png)

### 打开 azure Directory

应用注册去注册一个应用程序，然后去证书和密码选项申请一个密码，时效为永久

注意添加一个回调地址

https://tui.echoteen.com/api/office/serve

[![](https://img.echoteen.com/upload/2020/05/01/495c64950dcc1.png)](https://img.echoteen.com/upload/2020/05/01/495c64950dcc1.png)

然后去添加接口权限，把文件相关的权限勾选上就好了

[![](https://img.echoteen.com/upload/2020/05/01/f59736f66b0de.png)](https://img.echoteen.com/upload/2020/05/01/f59736f66b0de.png)

不一定是上面所有的权限，只需要 file 相关的权限就好

主要是应用程序级别的 files.readwrite.all

好了，graph 就到这 ok 了

配置网站
----

打开网站域名配置

[![](https://img.echoteen.com/upload/2020/05/01/dbf50d8329f9b.png)](https://img.echoteen.com/upload/2020/05/01/dbf50d8329f9b.png)

如果没有 config 和 cache 文件夹请手动创建，并赋予 777 权限

[![](https://img.echoteen.com/upload/2020/05/01/8047293b02540.png)](https://img.echoteen.com/upload/2020/05/01/8047293b02540.png)

如果你没进行第二步或者你之前没创建过应用，可以点击这个去创建然后配置好相应的权限

redis 密码，如果没有设置就不填，设置了密码就写上去

cdn 直链等会我们再说，不过可以在配置里面再次配置，先过去

配置全局 CDN
--------

直接去 cloudflare 去创建一个 cdn，配置好 cname 或者 ns 就好了。

配置 sharePoint 下载加速（可选，但是速度快！）
-----------------------------

先留意下源码目录里的 worker.js

去 cloudflare 打开 workers

[![](https://img.echoteen.com/upload/2020/05/01/72b86eb13671d.png)](https://img.echoteen.com/upload/2020/05/01/72b86eb13671d.png)

创建一个 worker

把 worker 里的代码粘贴上去

注意改下 sharePoint 地址，你可以去 onedrive 里随便下载个文件，主机名就是了，为了防止出错，留意下是类似于 xxx.sharepoint.com

[![](https://img.echoteen.com/upload/2020/05/01/0854243116f52.png)](https://img.echoteen.com/upload/2020/05/01/0854243116f52.png)

修改这两个地方为你的 sharePoint 地址就 ok 了。

然后在

[![](https://img.echoteen.com/upload/2020/05/01/5e23f50de8d5d.png)](https://img.echoteen.com/upload/2020/05/01/5e23f50de8d5d.png)

基本设置页面配置好 cdn 直链地址就 ok 啦~

对比下有了 CDN 和微软官方的速度

没配置 cdn

[![](https://img.echoteen.com/upload/2020/05/01/2fd3eb6a97df7.png)](https://img.echoteen.com/upload/2020/05/01/2fd3eb6a97df7.png)

套了 CDN 以后

[![](https://img.echoteen.com/upload/2020/05/01/9e0e0889f6bba.png)](https://img.echoteen.com/upload/2020/05/01/9e0e0889f6bba.png)

可以看到简直一个天上一个地下了！
