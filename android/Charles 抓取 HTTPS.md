Charles 抓取 HTTPS
========

- Charles 需要安装证书, 通过 Help -> SSL Proxying -> Install Charles Root Certificate 安装。

   - MAC 安装之后需要找到Charles Proxy CA,双击打开查看证书，在 信任 -> 使用证书时，选择-使用信任
- 在手机也需要安装证书，配置代理后打开 https://chls.pro/ssl ,下载并安装证书。

  - 苹果手机安装后，需要在 通用 - 关于本机 - 证书信任设置 - 针对根证书启用完全信任，打开Charles Proxy CA 的开关才能抓取HTTPS包。

  - 安卓手机，小米的手机自带浏览器，下载后无法安装该证书，需要用其他浏览器下载，安装的时候需要凭证密码，需要设置锁屏密码。TMD麻烦。。。

- MAC和手机都安装完证书之后，需要在 Charles 的 SSL Proxying Settings 进行设置，在SSL Proxying 添加，抓取全部用 * 表示，端口设置为443。