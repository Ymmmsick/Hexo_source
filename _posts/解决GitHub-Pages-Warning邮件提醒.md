title: 解决GitHub Pages Warning邮件提醒
date: 2015-07-26 16:44:17
categories: Hexo
tags:
---
博客转载自:[http://mayecn.com/blog/2014/05/17/githubpages-accelerating/](http://mayecn.com/blog/2014/05/17/githubpages-accelerating/)


本文旨在解决每次写博都会收到 Github 的 warning 邮件问题，同时又提升了托管在 Github Pages 上的博客的访问速度。


　　今天在 v2ex 上看到大学邮箱注册某比特币交易网站送价值 10 刀的比特币的事，于是登陆了许久不用的学校教育网邮箱，然后就发现原来每次生成博客都会收到 Github 的一封 warning 邮件，内容如下：
　　
　　
The page build completed successfully, but returned the following warning:  

GitHub Pages recently underwent some improvements (https://github.com/blog/1715-faster-more-awesome-github-pages) to make your site faster and more awesome, but we’ve noticed that mayecn.com isn’t properly configured to take advantage of these new features. While your site will continue to work just fine, updating your domain’s configuration offers some additional speed and performance benefits. Instructions on updating your site’s IP address can be found at https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages#step-2-configure-dns-records, and of course, you can always get in touch with a human at support@github.com. For the more technical minded folks who want to skip the help docs: your site’s DNS records are pointed to a deprecated IP address.  

For information on troubleshooting Jekyll see:   

https://help.github.com/articles/using-jekyll-with-pages#troubleshooting  

If you have any questions please contact us at https://github.com/contact.


　　其实全文中最重要的就只有这一句话：
your site’s DNS records are pointed to a deprecated IP address.  

　　Github 贴心的提醒我现在的 A 记录指向的是一个不赞成使用的 IP 地址，意思就是如果采用了 Github 新增加的 IP 地址为 A 记录的话博客的访问速度会加快一些。于是使用 dig 命令势在必行。但是 Windows 下如何使用 dig 呢？在这里发现了一个现成的解决方案：Win 下使用 dig
　　照做后，在 cmd 下就可以愉快地使用 dig 命令了。执行：
dig maye696.github.io +nostats +nocomments +nocmd  

　　获得如下结果：  
<pre><code>
C:\Users\w>dig maye696.github.io +nostats +nocomments +nocmd
;maye696.github.io. IN A
maye696.github.io. 1097 IN CNAME github.map.fastly.net.
github.map.fastly.net. 30 IN A 103.245.222.133  
</code></pre>
　　很明显，你只需把自己的域名的 A 记录改为 103.245.222.133，就会使博客获得更快的访问速度了。同时，也不会每次写博都收到 Github 那“贴心”的邮件了。