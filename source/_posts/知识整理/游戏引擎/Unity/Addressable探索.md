---
title: Addressable探索
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-08 16:30:59
password:
summary:
tags: 
- Unity
- Addressable
categories: 游戏引擎
---

> 测试环境：
> Unity2019.4.15f1c1
> Addressable1.16.15
> Apache2.4.46(win64)

## 安装Addressable
1. 打开菜单Widnow-->Package Manager
![](/images/Addressable/a1.png)
2. 在搜索框输入add，检查Addressable版本，确认无误后点击`install`
![](/images/Addressable/a2.png)
3. 初始化
![](/images/Addressable/a3.png)
![](/images/Addressable/a4.png)
![](/images/Addressable/a5.png)

## 安装Apache
1. 下载Apache，地址：http://httpd.apache.org/download.cgi，解压缩Apache24文件夹
2. 以管理员身份启动cmd
![](/images/Addressable/a6.png)
3. 进入`Apache24\bin`目录，输入`httpd -k install`
4. 打开`Apache24\conf\httpd.conf`文件
5. 找到`Define SRVROOT`后面是Apache24的解压目录，这里是`Define SRVROOT "D:\Apache24"`
6. 找到`ServerName`后面是服务器的url，这里是`ServerName http://localhost:8080/`
   

## 源码
PlayerBuildVersion
```csharp
/// <summary>
        /// The version of the player build.  This is implemented as a timestamp int UTC of the form  string.Format("{0:D4}.{1:D2}.{2:D2}.{3:D2}.{4:D2}.{5:D2}", now.Year, now.Month, now.Day, now.Hour, now.Minute, now.Second).
        /// </summary>
        public string PlayerBuildVersion
        {
            get
            {
                if (!string.IsNullOrEmpty(m_overridePlayerVersion))
                    return profileSettings.EvaluateString(activeProfileId, m_overridePlayerVersion);
                var now = DateTime.UtcNow;
                return string.Format("{0:D4}.{1:D2}.{2:D2}.{3:D2}.{4:D2}.{5:D2}", now.Year, now.Month, now.Day, now.Hour, now.Minute, now.Second);
            }
        }
```

addressables_content_state.bin内容
![](/images/Addressable/a7.png)

   