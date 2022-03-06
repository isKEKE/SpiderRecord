# Selenium<学习笔记

## 一、安装镜像网站

> [chrome浏览器历史版本下载](https://www.chromedownloads.net/)
> [chromedriver.exe-浏览器驱动-国内镜像下载](http://chromedriver.storage.googleapis.com/index.html)

## 二、简易测试

> 本笔记观看建议配合崔庆才大佬《Python3网络爬虫开发实战》第二版一起。

```python
# _*_ coding: utf-8 _*_
from selenium import webdriver
from selenium.webdriver.chrome.service import Service

'''
高版本路径载入会有个WARNING, 高版本载入建议使用Service对象。
'''
# 导入服务实例, 我的chromedriver.exe在本地的F盘
service = Service("F://chromedriver.exe")
# chrome浏览器实例
driver = webdriver.Chrome(service=service)
# 浏览器最大化
browser.maximize_window()
# 访问百度
driver.get("https://www.baidu.com")
input("quit?>")
# 关闭驱动器及相关服务, 建议编写都要加这个，养成良好习惯，对资源进行回收释放。
driver.quit()
```
![image-20220305174258472](https://raw.githubusercontent.com/isKEKE/SpiderRecord/main/Selenium/image-20220305174258472.png)

## 三、模拟操作

#### 3.1 文本框输入

```python
# _*_ coding: utf-8 _*_
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

# 导入服务实例
service = Service("F://chromedriver.exe")
# chrome驱动器实例
browser = webdriver.Chrome(service=service)
try:
    # 访问百度
    browser.get("https://www.baidu.com")
    # 查找节点，锁定文本输入框; 高版本建议使用find_element方法
    input_element = browser.find_element(By.ID, "kw")
    # 可以清楚旧内容
    input_element.clear()
    input_element.send_keys("名侦探柯南")
    input("quit?>")
finally:
    # 关闭驱动器及相关服务, 建议编写都要加这个，养成良好习惯，对资源进行回收释放。
    browser.quit()

```

#### 3.2 模拟点击

```python
# _*_ coding: utf-8 _*_
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

# 导入服务实例
service = Service("F://chromedriver.exe")
# chrome驱动器实例
browser = webdriver.Chrome(service=service)
try:
    # 访问百度
    browser.get("https://www.baidu.com")
    # 查找节点，文本输入框; 高版本建议使用find_element方法
    input_element = browser.find_element(By.ID, "kw")
    # 可以清楚旧内容
    input_element.clear()
    # 填写内容
    input_element.send_keys("名侦探柯南")
    # 查找百度一下按钮节点
    submit_btn = browser.find_element(By.ID, "su")
    # 模拟点击
    submit_btn.click()
    input("quit?>")
finally:
    # 关闭驱动器及相关服务, 建议编写都要加这个，养成良好习惯，对资源进行回收释放。
    browser.quit()
```

#### 3.3 滑动条

> 目标网站: https://spa1.scrape.center/

```python
# _*_ coding: utf-8 _*_
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service

# 导入服务实例
service = Service("F://chromedriver.exe")
# chrome驱动器实例
browser = webdriver.Chrome(service=service)
try:
    browser.get("https://spa1.scrape.center/")
    # 网站响应慢，这里进行强制等待
    time.sleep(3)
    # 通过js移动滑动条, 滑动条到最底部; document.documentElement.scrollTop
    browser.execute_script("document.documentElement.scrollTop=10000")
    input("quit?>")
finally:
    # 关闭驱动器及相关服务, 建议编写都要加这个，养成良好习惯，对资源进行回收释放。
    browser.quit()
```

这里只是最简单的移动滑动条，有些滑动条是内置的，需要自己find_element找到节点对象然后再操作。

#### 3.4 动作链

```python
# _*_ coding: utf-8 _*_
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import ActionChains

# 导入服务实例
service = Service("F://chromedriver.exe")
# chrome驱动器实例
browser = webdriver.Chrome(service=service)
browser.maximize_window()
try:
    browser.get("https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable")
    # 切换iframe内联框架，因为操作节点在内联框架中，
    browser.switch_to.frame("iframeResult")
    # 选择节点
    draggable = browser.find_element("id", "draggable")
    droppable = browser.find_element("id", "droppable")
    # 动作链对象
    actions = ActionChains(browser)
    # 拖拽方法，拖拽对象>拖拽目标
    actions.drag_and_drop(draggable, droppable)
    # 执行动作
    actions.perform()
    input("quit?>")
finally:
    # 关闭驱动器及相关服务, 建议编写都要加这个，养成良好习惯，对资源进行回收释放。
    browser.quit()
```

## 四、提取数据

> 目标网站：https://spa2.scrape.center/

#### 4.1 提取节点属性内容

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

service = Service("F:\\chromedriver.exe")
browser = Chrome(service=service)
browser.maximize_window()
# 隐式等待
browser.implicitly_wait(10)
try:
    browser.get("https://spa2.scrape.center/")
    # 查询节点
    element = browser.find_element(By.CSS_SELECTOR, ".name")
    print(element, type(element))
    # 获得节点属性
    print(element.get_attribute("href"))
finally:
    browser.quit()
```

#### 4.2 提取节点文本内容

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

service = Service("F:\\chromedriver.exe")
browser = Chrome(service=service)
browser.maximize_window()
# 隐式等待
browser.implicitly_wait(10)
try:
    browser.get("https://spa2.scrape.center/")
    # 查询节点
    element = browser.find_element(By.CSS_SELECTOR, ".name")
    print(element, type(element))
    # 获得节点文本
    print(element.text)
finally:
    browser.quit()
```

#### 4.3 获得ID、位置、标签名和大小

```
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

service = Service("F:\\chromedriver.exe")
browser = Chrome(service=service)
browser.maximize_window()
# 隐式等待
browser.implicitly_wait(10)
try:
    browser.get("https://spa2.scrape.center/")
    # 查询节点
    element = browser.find_element(By.CSS_SELECTOR, ".name")
    # 获得ID、位置、标签名和大小
    print("ID => ", element.id)
    print("位置 => ", element.location)
    print("标签名 => ", element.tag_name)
    print("大小 => ", element.size)
finally:
    browser.quit()
```

#### 4.4 使用`page_source`+`parsel`解析

> browser.page_source 此方法就是浏览器动态加载到内存中的页面数据，所以可以会出现爬取不完整情况，为什么？因为浏览器加载也需要时间，但脚本执行此方法是立即执行，所以此解析方式慎用或添加显示等待某节点增加容错。

```python
# _*_ coding: utf-8 _*_
import parsel
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

service = Service("F:\\chromedriver.exe")
browser = Chrome(service=service)
browser.maximize_window()
try:
    browser.get("https://spa2.scrape.center/")
    # 查询节点, 显示等待
    wait = WebDriverWait(driver=browser, timeout=5)
    # EC.presence_of_element_located => 表示节点出现，否则一次TimeoutException
    try:
        element = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, ".name")))
    except TimeoutException as exc:
        print(f"error => {type(exc)}: {exc.msg}")
    else:
        # 解析当前内存中页面数据
        selector = parsel.Selector(browser.page_source)
        # 获得电影信息
        for item in selector.xpath('''//div[@class="el-card__body"]'''):
            # 这里就不继续解析了
            print(item.getall())
finally:
    browser.quit()
```

## 五、切换Frame

> 必须掌握的知识，因很多页面会出现嵌套页面。

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

def main() -> int:
    '''运行'''
    url = "https://www.runoob.com/try/try.php?filename=jqueryui-api-droppable"
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    browser.maximize_window()
    browser.implicitly_wait(10)
    try:
        browser.get(url)
        # 最外层切换到frame id为iframeResult子页面
        browser.switch_to.frame("iframeResult")
        # 获得当前页面中文本内容为`请拖拽我！`节点对象
        element = browser.find_element(By.XPATH, '''//div[contains(text(), "请拖拽我")]''')
        print(element, element.location)

        # 注意如果向获得外层页面的节点得切换出当前子页面
        browser.switch_to.parent_frame()
        element_logo = browser.find_element(By.TAG_NAME, "img")
        print(element_logo)
    finally:
        browser.quit()
    return 0


if __name__ == "__main__":
    main()
```

注意：**不可以跨层选取节点**。例如：我在子页面中，我想选父页面某图片节点，这是不可以的，得先切换到父页面才可以。

## 六、延迟等待

> 之前也说了, Selenium中get访问页面，有可能页面未加载完，而且立即执行之后功能会导致数据丢失！这时候就需要通过等待来保证节点等数据加载完成。

#### 6.1 隐式等待

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
service = Service("F:\\chromedriver.exe")
browser = Chrome(service=service)
browser.implicitly_wait(10)
browser.quit()
```

关键语句:`browser.implicitly_wait(10)`这句意思是隐式等待10s，即页面find_element某节点会查找等待10s，如果找到返回节点，如果未找到报异常没有到节点。好处是：先等待一段时间然后再差DOM树，默认的等待为0s。

#### 6.2 显示等待

隐式等待只规定了一个固定时间，可能收网络或硬件因素出现超时，所以使用显示等待更好！

| 方法名称  | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| until     | 参数mehod:回调方法，每过一段时间调用，直到返回值不是false，或超过最长等待时长异常。 |
| until_not | 和until方法相反，until是某元素出现或说明什么条件成立继续执行；而until_not是当某元素消失或什么条件不成立立即继续执行。参数相同。 |

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

def main() -> int:
    '''运行'''
    url = "https://www.taobao.com/"
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    try:
        browser.get(url)
        # WebDriverWait对象, timeout参数为等大最大时长s.
        web_wait = WebDriverWait(driver=browser, timeout=15)
        # presence_of_element_located 等待节点出现
        element_input = web_wait.until(EC.presence_of_element_located(
            (By.ID, "q")))
        element_serach_btn = web_wait.until(EC.presence_of_element_located(
            (By.CSS_SELECTOR, ".btn-search")
        ))
        element_input.send_keys("名侦探柯南")

        input("quit?>>>")
    finally:
        browser.quit()
    return 0


if __name__ == "__main__":
    main()
```

注意：**如果使用了隐式等待，那么显示等待最长等待时间将不会少于隐式等待的时长。**例如：隐式等待10s，而显示等待5s，那么等待时长取最高值为10s。所以不建议隐式和显示都使用。

#### 6.3 等待条件

| 等待条件                                   | 含义                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| **title_is**                               | 验证传入的参数title是否等于。                                |
| **presence_of_element_located**            | 只要一个符合条件的元素加载出来就通过,参数元组，**例如(By.ID, "q")。** |
| **presence_of_all_elements_located**       | 必须所有符合条件的元素都加载出来才行参数元组。               |
| **visibility_of_element_located**          | 节点是否可见，参数为元素。                                   |
| **visibility_of**                          | 节点是否可见，参数为节点对象。                               |
| **text_to_be_present_in_element**          | 判断某段文本是否出现在某元素中。                             |
| **text_to_be_present_in_element_value**    | 判断某段文本是否出现在某元素值中。                           |
| **frame_to_be_available_and_switch_to_it** | 判断frame是否可切入，传入元组或者定位到frame对象。           |
| **alert_is_present**                       | 判断是否有alert出现。                                        |
| **invisibility_of_element_located**        | 节点不可见，参数元组。                                       |
| **element_to_be_clickable**                | 判断按钮是否可点击，参数元组。                               |
| **element_to_be_selected**                 | 判断节点是否被选中，参数节点对象。                           |
| **element_located_to_be_selected**         | 判断节点是否被选中，参数元组。                               |
| **element_selection_state_to_be**          | 判断节点是否被选中，参数为节点对象以及状态，相对返回True，否则返回False。 |
| **element_located_selection_state_to_be**  | 判断节点是否被选中，参数为元组以及状态，相对返回True，否则返回False。 |
| **staleness_of**                           | 判断一个元素是否仍在DOM中, 可以判断页面是否刷新了, 参数节点对象。 |

## 七、前进+后退

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service


def main() -> int:
    '''运行'''
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    try:
        # 访问B站
        browser.get("https://www.bilibili.com/")
        # 访问百度
        browser.get("https://www.baidu.com/")
        # 后退
        browser.back()
        # 前进
        browser.forward()
    finally:
        browser.quit()

    return 0

if __name__ == "__main__":
    main()
```

## 八、处理Cookie

> 像登录操作，会用cookie进行保存会话，但我们重新启动脚本需要再次重新登录，那么我们就可以操作cookie来让重新启动脚本继续使用旧cookie而无需再登录一遍。

```python
# _*_ coding: utf-8 _*_
import os
import pickle
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 先修改账号和密码
USERNAME = None
PASSWORD = None

def login(browser: Chrome) -> None:
    '''登录'''
    browser.get("https://passport.bilibili.com/login?from_spm_id=333.1007.top_bar.login")
    if os.path.exists("cookies.pkl"):
        # 使用本地cookies
        cookies = pickle.load(open("cookies.pkl", "rb"))
        for cookie in cookies:
            browser.add_cookie(cookie)
        # 访问个人空间
        browser.get("https://space.bilibili.com/")

    else:
        web_wati = WebDriverWait(browser, 10)
        username = web_wati.until(EC.presence_of_element_located(
            (By.ID, "login-username")
        ))
        username.send_keys(USERNAME)

        password = web_wati.until(EC.presence_of_element_located(
            (By.ID, "login-passwd")
        ))
        password.send_keys(PASSWORD)

        login_btn = web_wati.until(EC.presence_of_element_located(
            (By.CSS_SELECTOR, "a.btn.btn-login")
        ))
        login_btn.click()

        # 判断
        while (not (browser.current_url == "https://www.bilibili.com/")):
            pass
        cookies = browser.get_cookies()
        with open("cookies.pkl", "wb") as fp:
            pickle.dump(cookies, fp)


def main() -> int:
    '''半自动的登录B站'''
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    browser.maximize_window()
    try:
        login(browser) # 登录
        input("quit?>")
    finally:
        browser.quit()
    return 0

if __name__ == "__main__":
    main()
```

## 九、选项卡切换

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.service import Service


def main() -> int:
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    browser.maximize_window()
    try:
        browser.get("https://www.bilibili.com/")
        # 这里操作是打开一个新选项卡
        browser.execute_script("window.open()")
        # 记录当前选项卡
        bilibili_tag = browser.current_window_handle
        # 切换选项卡
        browser.switch_to.window(browser.window_handles[-1])
        browser.get("https://www.baidu.com")
        # 切回B站选项卡
        browser.switch_to.window(bilibili_tag)
        input("quit?>")
    finally:
        browser.quit()


if __name__ == "__main__":
    main()
```

## 十一、异常处理

> 常用到，优秀的程序必须有异常处理进行容错

```python
# _*_ coding: utf-8 _*_
import logging
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import Chrome
from selenium.common.exceptions import NoSuchElementException


def main() -> int:
    '''这里拿百度测试'''
    url = "https://www.baidu.com/"
    service = Service("F:\\chromedriver.exe")
    browser = Chrome(service=service)
    browser.get(url)
    try:
        browser.find_element("id", "123")
    except NoSuchElementException as e:
        logging.error(f"Type => {type(e)}; Msg => {e.msg}.")
    finally:
        browser.quit()
    return 0


if __name__ == "__main__":
    main()
```

这里只是简单使用，而实战中往往和显示等待搭配使用更多。

## 十二、反屏蔽

> 这个技巧很重要，现在很多网站会检测selenium导致你爬取失败。

- 一般检测的是`window.navigator`对象`webdriver`属性。我们正常浏览器此属性是undefined，但selenium会设置此属性。当然如果你觉得通过js代码执行来掩盖这个属性，那就大错特错！你想get网页都已经加载当前页面了，你再excute_script执行改掉此属性，这不掩耳盗铃吗！
- 解决办法：Selenium CDP(ChromeDevTools协议简称CDP), 它的作用就是页面每次加载时候会自动执行设置好的js语句，那么我们只需要添加自定义的cdp协议就可以解决这个问题

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import Chrome
from selenium.webdriver import ChromeOptions


def main() -> int:
    url = "https://antispider1.scrape.center/"
    service = Service("F:\\chromedriver.exe")
    # chrome 配置
    chrome_options = ChromeOptions()
    # 隐藏提示信息
    chrome_options.add_experimental_option("excludeSwitches", ["enable-automation"])
    chrome_options.add_experimental_option("useAutomationExtension", False)
    # 浏览器对象
    browser = Chrome(service=service, options=chrome_options)
    browser.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument",
        {"source": "Object.defineProperty(navigator, 'webdriver', {get: () => undefined})"})
    try:
        browser.get(url)
        input("quit?>")
    finally:
        browser.quit()
    return 0


if __name__ == "__main__":
    main()
```

## 十三、无头模式

> selenium脚本每次都会打开N个web ui，很碍事，所以可以启动无头模式进行隐藏

```python
# _*_ coding: utf-8 _*_
from selenium.webdriver import Chrome
from selenium.webdriver import ChromeOptions
from selenium.webdriver.chrome.service import Service


def main() -> int:
    '''拿B站热播进行测试'''
    url = "https://www.bilibili.com/v/popular/all?spm_id_from=333.1007.0.0"
    service = Service("F:\\chromedriver.exe")
    chrome_options = ChromeOptions()
    # 无头模式
    chrome_options.add_argument("--headless")
    browser = Chrome(service=service, options=chrome_options)
    browser.implicitly_wait(5)
    browser.maximize_window()
    
    try:
        browser.get(url)
        elements = browser.find_elements(
            "xpath", '''//div[contains(@class, "video-card__info")]''')
        for item in elements:
            title = item.find_element("xpath", "./p").get_attribute("title")
            print("title => ", title)

    finally:
        browser.quit()
    return 0

if __name__ == "__main__":
    main()
```

## 十四、骚操作

> 有时候某网页不只是检测`window.navigator`对象，但是我又很小白咋办？来了：通过自定义端口的方式进行自动化！什么意思？就是我开启一个正常的chrome浏览器，然后我开个端口，我让selenium通过这个端口去操作打开的正常chrome浏览器。

```python
# _*_ coding: utf-8 _*_
import os
import subprocess
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import Chrome
from selenium.webdriver import ChromeOptions

# chrome.exe文件绝对路径
CHROME_PATH = r"C:\Users\86131\AppData\Local\Google\Chrome\Application\chrome.exe"
# 自定义端口方式chrome浏览器用户数据存储位置，防止覆盖自己的源数据
CHROME_INFO_PATH = r"F:\chrome-data"
# 开启端口
CHROME_PORT = 9222
# 命令
COMMANDS = [CHROME_PATH,
            f"--remote-debugging-port={CHROME_PORT}",
            f'--user-data-dir={CHROME_INFO_PATH}']

def main() -> int:
    # 打开浏览器
    pipe = subprocess.Popen(" ".join(COMMANDS), shell=False)
    url = "https://antispider1.scrape.center/"
    service = Service("F:\\chromedriver.exe")
    # chrome配置
    chrome_options = ChromeOptions()
    # 监听本地
    chrome_options.add_experimental_option(
        "debuggerAddress", f"127.0.0.1:{CHROME_PORT}")
    # 浏览器对象
    browser = Chrome(service=service, options=chrome_options)
    try:
        browser.get(url)
        input("quit?>")
    finally:
        browser.quit()
        pipe.kill()
    return 0

if __name__ == "__main__":
    main()
```

## 十五、作者声明

- 这个笔记只是简单对selenium一些整合，还有一些进阶东西并没有出现在这里，它根本目的是让我写脚本时忘记方便进行查找。
- Github: https://github.com/isKEKE/SpiderRecord/tree/main/Selenium