**5.为什么我的网站更新后，通过CDN后看到网页还是旧网页，如何解决？**　　

*由于CDN采用各节点缓存的机制，网站的静态网页和图片修改后，如果CDN缓存没有做相应更新，则看到的还是旧的网页。
为了解决这个问题，**CDN管理面板中提供了URL推送服务，来通知CDN各节点刷新自己的缓存。**　　
在URL推送地址栏中，输入具体的网址或者图片地址，则各节点中的缓存内容即被统一删除，并且当即生效。*

