# chromedriver Mac 安装

> 参考链接
>
> chromedriver Mac 安装
> <https://www.jianshu.com/p/4276742d74d0>
>
> mac 搭建selenium与ChromeDriver环境
> <https://www.jianshu.com/p/39716ea15d99>


下载ChromeDriver地址：

<http://npm.taobao.org/mirrors/chromedriver/>
或者
<http://chromedriver.storage.googleapis.com/index.html>


本机Google Chrome 版本为77.0.3865.90（正式版本），下载`77.0.3865.40`。

安装方法如下：
```
cp  chromedriver /usr/local/bin/
cd /usr/local/bin/
sudo chmod u+x,o+x   /usr/local/bin/chromedriver
chromedriver --version
```

```python

from selenium import webdriver

driver = webdriver.Chrome()
base_url = 'https://www.baidu.com'
driver.get(base_url)
```