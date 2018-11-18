---
layout: post
title:  "双机调试配置心得[转]"
date:   2018/09/12 23:00:00
categories: 内核
tag: 内核
---

1. 安装虚拟机VM
2. 创建虚拟机
3. 安装操作系统(Ghost XP)
4. 双击调试的设置

    1. 配置WinDbg的环境,在path变量里，在变量值后面增加: ;+Windgb的安装目录
    
        ![image]({{ '/styles/images/kernel/1.png' | prepend: site.baseurl }})


    2. 在环境变量里新建一个变量名称为_NT_SYMBOL_PATH,变量值为SRV\*e:\symbol\*http://msdl.microsoft.com/download/symbols,其中e:\symbol为你的symbol的安装时方的目录
    
        ![image]({{ '/styles/images/kernel/2.png' | prepend: site.baseurl }})


    3. 右键新建快捷方式,地址为："C:\Program Files\Debugging Tools for Windows (x86)\windbg.exe" -b -k com:port=//./pipe/com_1,baud=115200,pipe 注意第二个"后面要有一个空格""里内容为windbg.exe的路径

        ![image]({{ '/styles/images/kernel/3.png' | prepend: site.baseurl }})

 
    4. 点击下一步,完成了WinDbg的全部配置

    5. 编辑虚拟机设置

        ![image]({{ '/styles/images/kernel/4.png' | prepend: site.baseurl }})


    6. 点击添加,选择串行端口，点击下一步

        ![image]({{ '/styles/images/kernel/5.png' | prepend: site.baseurl }})


    7. 选择输出到命名管道，点下一步

        ![image]({{ '/styles/images/kernel/6.png' | prepend: site.baseurl }})


    8. 下一步设置如下,这里要注意要设置成//./pipe/com_1，如果设置成了\\.\pipe\com_1那你就悲剧了，搞半天你会发现不知道哪里出问题了

        ![image]({{ '/styles/images/kernel/7.png' | prepend: site.baseurl }})


    9. 点击完成，再点击确定完成串行端口的添加

        ![image]({{ '/styles/images/kernel/8.png' | prepend: site.baseurl }})


    10. 修改C盘目录下的隐藏只读文件boot.ini
    
        ![image]({{ '/styles/images/kernel/9.png' | prepend: site.baseurl }})


    11. 复制multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="Microsoft Windows XP Professional" /noexecute=optin /fastdetect /noexecute=alwaysoff这段代码到下面,然后在后面添加/debug /debugport=com /baudrate=115200,这里要注意:/debugport=com这条设置,如果你是7.1以下版本，请设置为/debugport=com1或者/debugport=com_1,反正不同版本这个/debugport的值设置不一样，如果不能连接，请把每一个都试遍，这里我用的是官方的7.1，经过测试这里要设置为/debugport=com才能正常连接.

        ![image]({{ '/styles/images/kernel/10.png' | prepend: site.baseurl }})


    12. 设置完保存重新启动操作系统，选择启动调试程序,进入操作系统

        ![image]({{ '/styles/images/kernel/11.png' | prepend: site.baseurl }})


    13. 打开刚才保存的Windbg快捷方式，会自动连接，并且发现有断点，输入g命令执行

        ![image]({{ '/styles/images/kernel/12.png' | prepend: site.baseurl }})
