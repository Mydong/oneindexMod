# OneindexMod魔改版完全使用教程和下载加速

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [https://www.echoteen.com/oneindex-paged-fast.html](https://www.echoteen.com/oneindex-paged-fast.html)


<br />上次二开了大佬的 oneindex 程序，持持续续有大佬在不断的关注，不断的提 bug 和功能，上班的时候不是很有时间，现在趁着假期，重构了部分代码，带来了比较实用性的功能，修改了重要的 bug<br />
<br />本篇文章介绍如何去使用和安装，怎么配置 redis 和 sharePoint 的 CDN 提高下载速度。<br />
<br />项目地址：[https://github.com/david7207/oneindexMod.git](https://github.com/david7207/oneindexMod.git)<br />

<a name="09ef796f"></a>
## 升级安装

<br />如果之前用过可以直接删掉代码，重新来一遍就好了，注意保留好 config 目录下的 base.php<br />
<br />里面有 clientid 和密钥，可以复用！<br />

<a name="f409f986"></a>
## 安装（以宝塔面板为 demo）

<br />材料：office365 A1，E3，或者 E5<br />
<br />没有材料的可以去 [https://office.shax.vip](https://office.shax.vip) 去免费自助获取<br />
<br />拉取代码到服务器目录<br />

```
git pull https://github.com/david7207/oneindexMod.git
```

<br />去网站里新建一个站点<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181909726-7ccfeecf-5019-4a5e-993c-862a38f7c999.png#align=left&display=inline&height=454&margin=%5Bobject%20Object%5D&originHeight=876&originWidth=1440&size=0&status=done&style=none&width=746)[<br />](https://img.echoteen.com/upload/2020/05/01/eab634e233b5e.png)<br />
<br />配置好站点域名，php 版本（一定要是 php7.0+）<br />
<br />必要操作：安装 Redis 并开启 redis 服务，php 必须安装 redis 模块<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181948996-76133e28-ff89-428e-a654-d1a975a1b99e.png#align=left&display=inline&height=771&margin=%5Bobject%20Object%5D&originHeight=771&originWidth=1604&size=0&status=done&style=none&width=1604)
<a name="e026f359"></a>
## 配置 graph Api

<br />（可以直接在第三步创建，但是建议自己配置好，懒癌患者 pass）<br />
<br />用分配的账号登陆 [https://portal.azure.com/](https://portal.azure.com/)<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181959308-078e6b43-1c11-4e03-a929-758e36db1098.png#align=left&display=inline&height=857&margin=%5Bobject%20Object%5D&originHeight=857&originWidth=1815&size=0&status=done&style=none&width=1815)[<br />](https://img.echoteen.com/upload/2020/05/01/4e85e6c582fd4.png)<br />

<a name="61689811"></a>
### 打开 azure Directory

<br />应用注册去注册一个应用程序，然后去证书和密码选项申请一个密码，时效为永久<br />
<br />注意添加一个回调地址<br />
<br />[https://tui.echoteen.com/api/office/serve](https://tui.echoteen.com/api/office/serve)<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181970233-232b5cb3-307c-4cc7-b2de-dd6acce45efb.png#align=left&display=inline&height=709&margin=%5Bobject%20Object%5D&originHeight=709&originWidth=1848&size=0&status=done&style=none&width=1848)[<br />](https://img.echoteen.com/upload/2020/05/01/495c64950dcc1.png)<br />
<br />然后去添加接口权限，把文件相关的权限勾选上就好了<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181981403-fc1ac7e1-efd1-4a17-9063-3f8b354eab41.png#align=left&display=inline&height=781&margin=%5Bobject%20Object%5D&originHeight=781&originWidth=1494&size=0&status=done&style=none&width=1494)[<br />](https://img.echoteen.com/upload/2020/05/01/f59736f66b0de.png)<br />
<br />不一定是上面所有的权限，只需要 file 相关的权限就好<br />
<br />主要是应用程序级别的 files.readwrite.all<br />
<br />好了，graph 就到这 ok 了<br />

<a name="587c8fbd"></a>
## 配置网站

<br />打开网站域名配置<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181990441-43bf8dca-4bc8-4af9-97ce-87d4a10fd84e.png#align=left&display=inline&height=808&margin=%5Bobject%20Object%5D&originHeight=808&originWidth=1808&size=0&status=done&style=none&width=1808)[<br />](https://img.echoteen.com/upload/2020/05/01/dbf50d8329f9b.png)<br />
<br />如果没有 config 和 cache 文件夹请手动创建，并赋予 777 权限<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589181998971-3924eccf-801f-49a8-a4cc-c4872366391c.png#align=left&display=inline&height=813&margin=%5Bobject%20Object%5D&originHeight=813&originWidth=1677&size=0&status=done&style=none&width=1677)[<br />](https://img.echoteen.com/upload/2020/05/01/8047293b02540.png)<br />
<br />如果你没进行第二步或者你之前没创建过应用，可以点击这个去创建然后配置好相应的权限<br />
<br />redis 密码，如果没有设置就不填，设置了密码就写上去<br />
<br />cdn 直链等会我们再说，不过可以在配置里面再次配置，先过去<br />

<a name="5b346201"></a>
## 配置全局 CDN

<br />直接去 cloudflare 去创建一个 cdn，配置好 cname 或者 ns 就好了。<br />

<a name="fb4e1138"></a>
## 配置 sharePoint 下载加速（可选，但是速度快！）

<br />先留意下源码目录里的 worker.js<br />
<br />去 cloudflare 打开 workers<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589182010830-197ca959-7836-41fb-94d8-95646325c8cc.png#align=left&display=inline&height=674&margin=%5Bobject%20Object%5D&originHeight=674&originWidth=1850&size=0&status=done&style=none&width=1850)<br />
<br />创建一个 worker<br />
<br />把 worker 里的代码粘贴上去<br />
<br />注意改下 sharePoint 地址，你可以去 onedrive 里随便下载个文件，主机名就是了，为了防止出错，留意下是类似于 xxx.sharepoint.com<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589182020295-f4c68f50-2bfe-46c4-abc3-b9f271fee62b.png#align=left&display=inline&height=343&margin=%5Bobject%20Object%5D&originHeight=603&originWidth=1310&size=0&status=done&style=none&width=746)[<br />](https://img.echoteen.com/upload/2020/05/01/0854243116f52.png)<br />
<br />修改这两个地方为你的 sharePoint 地址就 ok 了。<br />
<br />然后在<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589182082585-749aa120-d652-46f3-94cc-274b05c7d641.png#align=left&display=inline&height=762&margin=%5Bobject%20Object%5D&originHeight=762&originWidth=1539&size=0&status=done&style=none&width=1539)[<br />](https://img.echoteen.com/upload/2020/05/01/5e23f50de8d5d.png)<br />
<br />基本设置页面配置好 cdn 直链地址就 ok 啦~<br />
<br />对比下有了 CDN 和微软官方的速度<br />
<br />没配置 cdn<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589182094806-65f757a7-7a58-4ff2-9d4c-f54b51ee3f76.png#align=left&display=inline&height=768&margin=%5Bobject%20Object%5D&originHeight=768&originWidth=1311&size=0&status=done&style=none&width=1311)[<br />](https://img.echoteen.com/upload/2020/05/01/2fd3eb6a97df7.png)<br />
<br />套了 CDN 以后<br />
<br />![](https://cdn.nlark.com/yuque/0/2020/png/462793/1589182101339-86138ad9-baf2-4179-a8db-934fddad8463.png#align=left&display=inline&height=767&margin=%5Bobject%20Object%5D&originHeight=767&originWidth=1636&size=0&status=done&style=none&width=1636)[<br />](https://img.echoteen.com/upload/2020/05/01/9e0e0889f6bba.png)<br />
<br />可以看到简直一个天上一个地下了！
