## LUAWAF
LUAWAF是我编写的一个针对Nginx的Web应用提供安全防护的Web应用防火墙。

## 支持平台
目前WAF只支持Windows平台

## 安装
1，使用本软件最好直接在OpenResty官网下载OpenResty，OpenResty自身带有Nginx

2，将该WAF下载到OpenResty的lua目录下，解压即可

3，日志读写的文件必须给与相应的权限

## 使用说明
1，需要在nginx.conf文件中包含WAF解压出来的conf中的waf.conf文件

2，需要在waf/conf路径下的config.lua文件中修改日志存储位置以及规则存储位置

## 配置说明
    是否开启白名单IP检查
    _M.IPWhiteCheck = "on"

    是否开启黑名单IP检查
    _M.IPBlockCheck = "on"

    是否开启白名单URl检查
    _M.WhiteUrlCheck = "on"

    是否开启黑名单URl检查
    _M.BlockUrlCheck = "on"

    是否开启参数检查
    _M.ArgCheck = "on"

    是否开启useragent检查
    _M.UserAgentCheck = "on"

    是否开启cookie检查
    _M.CookieCheck = "on"

    是否开启CC攻击防御
    _M.CCDenyCheck = "on"

    是否开启POST上传检查
    _M.PostCheck = "on"

    CC攻击的速率（次数/秒）
    _M.CCRate = "100/60"

    IP访问的速率（次数/秒）
    _M.Count = "100/60"

    是否开启日志记录
    _M.LogCheck = "on"

    疑似对攻击网站的行为的封禁时间(单位：秒)
    _M.BlockTime = "1800"

    是否开启错误页面重定向
    _M.Redirect = "on"

    是否开启某些关键路径只有某些IP可以访问
    _M.AccessControl = "on"

    是否开启XSS检测
    _M.XSSCheck = "on"

    是否开启数据库存储
    _M.MysqlLog = "on"

以上处于waf/conf/config.lua配置文件中，若是配置文件中问on的表示该功能启用，网站禁止访问时间以秒为单位，可以根据自己的需求进行设置，同样的检测CC攻击和IP访问频率也可以根据（次数/秒）设置

## 规则说明
LUAWAF采用的匹配规则大部分是[OWASP ModSecurity Core Rule Set (CRS) Project (Official Repository) ](https://github.com/SpiderLabs/owasp-modsecurity-crs)中的规则，同时还借鉴了[@loveshell](https://github.com/loveshell/ngx_lua_waf)的ngx_lua_waf中的规则

    规则配置文件在waf/rule文件夹下

    args.rulel里面的规则用于GET请求时检查参数和参数名是否正确
    block.rule里面的规则用于检测GET请求时用户访问的URL是否是敏感信息
    cookies.rule里面的规则用于检测在请求头中的cookie是否包含的扫描器特征值
    filenameExt.rule里面的规则用于检测上传的文件扩展名是否合法
    IPBlocklist.rule里面的规则是禁止访问的IP
    IPWhitelist.rule里面的规则是可以不需要进行权限控制和黑名单检测的IP
    manage_IP.json里面的规则用于权限管理检测
    post.rule里面的规则用于检测POST请求中的数据
    useragents.rule里面的规则用于检测在请求头中的useragent是否包含的扫描器特征值
    White.rule里面的规则不需要进行过滤
    xss.rule里面的规则用于检测XSS攻击

    默认开启了WAF的所有功能，若是需要关掉某些功能，只需要在access.lua文件中使用“--”对相应的功能注释

    
## 日志记录
    日志记录提供两种方式：本地日志记录和数据库记录

    本地日志记录的日志文件名称格式如下:虚拟主机名_sec.log
    本地日志记录的日志文件中的数据以JSON格式存储，方便Web应用管理员通过日志对Web的日志进行分析

    数据库记录需要手动建立一个waflog的数据库，并且需要在config.lua文件中配置数据库的相关配置

### 本地日志记录结果：
![](http://oo2nztqg3.bkt.clouddn.com/18-6-5/22190235.jpg)

### 数据库记录结果：
![](http://oo2nztqg3.bkt.clouddn.com/18-6-5/20330746.jpg)

## WAF的功能
    防御sql注入，xss等web攻击
    防止svn、数据库和网站备份等敏感信息文件泄漏
    防止ApacheBench之类压力测试工具的攻击
    屏蔽常见像SQLMAP、netsparkerd等黑客扫描工具
    屏蔽异常的网络请求
    禁止图片附件类目录php执行权限
    防止webshell上传
    提供对HTTP请求头中的useragent、cookie和referrer进行检测
    提供对Web应用后台只允许特定的IP进行访问

## 感谢
感谢OpenResty的开发者[@agentzh](https://github.com/agentzh/)

LUAWAF感谢[@loveshell](https://github.com/loveshell/ngx_lua_waf)的ngx_lua_waf所提供的思路