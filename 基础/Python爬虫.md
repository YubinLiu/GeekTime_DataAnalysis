### Python 爬虫
用浏览器访问的方式模拟了访问网站的过程，包括：打开网页、提取数据、保存数据。
***
#### Requests 访问页面
Python HTTP 的客户端库。
  * GET：把参数包含在 url 中。通过 r.text 或 r.content 获取 HTML 正文。
  ```
  r = requests.get("http://www.douban.com")
  ```
  * POST：通过 request body 传递参数。data 是表单参数，字典结构。
  ```
  r = request.post("http://xxx.com", data={"key":"value"})
  ```
***
#### Xpath 定位
通过属性和元素进行导航。
  * node：选 node 节点的所有节点。
    * xpath('node')，选取了 node 节点的所有子节点。

  * /：从根节点选取。
    * xpath('/div')，从根节点上选择 div 节点。

  * //：选取所有的当前节点，不考虑他们的位置。
    * xapth('//div')，选取所有的 div 节点。

  * .：当前节点。
    * xpath('./div')，当前节点下的 div 节点。

  * ..：父节点。
    * xpath('...')，回到上一个节点。

  * @：属性选择。
    * xpath('//@id')，获取所有的 id 属性。
    * xpath('//book[@id='abc']')，获取所有 book 元素，且这些 book 元素拥有 id=abc 的属性。

  * |：或，两个节点的合计。
    * xpath('//book/title | //book/price')，选取 book 元素所有 title 和 price 元素。

  * text()：当前路径下的文本内容。

#### JSON 对象
* json.dumps()：将 Python 对象转换成 JSON 对象。
* json.loads()：将 JSON 对象转换为 Python 对象。

#### Selenium 库
Web 应用的测试工具，可以直接运行在浏览器中，原理是模拟用户在进行操作。
* 例
```Python
from selenium import webdriver
driver = webdriver.Chrome()
driver.get(request_url)
```
