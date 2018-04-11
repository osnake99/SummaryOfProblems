# Charles无法对Android7.0进行HTTPS抓包的问题

标签（空格分隔）： Charles 抓包 HTTPS

---

使用Mac和Charles进行HTTPS抓包，在苹果手机和Android4.4版本的手机上正常，可以显示HTTPS的内容，但是在抓取Android7.0的HTTPS请求时，总是不显示请求链接，有显示链接的也是显示connect方法，无法查看具体内容
![无法查看HTTPS的内容][1]
  
查询了一些参考资料：[android使用Charles抓包https请求][2]
明白这是android7.0安全策略问题。手机持有者也没有权限，只有该应用自己设置安全证书，你才能抓到该应用的https数据。

###添加安全配置文件

    <!--?xml version="1.0" encoding="utf-8"?-->
    <manifest ...="">
        ...
        </application>
    </manifest>

###配置用于调试的 CA
在res文件夹下新建xml文件夹，再在xml文件夹下新建network_security_config.xml文件：

    <code><code><!--?xml version="1.0" encoding="utf-8"?-->
    <network-security-config>
      <debug-overrides>
        <trust-anchors>
          <certificates src="@raw/debug_cas">
          </certificates>
        </trust-anchors>
      </debug-overrides>
    </network-security-config>

然后在res文件夹下新建raw文件夹，将手机上下载的安全证书重命名为"debug_cas"（没有后缀），并放到raw文件夹下。安全证书可到如下地址下载：https://www.charlesproxy.com/getssl/，最终就能正常抓包了。

  [1]: https://note.youdao.com/yws/public/resource/83a109abea179c6ad6a1b928f56c113b/xmlnote/9132923734A74665B92FFA6F1B1F7F75/6543
  [2]: https://www.2cto.com/kf/201708/665364.html