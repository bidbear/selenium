# selenium
selenium学习（爬虫）
## 下载安装 selenium 模块
`pip install selenium`
## 基本用法
```
from selenium import webdriver
browser = webdriver.Firefox()
browser.get('https://www.toutiao.com/a6662620813969064459/')

```
 使用selenium需要下载相应的浏览器驱动，以Firefox为例，根据本地安装的浏览器下载对应的驱动[驱动下载地址](https://github.com/mozilla/geckodriver/releases/),下载后解压，把exe放在运行文件同目录即可或者配置环境变量。
## 声明浏览器
```
# -*- coding: utf-8 -*-
from selenium import webdriver
#声明谷歌、Firefox、Safari等浏览器
browser=webdriver.Chrome()
browser=webdriver.Firefox()
browser=webdriver.Safari()
browser=webdriver.Edge()
browser=webdriver.PhantomJS()
```
## 选择元素的方法
```
browser.find_element_by_css_selector('div.class')  通过css选择器选择
browser.find_element_by_xpth('//from[@id='loginFrom']') 通过xpth选择
browser.find_element_by_id('idname')  通过元素id选择
browser.find_element_by_name('password')  通过元素的name选择
browser.find_element_by_link_text('linkText') 通过链接地址选择
browser.find_element_by_partial_link_text('linkText') 通过链接的部分选择
browser.find_element_by_tag_name('h1')  通过元素的名称选择
browser.find_element_by_class_name('className') 通过元素的class选择
```
返回多个元素，迭代器
```
browser.find_elements_by_name()
browser.find_elements_by_xpath()
browser.find_elements_by_link_text()
browser.find_elements_by_partial_link_text()
browser.find_elements_by_tag_name()
browser.find_elements_by_class_name()
browser.find_elements_by_css_selector()
```
获取元素后可以进行元素的操作
```
user = browser.find_element_by_name('username')
user.clear()  清除元素中的内容
user.send_keys('要输入的内容')
element.click() 单击元素
element.submit()  提交表单
```
## 高级用法限制css，图片，flash的加载
```
def disableImages():
    ## get the Firefox profile object
    firefoxProfile = FirefoxProfile()
    ## Disable CSS
    firefoxProfile.set_preference('permissions.default.stylesheet', 2)
    ## Disable images
    firefoxProfile.set_preference('permissions.default.image', 2)
    ## Disable Flash
    firefoxProfile.set_preference('dom.ipc.plugins.enabled.libflashplayer.so',
                                  'false')
    ## Set the modified profile while creating the browser object 
    return webdriver.Firefox(firefoxProfile)
#声明浏览器可调用这个函数
browser = disableImages()
```
