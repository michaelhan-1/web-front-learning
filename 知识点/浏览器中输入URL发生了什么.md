原文： https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a

DNS: Domain Name System的缩写，作用就是根据域名查出IP地址。类似电话本的作用。浏览器要访问某个网址，首先要查出其IP地址。

### 查询IP地址过程
- 首先会检查缓存里有没有该域名的DNS记录，会检查以下四种缓存
  1. 浏览器缓存
  2. OS（操作系统）缓存
  3. 路由器缓存
  4. ISP(Internet service provider)缓存
- 如果缓存里没有，ISP的服务器会发起一个DNS查询以查出域名的IP地址

这个过程会从根域名开始一层一层地查寻，直到找到对应域名的IP地址。如下图所示：
![image](https://user-images.githubusercontent.com/69185043/137101480-081898c5-0109-4a90-8f45-303f71ca9c5c.png)



浏览器得到其IP地址后会发起TCP连接，成功之后就是http请求和响应的过程，最后浏览器渲染界面。


