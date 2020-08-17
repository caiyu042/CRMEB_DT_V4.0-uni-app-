CRMEB 4.0
===============

## 注意事项！！！

### CRMEB v4.0前端源码包
本源码为CRMEB单商户V4.0前端源码包，需搭配CRMEB V4.0后端程序才可以正常使用。

### 商用需购买CRMEB正版授权
为保证您合法权益，及软件的完整性与安全性，同时您在后续的使用得到官方的技术支持与更新升级，请购买CRMEB官方授权。

### 购买方式
您可以通过 淘宝 或 微信CRMEB公众号官方商城购买。
淘宝：[CRMEB官方企业店](https://xazbkj.taobao.com/)
微信：搜索公众号“CRMEB”，关注后点击下方菜单“官方商城”，进入后即可购买。

![扫码购买更优惠](https://github.com/xingfuggz/CRMEB_DT_V4.0-uni-app-/blob/master/static/img/goumai.jpg)

扫描上方小程序码进入官方小程序商城购买可使用以下优惠码，购买授权更加优惠。

- **5元优惠码：**97692944484，34293378，29137416368
- **10元优惠码：**74392244484，93303278，60983316368
- **20元优惠码：**55413644484，21211078，93922516368

### 正版的权益

使用正版软件，我们会提供：
1.免费升级，程序的bug修复、功能更新等后续的程序升级您都将永久享受。
2.售后无忧，我们提供专属客服、售后技术、交流群、论坛等种种售后措施，保证您的正常使用。
3.保障服务，后期软件升级的新功能，您都将免费获取使用，无需额外付费。


## 主要特性

> 运行环境要求PHP7.1+。
### TP6框架
使用最新的 ThinkPHP 6.0框架开发
### 前端采用Vue CLI框架
前端使用Vue CLI框架nodejs打包，页面加载更流畅，用户体验更好
### 标准接口
标准接口、前后端分离，二次开发更方便
### 支持队列
降低流量高峰，解除耦合，高可用
### 长连接
减少CPU及内存使用及网络堵塞，减少请求响应时长
### 无缝事件机制
行为扩展更方便，方便二次开发
### 后台快速生成表单
后台应用form-builder 无需写页面快速增删改查
### 数据表格导出
PHPExcel数据导出,导出表格更加美观可视；
### 数据统计分析
后台使用ECharts图表统计，实现用户、产品、订单、资金等统计分析
### 强大的后台权限管理
后台多种角色、多重身份权限管理，权限可以控制到每一步操作
### 一件安装
自动检查系统环境一键安装

## 安装

## 一键安装
上传你的代码，站点入口目录设置/public
在浏览器中输入你的域名或IP（例如：www.yourdomain.com）,
安装程序会自动执行安装。期间系统会提醒你输入数据库信息以完成安装，安装完成后建议删除install目录下index.php文件或将其改名。

后台访问地址：
1.域名/admin
2.域名/index.php/admin
3.域名/index.php?s=/admin
公众号和H5首页访问地址：
1.域名/
提示：正常访问是第一中模式，第一种访问不了请检测[URL重写](http://help.crmeb.net/895486)是否配置好
安装过程中请牢记您的账号密码！

## 重新安装
1. 清除数据库
2. 删除/public/install/install.lock 文件

## 手动安装
1.创建数据库，倒入数据库文件
数据库文件目录/public/install/crmeb.sql
2.修改数据库连接文件
配置文件路径/.env
~~~
APP_DEBUG = true

[APP]
DEFAULT_TIMEZONE = Asia/Shanghai

[DATABASE]
TYPE = mysql
HOSTNAME = 127.0.0.1 #数据库连接地址
DATABASE = test #数据库名称
USERNAME = username #数据库登录账号
PASSWORD = password #数据库登录密码
HOSTPORT = 3306 #数据库端口
CHARSET = utf8
DEBUG = true

[LANG]
default_lang = zh-cn
~~~
3.修改目录权限（linux系统）777
/public
/runtime
4.后台登录：
http://域名/admin
默认账号：admin 密码：crmeb.com

## 定时任务
在自动收货,库存预警等功能使用到
```sh
php think timer [ status ] [ --d ]
```
参数
- status: 状态
    - start: 启动
    - stop: 关闭
    - restart: 重启
- --d : 后台执行
## 长连接服务
在h5聊天,后台管理员消息通知等功能使用到
```sh
php think workerman [ status ] [ server ] [ --d ]
```
windows环境下需要分三步执行
```sh
# 内部通讯服务
php think workerman start channel
# h5端聊天服务
php think workerman start chat
# 后台管理员通知
php think workerman start admin
```
参数
- status: 状态
    - start: 启动
    - stop: 关闭
    - restart: 重启
- server: 服务 (windows)
    - channel: 内部通讯
    - chat: h5
    - admin: 后台

- --d : 后台执行

## 文档

[使用手册](https://help.crmeb.net)
[TP6开发手册](https://www.kancloud.cn/manual/thinkphp6_0/content)


## 参与开发

请参阅 [CRMEB](https://github.com/crmeb/CRMEB)。

## 版权信息


本项目包含的第三方源码和二进制文件之版权信息另行标注。

版权所有Copyright © 2017-2020 by CRMEB (http://www.crmeb.com)

All rights reserved。

CRMEB® 商标和著作权所有者为西安众邦网络科技有限公司。
