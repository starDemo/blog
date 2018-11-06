---
title: apache简单实现自定义403页面
url: 3387.html
id: 3387
categories:
  - 运维
date: 2016-05-07 12:31:56
tags:
---

如果不自定义403页面，当你试图访问一个页面出错时，apache默认页面将会显示出你的服务器环境信息。如果你想隐藏这些信息就需要自定义一个页面将默认页面替换掉，实现方式： 第一步：vim /etc/httpd/conf/httpd.conf 找到这一个地方: ErrorDocument 400 /error/HTTP\_BAD\_REQUEST.html.var #    ErrorDocument 401 /error/HTTP\_UNAUTHORIZED.html.var #    ErrorDocument 403 /error/HTTP\_FORBIDDEN.html.var ErrorDocument 403 [http://www.xx.com/error.html](http://www.xx.com/error.html) #    ErrorDocument 408 /error/HTTP\_REQUEST\_TIME\_OUT.html.var #    ErrorDocument 410 /error/HTTP\_GONE.html.var #    ErrorDocument 411 /error/HTTP\_LENGTH\_REQUIRED.html.var #    ErrorDocument 412 /error/HTTP\_PRECONDITION\_FAILED.html.var #    ErrorDocument 413 /error/HTTP\_REQUEST\_ENTITY\_TOO\_LARGE.html.var #    ErrorDocument 414 /error/HTTP\_REQUEST\_URI\_TOO\_LARGE.html.var #    ErrorDocument 415 /error/HTTP\_UNSUPPORTED\_MEDIA\_TYPE.html.var #    ErrorDocument 500 /error/HTTP\_INTERNAL\_SERVER\_ERROR.html.var #    ErrorDocument 501 /error/HTTP\_NOT\_IMPLEMENTED.html.var #    ErrorDocument 502 /error/HTTP\_BAD\_GATEWAY.html.v 新增一个403或者将原403项注释去掉直接编辑成我上面描红的样式，当然你也可以将403、404页面都指向到这里。然后保存退出。 第二步：在/var/www/html(对照你自己的网站根目录)下面新建一个error.html页面，页面内容自己编辑。 第三步：service httpd restart 结束。