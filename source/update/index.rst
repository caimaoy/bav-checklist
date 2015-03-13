升级
====

详细说明文档下载地址
-----------------------

http://client.baidu.com:8811/tool/storage

部分链接地址
=============

测试服务器部署地址
---------------------
http://hk01-client-cc.hk01.baidu.com:8000/autoUpdate/auto_deploy_rebuild/index.php?m=Op&a=toOnline&

ftp地址
--------
ftp://hk01-secure-test01.hk01.baidu.com//home/work/luoman

常用上传方法
-------------
::

    上传sign*.zip和nsign.zip到测试服务器10.240.36.36.
    登录securecrt
    ssh work@10.240.36.36
    密码：问老员工
    cd  /home/work/luoman
    rz –be
    在相关目录选择文件
    上传100%表示成功


常用hosts
-----------
::

    10.240.36.36 download.antivirus.baidu.com
    10.240.36.36 updown.bav.baidu.com

    10.240.36.36 update.bav.baidu.com
    10.240.36.36 download.bav.baidu.com 

    10.240.36.36 update.sd.baidu.com
    10.240.36.36 download.sd.baidu.com

    10.252.23.29 sync.sd.baidu.com
    10.252.23.29 bug.sd.baidu.com



模块升级
========

见链接: http://client.baidu.com:8811/tool/storage/tutorial


小工具部署
----------

| 有2种情况，如果是普通的打包测试连接，如: 
| http://172.17.194.10:8088/Build/bav_5.3/5.3.2.102045_87/
| 部署方法为：
| http://ps.jenkins.baidu.com/job/advtools_pack_and_deploy_local_shell/
| 输入打包链接即可，http://172.17.194.10:8088/Build/bav_5.3/5.3.2.102045_87/
| 
| 如果是专门的ToolServer.zip下载地址，如：
| ftp://getprod:getprod@buildprod.scm.baidu.com/temp/data/prod-32/app/gensoft/security-client/av-pro/bav_advtools_release_cluster/r102010-202/output
| 部署方法为：
| http://mars.baidu.com/Monitor/Channel.html#create
| 选择小工具部署，输入地址如下：
| ftp://getprod:getprod@buildprod.scm.baidu.com/temp/data/prod-32/app/gensoft/security-client/av-pro/bav_advtools_release_cluster/r102010-202/output/ToolsServer.zip

版本部署
--------
| http://mars.baidu.com/Monitor/Channel.html#create
| 选择版本升级部署，输入地址如下(请输入jenkins地址或者ftp地址，不支持http地址，即使只有server.zip，也要加上版本号server5.3.2.102694.zip)：
| http://ps.jenkins.baidu.com/job/copy_products_local/1452/artifact/output/server5.3.2.102694.zip


自动化失败情况处理
==================

版本升级
--------
#.  安装相关版本BAV
#.  关自保
#.  关闭小红伞引擎(setting界面)
#.  若存在升级进程，先kill。
#.  运行BavUpdater.exe  -svc_no_ui
#.  检查update文件夹下是否有相关文件下载(全量升级(如5.0升级到5.2)是下载安装包，增量升级是下载Bav.exe等文件)，server_response.xml中version字段是否为指定升级版本。

模块升级
--------
#. 步骤和版本升级类似
#. 第6步不一样，等待升级进程结束，继续运行步骤5，查看是否下载类似文件2014.12.11.45.FileList.xml和xml内部指定的exe文件。

小工具升级
----------

* update.sd.baidu.com/cgi-bin/get_bavtools_update_info.cgi?guid=TF645&ProgramVersion=5.0.2.*&os=6&ChannelName=web%7Cth%7Cofficial%7Cdirect&IsPro=yes&Ismanual=yes

::

    把ProgramVersion改成测试BAV版本，得到页面如下：
    <?xml version="1.0" encoding="utf-8" ?> 
    <ServerRespond XmlVersion="1.0">
    <UpdateTools Md5="0xf644072993c0e84ecde6b598158b3111" NeedUpdate="Yes" Size="1268" 
    Url="http://download.sd.baidu.com/baidu_tool/ToolsList_2014_10_10_17_10_26_0_******.xml" /> 
    </ServerRespond>
    从返回内容中可以找到ToolsList_2014_10_10_17_10_26_0，与规则文件中的tools_list匹配，证明部署成功，否则，需要重新部署。

* 步骤和2.1类似
* 第5步不一样。
* 保证config.ini中的backgroundupdating =0，toolslastupdatetime=0。
* 然后运行BavUpdater.exe  -tools_update_query
* 重新打开主界面，点击指定小工具。
* 检查小工具版本。
* 如果不符合预期，重新运行一次。

渠道包
-------
* 找PM询问失败安装包下载地址。
* 卸载BAV
* 若：BD Test Type: setup_and_xml 配置hosts 

::

    10.240.36.36  dl2.bav.baidu.com
    10.240.36.36  dl-vip.bav.baidu.com
    10.240.36.36  download.antivirus.baidu.com

*  运行Mini包，BAV安装成功后，检查渠道号，是否符合预期。（如果检查下载很慢，只需要检查sync.bav.baidu.com上报的字段中存在download_start即可）
* 检查邮件中的数字签名是否成功，如果失败，需要重新打包。

::

    BD Test Type: all | DigitalSign: Succeed | Version: 5.2.0.84536|5.0.3.93139 | Install: Succeed | Download: Succeed | Channel Number: Succeed | Test Result: Succeed


