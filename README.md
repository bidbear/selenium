# selenium
selenium学习（爬虫）[api文档](https://selenium-python.readthedocs.io/installation.html)
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

## 模拟鼠标事件
```
click()  单机鼠标左键
clear()  清空内容
sendkeys()  输入数据
submit()  提交表单
鼠标悬停
需要引入模块
from selenium.webdriver.common.action_chains import ActionChains

ActionChains(driver).move_to_element(select_obj).perform() 鼠标悬停
move_to_element() 鼠标悬停
context_click()  右击鼠标
double_click()   双击鼠标

```
## 模拟键盘事件
```
需要引入模块
from selenium.webdriver.common.keys import Keys
send_keys(Keys.ENTER)
send_keys(Keys.F1)
send_keys(Keys.CONTROL,'c')
send_keys(Keys.CONTROL,'v')
send_keys(Keys.CONTROL,'a')
send_keys(Keys.CONTROL,'x')
send_keys(Keys.TAB)
```
## 模拟下拉框
```
需要导入模块
from selenium.webdriver.support.select import Select
select = Select(driver.find_element_by_id('下拉框元素'))
select.select_by_index(2)  索引值从0开始
select.select_by_value('值')
select.select_by_visible_text('文本值')
deselect_all()  取消所有选项
deselect_by_index()    取消对应index选项
deselect_by_value()    取消对应value选项
deselect_by_visible_text()   取消对应文本选项
first_selected_option()    返回第一个选项
all_selected_options()     返回所有的选项
```
## 浏览器句柄
```
now = driver.current_window_handle  获取当前浏览器句柄
all = driver.window_handles         获取所有浏览器句柄（返回个可迭代器）索引从0 开始
driver.switch_to.window(all[1])     切换到新的浏览器窗口
```
## Alert窗口
```
alter 弹出框处理
a = driver.switch_to.alert
a.text    获取弹出文本的内容
a.accept()  点击确认按钮
a.dismiss()   关闭弹出框 
a.send_keys()    输入文本值
```
## iframe
```
driver.switch_to.frame('id值')   切换到iframe窗口
driver.switch_to.frame(driver.find_element_by_tag_name('iframe'))   切换到iframe窗口
driver.switch_to.default_content()     切换回主窗口

```
## 定位单选，复选
```
is_selected()   判断是否被选中
driver.find_element_by_id('boy').is_selected()    返回TRUE/FLASE
复选
boxs =driver.find_elements_by_xpath(".//*[@type='checkbox']")
for i in box:
   i.click()
```
## js处理滚动条
```
滚动条拉倒底部
js =  "var q= document.documentElement.scrollTop = 10000"
driver.execute_script(js)

js =  "window.scrollTo(0,document.body.scrollHeight)"
driver.execute_script(js)
滚动条拉到顶部
js = "var q =  document.documentElement.scrollTop = 0"
driver.execute_script(js)

js = "window.scrollTo(0,0)"
driver.execute_script(js)

滚动条拉到指定位置
target = driver.find_element_by_id("id_keypair")
driver.execute_script("arguments[0].scrollintoView();",target)
```
## js处理时间控件
```
js = 'document.getElementById('train_data').removeAttribute("readonly")'
driver.execute_script(js)
time.sleeep(3)
js_data = 'document.getElementById("train_data").value=2019-01-24'
driver.execute_script(js_data)
```
## 高级用法限制css，图片，flash的加载
```
from selenium import webdriver
from selenium.webdriver.firefox.firefox_profile import FirefoxProfile

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
## selenium 解决页面滚动懒加载

```
all_window_height =  []  # 创建一个列表，用于记录每一次拖动滚动条后页面的最大高度
all_window_height.append(driver.execute_script("return document.body.scrollHeight;")) #当前页面的最大高度加入列表
while True:
    driver.execute_script("scroll(0,100000)") # 执行拖动滚动条操作
    time.sleep(3)
    check_height = driver.execute_script("return document.body.scrollHeight;")
    if check_height == all_window_height[-1]:  #判断拖动滚动条后的最大高度与上一次的最大高度的大小，相等表明到了最底部
        break
    else:
        all_window_height.append(check_height)
--------------------- 
作者：ball23god 
来源：CSDN 
原文：https://blog.csdn.net/weixin_40718824/article/details/84196233 
版权声明：本文为博主原创文章，转载请附上博文链接！
```
