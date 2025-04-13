# Python 爬虫指南

## 一、基础知识

### 1. Python 基础
- 学习 Python 语法、数据结构（列表、字典、集合等）、函数、类和对象等基础知识。
- 推荐资源：
  - [Python 官方文档](https://docs.python.org/zh-cn/3/)
  - [廖雪峰的 Python 教程](https://www.liaoxuefeng.com/wiki/1016959663602400)
  - [Python3 教程 | 菜鸟教程](https://www.runoob.com/python3/python3-tutorial.html)

### 2. 网络基础
- 了解 HTTP 协议、请求方法（GET、POST）、状态码等。
- 学习如何查看网页源代码和开发者工具（Chrome/Firefox）。
- 推荐资源：
  - [HTTP 协议详解](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

## 二、爬虫工具入门

### 1. Requests 库
- 学习如何使用 Requests 发送 HTTP 请求，获取网页内容。
- 示例代码：
  ```python
  import requests
  response = requests.get('https://example.com')
  print(response.text)
  ```

### 2. BeautifulSoup 库
- 学习如何解析 HTML 页面，提取数据。
- 示例代码：
  ```python
  from bs4 import BeautifulSoup
  soup = BeautifulSoup(html_content, 'html.parser')
  titles = soup.find('h1')
  ```

### 3. 实践项目
- 尝试爬取一个简单的静态网页，比如获取某网站的标题、链接等。

## 三、进阶爬虫技术

### 1. 动态网页爬取
- 学习如何处理 JavaScript 渲染的网页，使用 Selenium 或 Playwright。
- 示例代码（Selenium）：
  ```python
  from selenium import webdriver
  driver = webdriver.Chrome()
  driver.get('https://example.com')
  print(driver.page_source)
  driver.quit()
  ```

### 2. 数据存储
- 学习如何将爬取的数据存储到文件（CSV、JSON）或数据库（MySQL、MongoDB）。
- 示例代码（存储到 CSV）：
    ```python
    import csv
    import os
    
    # 个人习惯,不喜欢定义字段,从接口返回的数据直接  {**fields}
    def xieru(csv_file, fields):
        # 处理 fields 字典
        fields = {
            key: (str(value) if value is not None else '')
            for key, value in fields.items()
        }
        file_exists = os.path.exists(csv_file) and os.path.getsize(csv_file) > 0
        with open(csv_file, mode='a', newline='', encoding='utf-8-sig') as file:
            writer = csv.DictWriter(file, fieldnames=fields.keys())
            # 如果文件不存在或为空，写入表头
            if not file_exists:
                writer.writeheader()  # 写入表头
                writer.writerows([fields])  # 写入数据
    ```

### 3.AI使用

```python
from openai import OpenAI


text = """返回json,严格遵守格式{................}"""

class DeepSeekClient:
    def __init__(self, api_key='', base_url="https://api.deepseek.com"):
        self.client = OpenAI(
            api_key=api_key,
            base_url=base_url,
        )
	def get_response(self, user_prompt, model="deepseek-chat"):
        messages = [{"role": "system",
                     "content": text},
                    {"role": "user", "content": user_prompt}]
        response = self.client.chat.completions.create(
            model=model,
            messages=messages,
            response_format={
                'type': 'json_object'
            }
        )
        return json.loads(response.choices[0].message.content)
```

### 4. 反爬虫应对

- 学习如何处理常见的反爬虫机制，比如 IP 限制、验证码、User-Agent 限制等。
- 推荐工具：
  - 使用代理 IP	[快代理 - 企业级HTTP代理IP云服务_专注IP代理11年](https://www.kuaidaili.com/)
  - 模拟浏览器行为（设置 headers、随机延时）

## 四、框架与工具

### 1. Scrapy 框架
- [🛰️ 概述 | DrissionPage官网](https://www.drissionpage.cn/browser_control/intro)
- 示例代码(最常用)：
  ```python
  from DrissionPage import Chromium
  
  tab = Chromium().latest_tab
  tab.get('https://gitee.com/explore/all')  # 访问网址，这行产生的数据包不监听
  
  tab.listen.start('gitee.com/explore')  # 开始监听，指定获取包含该文本的数据包
  for _ in range(5):
      tab('@rel=next').click()  # 点击下一页
      res = tab.listen.wait()  # 等待并获取一个数据包
      print(res.url)  # 打印数据包url
  ```

### 2.快速工具

[带带弟弟ocr验证码](https://github.com/sml2h3/ddddocr)  验证码识别

[爬虫工具库](https://spidertools.cn/#/)     在线工具库

[Reqable ](https://reqable.com/zh-CN/)	新一代API开发工具

[DrissionPage官网](https://www.drissionpage.cn/)   自动化模拟浏览器,过检测

[超级鹰验证码识别](https://www.chaojiying.com/)

[Moonshot AI](https://platform.moonshot.cn/docs/)

[DeepSeek 开放平台](https://platform.deepseek.com/usage)



### 3. 开源项目参考

- 参考 GitHub 上的优秀爬虫项目，学习代码结构和最佳实践。
- 推荐项目：
  - [Scrapy 官方示例](https://github.com/scrapy/scrapy)
  - [Python 爬虫实战](https://github.com/Python3WebSpider)
  - [NanmiCoder/MediaCrawler: 小红书笔记 | 评论爬虫、抖音视频 | 评论爬虫、快手视频 | 评论爬虫、B 站视频 ｜ 评论爬虫、微博帖子 ｜ 评论爬虫、百度贴吧帖子 ｜ 百度贴吧评论回复爬虫 | 知乎问答文章｜评论爬虫](https://github.com/NanmiCoder/MediaCrawler)
  - [wistbean/learn_python3_spider： python爬虫教程系列、从0到1学习python爬虫，包括浏览器抓包，手机APP抓包，如 fiddler、mitmproxy，各种爬虫涉及的模块的使用，如：requests、beautifulSoup、selenium、appium、scrapy等，以及IP代理，验证码识别，Mysql，MongoDB数据库的python使用，多线程多进程爬虫的使用，css 爬虫加密逆向破解，JS爬虫逆向，分布式爬虫，爬虫项目实战实例等](https://github.com/wistbean/learn_python3_spider)

## 五、法律与伦理

- 了解爬虫的法律风险，遵守网站的 `robots.txt` 文件。
- 不爬取敏感数据，避免对目标网站造成过大负载。

## 六、学习资源推荐

### 在线教程
- [莫烦 Python - 爬虫教程](https://morvanzhou.github.io/tutorials/data-manipulation/scraping/)
- 

### 书籍
- [码农书籍网](https://www.manongbook.com/)
- 

## 七、实践项目建议

1. [笔趣阁](https://www.bqgh.com/)

2. [淘宝](https://www.taobao.com/)

3. [京东](https://www.jd.com/)

4. [起点中文网](https://www.qidian.com/)

5. [阿里资产](https://z.taobao.com/)

6. [国家专利检索](https://pss-system.cponline.cnipa.gov.cn/conventionalSearch)

7. [国家法律法规数据库](https://flk.npc.gov.cn/index.html)

8. [90图书馆](http://www.90tsg.com/)

9. [中药材价格_市场价格 - 康美中药网](https://www.kmzyw.com.cn/jiage/today_price.html?pageNum=1)

10. 大学

11. [天眼查](https://www.tianyancha.com/)

12. [企查查](https://www.qcc.com/)

13. [爱企查](https://aiqicha.baidu.com/?fl=1&castk=LTE=)

14. 抖音,快手,小红书,豆瓣

15. 微信公众号

16. [中国空气质量在线监测分析平台](https://www.aqistudy.cn/)

17. 网易云

18. 翻译

19. [裁判文书](https://wenshu.court.gov.cn/)

20. Boss直聘

21. 文件下载

22. [中国石油招标](https://www.cnpcbidding.com/)

23. 美团,饿了么

    
