---
layout: post
title:  "调试方法"
categories: 运维
tags:  后端 前端 linux sql 软件工具
author: shenqb
---

* content
{:toc}


## 前言

新人入门必备技能：后端调试、前端调试、SQL调试、shell调试。

##  前端调试


基于Chrome控制台做前端样式、前端脚本调试

说明：
1、Elements标签页：用来查看，修改页面上的元素，包括DOM标签，以及css样式的查看，修改，还有相关css盒模型的图形信息;  

2、Console控制台：用于打印和输出相关的命令信息，其实console控制台除了我们熟知的报错，打印console.log信息外，还可以查看断点时的对象信息；  

3、Sources源码页：这个页面内我们可以找到当然浏览器页面中的js 源文件，方便我们查看和调试，支持断点调试，查看调用栈信息；  

4、Network网络请求页：可以看到所有的资源请求，包括网络请求，图片资源，html,css，js文件等请求，可以根据需求筛选请求项，一般多用于网络请求的查看和分析，分析后端接口是否正确传输，获取的数据是否准确，请求头，请求参数的查看。  








### 1，Elements标签页



### 2，Console控制台



### 3，Sources源码页

> 1 简单打印

```js

每天早上6点 
0 6 * * * echo "Good morning." >> /tmp/test.txt //注意单纯echo，从屏幕上看不到任何输出，因为cron把任何输出都email到root的信箱了。

每两个小时 
0 */2 * * * echo "Have a break now." >> /tmp/test.txt  

晚上11点到早上8点之间每两个小时和早上八点 
0 23-7/2，8 * * * echo "Have a good dream" >> /tmp/test.txt

每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点 
0 11 4 * 1-3 command line

1月1日早上4点 
0 4 1 1 * command line SHELL=/bin/bash PATH=/sbin:/bin:/usr/sbin:/usr/bin MAILTO=root //如果出现错误，或者有数据输出，数据作为邮件发给这个帐号 HOME=/ 

每小时执行/etc/cron.hourly内的脚本
01 * * * * root run-parts /etc/cron.hourly

每天执行/etc/cron.daily内的脚本
02 4 * * * root run-parts /etc/cron.daily 

每星期执行/etc/cron.weekly内的脚本
22 4 * * 0 root run-parts /etc/cron.weekly 

每月去执行/etc/cron.monthly内的脚本 
42 4 1 * * root run-parts /etc/cron.monthly 

注意: "run-parts"这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是文件夹名。 　 

每天的下午4点、5点、6点的5 min、15 min、25 min、35 min、45 min、55 min时执行命令。 
5，15，25，35，45，55 16，17，18 * * * command

每周一，三，五的下午3：00系统进入维护状态，重新启动系统。
00 15 * * 1，3，5 shutdown -r +5

每小时的10分，40分执行用户目录下的innd/bbslin这个指令： 
10，40 * * * * innd/bbslink 

每小时的1分执行用户目录下的bin/account这个指令： 
1 * * * * bin/account

每天早晨三点二十分执行用户目录下如下所示的两个指令（每个指令以;分隔）： 
20 3 * * * （/bin/rm -f expire.ls logins.bad;bin/expire$#@62;expire.1st）　　

每年的一月和四月，4号到9号的3点12分和3点55分执行/bin/rm -f expire.1st这个指令，并把结果添加在mm.txt这个文件之后（mm.txt文件位于用户自己的目录位置）。 
12,55 3 4-9 1,4 * /bin/rm -f expire.1st$#@62;$#@62;mm.txt 
```

------

> 2 nginx示例

```js

30 21 * * * /etc/init.d/nginx restart
每晚的21:30重启 nginx。

45 4 1,10,22 * * /etc/init.d/nginx restart
每月1、 10、22日的4 : 45重启nginx。

10 1 * * 6,0 /etc/init.d/nginx restart
每周六、周日的1 : 10重启nginx。

0,30 18-23 * * * /etc/init.d/nginx restart
每天18 : 00至23 : 00之间每隔30分钟重启nginx。

0 23 * * 6 /etc/init.d/nginx restart
每星期六的11 : 00 pm重启nginx。

* */1 * * * /etc/init.d/nginx restart
每一小时重启nginx

* 23-7/1 * * * /etc/init.d/nginx restart
晚上11点到早上7点之间，每 隔一小时重启nginx

0 11 4 * mon-wed /etc/init.d/nginx restart
每月的4号与每周一到周三 的11点重启nginx

0 4 1 jan * /etc/init.d/nginx restart
一月一号的4点重启nginx

*/30 * * * * /usr/sbin/ntpdate 210.72.145.20
每半小时同步一下时间
```

###  4 Network网络请求页



##  后端调试


基于IDEA做后端代码调试



##  SQL调试


基于执行计划做SQL调试



##  shell调试


基于sh -x指令做shell调试






引用：

https://blog.csdn.net/Ivana_zyf/article/details/78783680
https://www.cnblogs.com/longjshz/p/5779215.html
http://blog.csdn.net/edgdvcyz/article/details/53348832








