---
title: PM2使用随记，强大的NodeJS启动管理工具
url: 3560.html
id: 3560
categories:
  - Codeing
  - node
  - 运维
date: 2017-12-23 12:03:14
tags:
---

之前做的Node IPP 项目需要在嵌入式设备中自动启动进程管理，原计划采用传统的方式  ==一个Shell丢进Cron里（不熟悉Node==尬） 然后查到了论坛等使用Forever以及Forever下成片的评论PM2好用 嗯  所以尝试了下 确实不错 分割线===== PM2的安装很简单`npm install pm2 -g` 安装好以后启动单个项目使用`pm2 start app.js` 随后可以使用`pm2 monit`启动Shell模式下的监控程序==界面还是不错的 ![](http://blog.istarboy.cc/wp-content/uploads/2017/12/pm2-monit.png) 但是我们需要批量集群的管理Node启动的服务，所以官方提供了通过JSON，JSON5，YML启动服务 [文档](http://pm2.keymetrics.io/docs/usage/application-declaration/)通道 我是用的是如下JSON Format

    {
      "apps" : [{
        "name"        : "worker",
        "script"      : "./worker.js",
        "watch"       : true,
        "env": {
          "NODE_ENV": "development"
        },
        "env_production" : {
           "NODE_ENV": "production"
        }
      },{
        "name"       : "api-app",
        "script"     : "./api.js",
        "instances"  : 4,
        "exec_mode"  : "cluster"
      }]
    }

可以设置变量如下附页==也可以去参考下官方文档，本处只做相关备忘== BTW这个可以自动生成服务配置进入开机启动项嗯 是最舒服的 以下是一些常用的命令，作为备忘=

$ pm2 start app.js # 启动app.js应用程序 $ pm2 start app.js -i 4 # cluster mode 模式启动4个app.js的应用实例 # 4个应用程序会自动进行负载均衡 $ pm2 start app.js --name="api" # 启动应用程序并命名为 "api" $ pm2 start app.js --watch # 当文件变化时自动重启应用 $ pm2 start script.sh # 启动 bash 脚本 $ pm2 list # 列表 PM2 启动的所有的应用程序 $ pm2 monit # 显示每个应用程序的CPU和内存占用情况 $ pm2 show \[app-name\] # 显示应用程序的所有信息 $ pm2 logs # 显示所有应用程序的日志 $ pm2 logs \[app-name\] # 显示指定应用程序的日志 pm2 flush $ pm2 stop all # 停止所有的应用程序 $ pm2 stop 0 # 停止 id为 0的指定应用程序 $ pm2 restart all # 重启所有应用 $ pm2 reload all # 重启 cluster mode下的所有应用 $ pm2 gracefulReload all # Graceful reload all apps in cluster mode $ pm2 delete all # 关闭并删除所有应用 $ pm2 delete 0 # 删除指定应用 id 0 $ pm2 scale api 10 # 把名字叫api的应用扩展到10个实例 $ pm2 reset \[app-name\] # 重置重启数量 $ pm2 startup # 创建开机自启动命令 $ pm2 save # 保存当前应用列表 $ pm2 resurrect # 重新加载保存的应用列表 $ pm2 update # Save processes, kill PM2 and restore processes $ pm2 generate # Generate a sample json configuration file $pm2 start app.js --node-args="--max-old-space-size=1024"

### General

Field

Type

Example

Description

name

(string)

“my-api”

application name (default to script filename without extension)

script

(string)

”./api/app.js”

script path relative to pm2 start

cwd

(string)

“/var/www/”

the directory from which your app will be launched

args

(string)

“-a 13 -b 12”

string containing all arguments passed via CLI to script

interpreter

(string)

“/usr/bin/python”

interpreter absolute path (default to node)

interpreter_args

(string)

”–harmony”

option to pass to the interpreter

node_args

(string)

alias to interpreter_args

### Advanced features

Field

Type

Example

Description

instances

number

-1

number of app instance to be launched

exec_mode

string

“cluster”

mode to start your app, can be “cluster” or “fork”, default fork

watch

boolean or \[\]

true

enable watch & restart feature, if a file change in the folder or subfolder, your app will get reloaded

ignore_watch

list

\[”\[\\/\\\\]\\./”, “node_modules”\]

list of regex to ignore some file or folder names by the watch feature

max\_memory\_restart

string

“150M”

your app will be restarted if it exceeds the amount of memory specified. human-friendly format : it can be “10M”, “100K”, “2G” and so on…

env

object

{“NODE_ENV”: “development”, “ID”: “42”}

env variables which will appear in your app

env_

object

{“NODE_ENV”: “production”, “ID”: “89”}

inject when doing pm2 restart app.yml --env

source\_map\_support

boolean

true

default to true, \[enable/disable source map file\]

instance_var

string

“NODE\_APP\_INSTANCE”

[see documentation](http://pm2.keymetrics.io/docs/usage/environment/#specific-environment-variables)

### Log files

Field

Type

Example

Description

log\_date\_format

(string)

“YYYY-MM-DD HH:mm Z”

log date format (see log section)

error_file

(string)

error file path (default to $HOME/.pm2/logs/XXXerr.log)

out_file

(string)

output file path (default to $HOME/.pm2/logs/XXXout.log)

combine_logs

boolean

true

if set to true, avoid to suffix logs file with the process id

merge_logs

boolean

true

alias to combine_logs

pid_file

(string)

pid file path (default to $HOME/.pm2/pid/app-pm_id.pid)

### Control flow

Field

Type

Example

Description

min_uptime

(string)

min uptime of the app to be considered started

listen_timeout

number

8000

time in ms before forcing a reload if app not listening

kill_timeout

number

1600

time in milliseconds before sending [a final SIGKILL](http://pm2.keymetrics.io/docs/usage/signals-clean-restart/#cleaning-states-and-jobs)

wait_ready

boolean

false

Instead of reload waiting for listen event, wait for process.send(‘ready’)

max_restarts

number

10

number of consecutive unstable restarts (less than 1sec interval or custom time via min_uptime) before your app is considered errored and stop being restarted

restart_delay

number

4000

time to wait before restarting a crashed app (in milliseconds). defaults to 0.

autorestart

boolean

false

true by default. if false, PM2 will not restart your app if it crashes or ends peacefully

cron_restart

string

“1 0 * * *”

a cron pattern to restart your app. Application must be running for cron feature to work

vizion

boolean

false

true by default. if false, PM2 will start without vizion features (versioning control metadatas)

post_update

list

\[“npm install”, “echo launching the app”\]

a list of commands which will be executed after you perform a Pull/Upgrade operation from Keymetrics dashboard

force

boolean

true

defaults to false. if true, you can start the same script several times which is usually not allowed by PM2