# 批量下载QQ邮箱附件，下载完后修改文件重命名

因为工作原因，需要处理QQ邮箱上来自各地网友的投稿附件。数量比较多，如果手动一个一个下载很麻烦，而且有些发来的附件命名也不规范，下载下来之后还需要手动去重命名，否则放一起就分不清谁是谁了。这种非常机械化的重复操作，我想写个脚本批量下载QQ邮箱附件。

于是临时研究了一下 Python + selenium + Chrome 来节省很多时间~

---
  
<br>

## 如何安装

- **Python 3**   
  https://www.python.org/
- **WebDriver for Chrome**   
  https://sites.google.com/a/chromium.org/chromedriver/downloads
- **selenium**
  ```
  pip install selenium
  ```

<br>
 
## 如何使用


### 1 第一次使用时

初次启用脚本需手动登陆，勾选**下次自动登陆** （记住密码）
以后再启用脚本时就可以实现自动登陆直接进入邮箱主页了。

不嫌麻烦，也可以先运行下面这段脚本来登陆

``` python
from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument("user-data-dir=selenium")
chrome = webdriver.Chrome(options=options)

url = 'https://mail.qq.com/'
chrome.get(url)

print('登陆成功！')
```
  
  
> 注：通过运行脚本启动的浏览器窗口，只能同时启动一个。若重复启动脚本将会打开空页面，需要关闭上一个窗口重新运行脚本。
<br>   

### 2 调整每页显示邮件数量 
邮箱默认只显示25条邮件，需要在邮箱设置里，调整每页显示**100**封邮件。

<br>  

### 3 邮箱文件夹
把你想要下载的邮件**移动到**文件夹里，方便区分。
  
<br>
  
### 4 新窗口打开文件夹
从邮箱左侧的面板‘我的文件夹’中找到你刚刚创建的文件夹，**右键-新窗口打开**
  
从浏览器的地址栏找到链接的几个参数: `sid` `folderid` `page`  
```
https://mail.qq.com/cgi-bin/frame_html?t=frame_html&sid={ A }&url=/cgi-bin/mail_list?folderid={ B }%26page={ C }
```
  
<br>
  
### 5 修改脚本里面的自定义参数

---
  
## 特性
1. 自定义文件下载路径夹
2. 自动翻页
3. 如果邮件中没有包含附件，会自动打星标。
4. 脚本结束后会生成一个批处理程序，运行后会自动为附件重命名。命名规则是在文件名前面添加'发件人邮箱'
5.~~可以自定义脚本从第几封邮件开始，到第几封邮件结束，或者只处理多少封邮件。~~
（实现自动翻页后，定位功能失效了。如果你需要使用这个功能，可以从/old/文件夹里使用上个版本的脚本）
  
<br>

## 可能出现的问题

1. 如果网络不稳定。附件的预览图如果没有加载出来，脚本可能会卡住。  
  （已修复。解决方案：以不加载图片的模式启动浏览器）
 
2. 本地重命名的批处理脚本，如果附件有重复的文件，后面的相同的文件不会被重命名。  
  （没想好。临时解决方案：根据控制台输出的记录，搜索文件名，找到发件人昵称或者邮箱，手动重命名）

3. 如果开车的速度过快，会被系统拦下车，提示：【您请求的频率太快，请稍后再试】
  （没写完。临时解决方案：根据控制台输出的记录，从暂停的地方重新下载）
  
<br>
  
## 踩坑历史
1. 附件收藏中的"全部附件"，并不是想象中真的把全部附件整合在一起，还是会漏掉一些，关键是还不知道漏了哪些。
  （解决方案：换成进入邮件主题下载附件）

