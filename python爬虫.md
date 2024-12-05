

## python爬虫

原链接：https://cuiqingcai.com/17777.html

### 爬虫基础

#### HTTP基本原理

URL的基本组成：

```
scheme://[username:password@]hostname[:port][/path][;parameters][?query][#fragment]
```

- 用户在浏览器中输入URL，向服务器发送了一个请求。
- 服务器接受请求后开始处理并解析，返回对应的响应。
- 浏览器再对响应进行解析，即可得到相关的网页。

#### 请求（Request）

请求可分为四部分：请求方法、请求网址、请求头、请求体。

**请求方法**：用于标识客户端请求服务端的方式，常见的有GET和POST。

在浏览器中直接输入URL便是发起了一个GET请求，而像登陆等提交表单的操作通常就是发起了POST请求。

GET与POST的区别：

- GET请求的参数会包含在URL中，而POST请求的数据都是通过表单传输，包含在请求体中。
- GET提交的数据最多1024字节，POST则不受限。

以下是一些请求方法：

| 方　　法 | 描　　述                                                     |
| :------- | :----------------------------------------------------------- |
| GET      | 请求页面，并返回页面内容                                     |
| HEAD     | 类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| POST     | 大多用于提交表单或上传文件，数据包含在请求体中               |
| PUT      | 从客户端向服务器传送的数据取代指定文档中的内容               |
| DELETE   | 请求服务器删除指定的页面                                     |
| CONNECT  | 把服务器当作跳板，让服务器代替客户端访问其他网页             |
| OPTIONS  | 允许客户端查看服务器的性能                                   |
| TRACE    | 回显服务器收到的请求，主要用于测试或诊断                     |

**请求网址**：即URL，唯一确认需要请求的资源。

**请求头**：用来说明给服务器使用的附件信息，包括Cookie、Referer、User-Agent等。

以下是一些常用的头信息：

- Accept：请求报头域，用于指定客户端可接受哪些类型的信息。
- Accept-Language：指定客户端可接受的语言类型。
- Accept-Encoding：指定客户端可接受的内容编码。
- Host：用于指定请求资源的主机 IP 和端口号，其内容为请求 URL 的原始服务器或网关的位置。
- Cookie：网站为了辨别用户进行会话跟踪而存储在用户本地的数据。它的主要功能是维持当前访问会话。
- Referer：此内容用来标识这个请求是从哪个页面发过来的，服务器可以拿到这一信息并做相应的处理，如做来源统计、防盗链处理等。
- User-Agent：简称 UA，可以使服务器识别客户使用的操作系统及版本、浏览器及版本等信息。在做爬虫时加上此信息，可以伪装为浏览器。
- Content-Type：也叫互联网媒体类型（Internet Media Type）或者 MIME 类型，在 HTTP 协议消息头中，它用来表示具体请求中的媒体类型信息。

注意，对于POST请求需要在请求头中指定Content-Type为application/x-www-form-urlencoded才会以表单提交。

| Content-Type                      | 提交数据的方式   |
| :-------------------------------- | :--------------- |
| application/x-www-form-urlencoded | 表单数据         |
| multipart/form-data               | 表单文件上传     |
| application/json                  | 序列化 JSON 数据 |
| text/xml                          | XML 数据         |

**请求体**：一般承载的是POST请求中的表单数据，对于GET，请求体为空。

#### 响应（Response）

响应由服务器返回客户端，分为三部分：响应状态码、响应头、响应体。

**响应状态码**：表示服务器响应状态，常见的错误代码及原因如下：



| 状态码 | 说　　明        | 详　　情                                                     |
| :----- | :-------------- | :----------------------------------------------------------- |
| 100    | 继续            | 请求者应当继续提出请求。服务器已收到请求的一部分，正在等待其余部分 |
| 101    | 切换协议        | 请求者已要求服务器切换协议，服务器已确认并准备切换           |
| 200    | 成功            | 服务器已成功处理了请求                                       |
| 201    | 已创建          | 请求成功并且服务器创建了新的资源                             |
| 202    | 已接受          | 服务器已接受请求，但尚未处理                                 |
| 203    | 非授权信息      | 服务器已成功处理了请求，但返回的信息可能来自另一个源         |
| 204    | 无内容          | 服务器成功处理了请求，但没有返回任何内容                     |
| 205    | 重置内容        | 服务器成功处理了请求，内容被重置                             |
| 206    | 部分内容        | 服务器成功处理了部分请求                                     |
| 300    | 多种选择        | 针对请求，服务器可执行多种操作                               |
| 301    | 永久移动        | 请求的网页已永久移动到新位置，即永久重定向                   |
| 302    | 临时移动        | 请求的网页暂时跳转到其他页面，即暂时重定向                   |
| 303    | 查看其他位置    | 如果原来的请求是 POST，重定向目标文档应该通过 GET 提取       |
| 304    | 未修改          | 此次请求返回的网页未修改，继续使用上次的资源                 |
| 305    | 使用代理        | 请求者应该使用代理访问该网页                                 |
| 307    | 临时重定向      | 请求的资源临时从其他位置响应                                 |
| 400    | 错误请求        | 服务器无法解析该请求                                         |
| 401    | 未授权          | 请求没有进行身份验证或验证未通过                             |
| 403    | 禁止访问        | 服务器拒绝此请求                                             |
| 404    | 未找到          | 服务器找不到请求的网页                                       |
| 405    | 方法禁用        | 服务器禁用了请求中指定的方法                                 |
| 406    | 不接受          | 无法使用请求的内容响应请求的网页                             |
| 407    | 需要代理授权    | 请求者需要使用代理授权                                       |
| 408    | 请求超时        | 服务器请求超时                                               |
| 409    | 冲突            | 服务器在完成请求时发生冲突                                   |
| 410    | 已删除          | 请求的资源已永久删除                                         |
| 411    | 需要有效长度    | 服务器不接受不含有效内容长度标头字段的请求                   |
| 412    | 未满足前提条件  | 服务器未满足请求者在请求中设置的其中一个前提条件             |
| 413    | 请求实体过大    | 请求实体过大，超出服务器的处理能力                           |
| 414    | 请求 URI 过长   | 请求网址过长，服务器无法处理                                 |
| 415    | 不支持类型      | 请求格式不被请求页面支持                                     |
| 416    | 请求范围不符    | 页面无法提供请求的范围                                       |
| 417    | 未满足期望值    | 服务器未满足期望请求标头字段的要求                           |
| 500    | 服务器内部错误  | 服务器遇到错误，无法完成请求                                 |
| 501    | 未实现          | 服务器不具备完成请求的功能                                   |
| 502    | 错误网关        | 服务器作为网关或代理，从上游服务器收到无效响应               |
| 503    | 服务不可用      | 服务器目前无法使用                                           |
| 504    | 网关超时        | 服务器作为网关或代理，但是没有及时从上游服务器收到请求       |
| 505    | HTTP 版本不支持 | 服务器不支持请求中所用的 HTTP 协议版本                       |

**响应头**：包含服务器对请求的应答信息，如 Content-Type、Server、Set-Cookie 等。

- Date：标识响应产生的时间。
- Last-Modified：指定资源的最后修改时间。
- Content-Encoding：指定响应内容的编码。
- Server：包含服务器的信息，比如名称、版本号等。
- Content-Type：文档类型，指定返回的数据类型是什么。
- Set-Cookie：设置 Cookie。响应头中的 Set-Cookie 告诉浏览器需要将此内容放在 Cookie 中，下次请求携带 Cookie 请求。
- Expires：指定响应的过期时间，可以使代理服务器或浏览器将加载的内容更新到缓存中。

**响应体**：响应的正文数据。

### Web网页基础

#### 节点树及节点间的关系

在HTML中，所有标签定义的内容都是节点，它们构成了一个节点树，也称为HTML DOM树。

结构如下：

![节点树](./images/5fe0q.jpg)

*通过 HTML DOM，树中的所有节点均可通过 JavaScript 访问，所有 HTML 节点元素均可被修改，也可以被创建或删除。*

#### 选择器

CSS选择器根据不同的节点设置不同的样式规则。例如使用`#`定位id，使用`.`定位class，或直接使用标签名定位。

| 选　择　器             | 例　　子             | 例子描述                                            |
| :--------------------- | :------------------- | :-------------------------------------------------- |
| `.class`               | `.intro`             | 选择 `class="intro"` 的所有节点                     |
| `#id`                  | `#firstname`         | 选择 `id="firstname"` 的所有节点                    |
| `*`                    | `*`                  | 选择所有节点                                        |
| `element`              | `p`                  | 选择所有 `p` 节点                                   |
| `element,element`      | `div,p`              | 选择所有 `div` 节点和所有 `p` 节点                  |
| `element element`      | `div p`              | 选择 `div` 节点内部的所有 `p` 节点                  |
| `element>element`      | `div>p`              | 选择父节点为 `div` 节点的所有 `p` 节点              |
| `element+element`      | `div+p`              | 选择紧接在 `div` 节点之后的所有 `p` 节点            |
| `[attribute]`          | `[target]`           | 选择带有 `target` 属性的所有节点                    |
| `[attribute=value]`    | `[target=blank]`     | 选择 `target="blank"` 的所有节点                    |
| `[attribute~=value]`   | `[title~=flower]`    | 选择 `title` 属性包含单词 `flower` 的所有节点       |
| `:link`                | `a:link`             | 选择所有未被访问的链接                              |
| `:visited`             | `a:visited`          | 选择所有已被访问的链接                              |
| `:active`              | `a:active`           | 选择活动链接                                        |
| `:hover`               | `a:hover`            | 选择鼠标指针位于其上的链接                          |
| `:focus`               | `input:focus`        | 选择获得焦点的 `input` 节点                         |
| `:first-letter`        | `p:first-letter`     | 选择每个 `p` 节点的首字母                           |
| `:first-line`          | `p:first-line`       | 选择每个 `p` 节点的首行                             |
| `:first-child`         | `p:first-child`      | 选择属于父节点的第一个子节点的所有 `p` 节点         |
| `:before`              | `p:before`           | 在每个 `p` 节点的内容之前插入内容                   |
| `:after`               | `p:after`            | 在每个 `p` 节点的内容之后插入内容                   |
| `:lang(language)`      | `p:lang`             | 选择带有以 `it` 开头的 `lang` 属性值的所有 `p` 节点 |
| `element1~element2`    | `p~ul`               | 选择前面有 `p` 节点的所有 `ul` 节点                 |
| `[attribute^=value]`   | `a[src^="https"]`    | 选择其 `src` 属性值以 `https` 开头的所有 `a` 节点   |
| `[attribute$=value]`   | `a[src$=".pdf"]`     | 选择其 `src` 属性以 .pdf 结尾的所有 `a` 节点        |
| `[attribute*=value]`   | `a[src*="abc"]`      | 选择其 `src` 属性中包含 `abc` 子串的所有 `a` 节点   |
| `:first-of-type`       | `p:first-of-type`    | 选择属于其父节点的首个 `p` 节点的所有 `p` 节点      |
| `:last-of-type`        | `p:last-of-type`     | 选择属于其父节点的最后一个 `p` 节点的所有 `p` 节点  |
| `:only-of-type`        | `p:only-of-type`     | 选择属于其父节点唯一的 `p` 节点的所有 `p` 节点      |
| `:only-child`          | `p:only-child`       | 选择属于其父节点的唯一子节点的所有 `p` 节点         |
| `:nth-child(n)`        | `p:nth-child`        | 选择属于其父节点的第二个子节点的所有 `p` 节点       |
| `:nth-last-child(n)`   | `p:nth-last-child`   | 同上，从最后一个子节点开始计数                      |
| `:nth-of-type(n)`      | `p:nth-of-type`      | 选择属于其父节点第二个 `p` 节点的所有 `p` 节点      |
| `:nth-last-of-type(n)` | `p:nth-last-of-type` | 同上，但是从最后一个子节点开始计数                  |
| `:last-child`          | `p:last-child`       | 选择属于其父节点最后一个子节点的所有 `p` 节点       |
| `:root`                | `:root`              | 选择文档的根节点                                    |
| `:empty`               | `p:empty`            | 选择没有子节点的所有 `p` 节点（包括文本节点）       |
| `:target`              | `#news:target`       | 选择当前活动的 `#news` 节点                         |
| `:enabled`             | `input:enabled`      | 选择每个启用的 `input` 节点                         |
| `:disabled`            | `input:disabled`     | 选择每个禁用的 `input` 节点                         |
| `:checked`             | `input:checked`      | 选择每个被选中的 `input` 节点                       |
| `:not(selector)`       | `:not`               | 选择非 `p` 节点的所有节点                           |
| `::selection`          | `::selection`        | 选择被用户选取的节点部分                            |

### Session和Cookie

#### Session

在Web中，Session对象用来存储特定用户会话所需的属性与配对信息。

#### Cookie

Cookie指网站为了辨别用户身份、进行Session跟踪而存储在用户本地终端上的数据。

当客户端第一次请求服务器时，服务器会返回一个响应头中带有 Set-Cookie 字段的响应给客户端，用来标记是哪一个用户，客户端浏览器会把 Cookies 保存起来。当浏览器下一次再请求该网站时，浏览器会把此 Cookies 放到请求头一起提交给服务器，Cookies 携带了 Session ID 信息，服务器检查该 Cookies 即可找到对应的 Session 是什么，然后再判断 Session 来辨认用户状态。

### urllib爬虫

urllib库中包含四个模块：

- request：最基本的HTTP请求模块，可以用来模拟发送请求。
- error：异常处理模块，如果出现请求错误，可以捕获这些异常，进行异常处理。
- parse：工具模块，提供了URL处理方法，比如拆分，解析和合并。
- robotparser：主要用来识别网站robots.txt，然后判断哪些网站可以爬，哪些不能爬。

#### 发送请求

使用request模块，可以方便地实现请求的发送并得到响应。

**urlopen**

urllib.request模块提供了最基本的构造HTTP请求的方法，利用它可以模拟浏览器的请求发起过程，同时它还带有处理授权验证、重定向、浏览器Cookie等内容。

```python
import urllib.request

response = urllib.request.urlopen('https://www.python.org')
print(response.read().decode('utf-8'))
```

其中，urlopen返回一个HTTPResponse类型的对象，主要包含read、readinto、getheader等方法及msg、version、status、reason等属性。得到这个对象后，可以调用以上方法，得到返回结果的一系列信息。*利用最基本的 `urlopen` 方法，可以完成最基本的简单网页的 GET 请求抓取。*

```python
urllib.request.urlopen(url, data=None, [timeout,]*, cafile=None, capath=None, cadefault=False, context=None)
```

**data参数：**

`data` 参数是可选的。如果要添加该参数，需要使用 `bytes` 方法将参数转化为字节流编码格式的内容，即 `bytes` 类型。如果，传递了这个参数，则请求方式就不再是GET方式，而是POST方式。

如：

```python
import urllib.parse
import urllib.request

data = bytes(urllib.parse.urlencode({'name': 'germey'}), encoding='utf-8')
response = urllib.request.urlopen('https://httpbin.org/post', data=data)
print(response.read().decode('utf-8'))
```

**timeout参数：**

`timeout` 参数用于设置超时时间，单位为秒，意思就是如果请求超出了设置的这个时间，还没有得到响应，就会抛出异常。如果不指定该参数，就会使用全局默认时间。它支持 HTTP、HTTPS、FTP 请求。

**Request**

urlopen可以实现最基本请求的发起，但如果要构造一个更完整的请求，可以使用`Request`类来实现。

```python
import urllib.request

request = urllib.request.Request('https://python.org')
response = urllib.request.urlopen(request)
print(response.read().decode('utf-8'))
```

其中，依然使用`urlopen`这个方法来发送请求，但参数不再是URL而是`Request`对象。

```python
%Request对象的构造方式
class urllib.request.Request(url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None)
```

其中，`header`是一个字典，就是请求头，，在构造时，可以通过`header`直接构造，也可以通过调用请求实例的`add_header()`方式添加。

添加请求头最常用的方法就是通过修改`User-Agent`来伪装浏览器。

第四个参数 `origin_req_host` 指的是请求方的 host 名称或者 IP 地址。

第五个参数 `unverifiable` 表示这个请求是否是无法验证的，默认是 `False`，意思就是说用户没有足够权限来选择接收这个请求的结果。

第六个参数 `method` 是一个字符串，用来指示请求使用的方法，比如 GET、POST 和 PUT 等。

#### 高级用法

在上面的过程中，虽然可以构造请求，但对于一些Cookie处理、代理设置之类的更高级的操作，就需要Handler工具。可以把它理解为各种处理器，有专门处理登录验证的，处理Cookie的，处理代理设置的。

首先介绍urllib.request模块里的`BaseHandler`类，这是其他所有Handler的父类，他提供了最基本的方法。例如`default_open`、`protocol_request`等。

- `HTTPDefaultErrorHandler` 用于处理 HTTP 响应错误，错误都会抛出 `HTTPError` 类型的异常。
- `HTTPRedirectHandler` 用于处理重定向。
- `HTTPCookieProcessor` 用于处理 Cookies。
- `ProxyHandler` 用于设置代理，默认代理为空。
- `HTTPPasswordMgr` 用于管理密码，它维护了用户名和密码的表。
- `HTTPBasicAuthHandler` 用于管理认证，如果一个链接打开时需要认证，那么可以用它来解决认证问题。

另一个重要的类就是，OpenerDirector，可以简称为Opener。之前的urlopen方法及时urllib提供的一个Opener。Opener相当于更底层的实现，能够进行更深一步的配置。

以下为Opener应用实例：

**验证**

在访问某些设置了身份认证的网站时，可能会遇见认证窗口。

如果遇到这种启用了基本身份认证的网站。可以使用如下代码：

```	python
from urllib.request import HTTPPasswordMgrWithDefaultRealm, HTTPBasicAuthHandler, build_opener
from urllib.error import URLError

username = 'admin'
password = 'admin'
url = 'https://ssr3.scrape.center/'

p = HTTPPasswordMgrWithDefaultRealm()
p.add_password(None, url, username, password)
auth_handler = HTTPBasicAuthHandler(p)
opener = build_opener(auth_handler)

try:
    result = opener.open(url)
    html = result.read().decode('utf-8')
    print(html)
except URLError as e:
    print(e.reason)
```

这里首先实例化 `HTTPBasicAuthHandler` 对象，其参数是 `HTTPPasswordMgrWithDefaultRealm` 对象，它利用 `add_password` 方法添加进去用户名和密码，这样就建立了一个处理验证的 Handler。

接下来，利用这个 Handler 并使用 `build_opener` 方法构建一个 Opener，这个 Opener 在发送请求时就相当于已经验证成功了。

接下来，利用 Opener 的 `open` 方法打开链接，就可以完成验证了。

**代理**

在做爬虫时，可以使用如下方式添加代理：

```python
from urllib.error import URLError
from urllib.request import ProxyHandler, build_opener

proxy_handler = ProxyHandler({
    'http': 'http://127.0.0.1:8080',
    'https': 'https://127.0.0.1:8080'
})
opener = build_opener(proxy_handler)
try:
    response = opener.open('https://www.baidu.com')
    print(response.read().decode('utf-8'))
except URLError as e:
    print(e.reason)
```

**Cookie**

获取网站的Cookie：

```python
import http.cookiejar, urllib.request

cookie = http.cookiejar.CookieJar()
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('https://www.baidu.com')
for item in cookie:
    print(item.name + "=" + item.value)
    
#BAIDUID=A09E6C4E38753531B9FB4C60CE9FDFCB:FG=1
#BIDUPSID=A09E6C4E387535312F8AA46280C6C502
#H_PS_PSSID=31358_1452_31325_21088_31110_31253_31605_31271_31463_30823
#PSTM=1590854698
```

声明一个Cookiejar对象，接着使用HTTPCookieProcessor生成一个Handler，构建出Opener，对网页实施操作。

当然也可以将Cookie存储到文件中：

```python
import urllib.request, http.cookiejar

filename = 'cookie.txt'
cookie = http.cookiejar.MozillaCookieJar(filename)
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('https://www.baidu.com')
cookie.save(ignore_discard=True, ignore_expires=True)
```

#### 异常处理

urllib 的 error 模块定义了由 request 模块产生的异常。如果出现了问题，request 模块便会抛出 error 模块中定义的异常。

**URLError**

`URLError` 类来自 urllib 库的 error 模块，它继承自 `OSError` 类，是 error 异常模块的基类，由 request 模块产生的异常都可以通过捕获这个类来处理。它具有一个属性reason，返回错误的原因。

```python
from urllib import request, error

try:
    response = request.urlopen('https://cuiqingcai.com/404')
except error.URLError as e:
    print(e.reason)
```

**HTTPError**

它是 `URLError` 的子类，专门用来处理 HTTP 请求错误，比如认证请求失败等。它有如下 3 个属性。

- `code`：返回 HTTP 状态码，比如 404 表示网页不存在，500 表示服务器内部错误等。
- `reason`：同父类一样，用于返回错误的原因。
- `headers`：返回请求头。

当报错的原因是请求超时时，reason返回的是一个对象。

#### 解析链接

urlib提供了parse模块，它定义了处理URL的标准接口，例如URL各部分的抽取，合并及链接转换。它支持如下协议的URL处理：`file`、`ftp`、`gopher`、`hdl`、`http`、`https`、`imap`、`mailto`、`mms`、`news`、`nntp`、`prospero`、`rsync`、`rtsp`、`rtspu`、`sftp`、`sip`、`sips`、`snews`、`svn`、`svn+ssh`、`telnet` 和 `wais`。

**urlparse**

该方法实现了URL的识别与分段。

```python
from urllib.parse import urlparse

result = urlparse('https://www.baidu.com/index.html;user?id=5#comment')
print(type(result))
print(result)

#<class 'urllib.parse.ParseResult'>
#ParseResult(scheme='https', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')
% # 后面是锚点，用于直接定位页面内部的下拉位置。
```

**urlunparse**

该方法可以实现URL的构造，它接收的参数是一个可迭代对象，但是它的长度必须是 6，否则会抛出参数数量不足或者过多的问题。

```python
from urllib.parse import urlunparse

data = ['https', 'www.baidu.com', 'index.html', 'user', 'a=6', 'comment']
print(urlunparse(data))

#https://www.baidu.com/index.html;user?a=6#comment
```

**urlsplit**

与urlparse方法类似，但params会合并到path中。

**urlunsplit**

与 `urlunparse` 方法类似，它也是将链接各个部分组合成完整链接的方法，传入的参数也是一个可迭代对象，例如列表、元组等，唯一的区别是长度必须为 5。

**urljoin**

生成链接还有另一个方法，那就是 `urljoin` 方法。我们可以提供一个 `base_url`（基础链接）作为第一个参数，将新的链接作为第二个参数，该方法会分析 `base_url` 的 `scheme`、`netloc` 和 `path` 这 3 个内容并对新链接缺失的部分进行补充，最后返回结果。

**urlencode**

序列化，用于构造GET请求参数

```python
from urllib.parse import urlencode

params = {
    'name': 'germey',
    'age': 25
}
base_url = 'https://www.baidu.com?'
url = base_url + urlencode(params)
print(url)

#https://www.baidu.com?name=germey&age=25
```

**parse_qs**

反序列化，将GET请求参数转回字典。

**parse_qsl**

反序列化，将GET请求参数转回元组。

**quote**

该方法可以将参数中的中文转为URL格式：

```python
from urllib.parse import quote

keyword = '壁纸'
url = 'https://www.baidu.com/s?wd=' + quote(keyword)
print(url)

#https://www.baidu.com/s?wd=%E5%A3%81%E7%BA%B8
```

**unquote**

对URL解码。

#### 分析Robots协议

利用 urllib 的 robotparser 模块，我们可以实现网站 Robots 协议的分析。

**Robots协议**

它通常是一个叫作 robots.txt 的文本文件，一般放在网站的根目录下。

只允许某一个爬虫访问的代码如下：

```txt
robot.txt:
User-agent: WebCrawler
Disallow:
User-agent: *
Disallow: /
```

常见爬虫名：

| 爬虫名称    | 名称      | 网站              |
| :---------- | :-------- | :---------------- |
| BaiduSpider | 百度      | www.baidu.com     |
| Googlebot   | 谷歌      | www.google.com    |
| 360Spider   | 360 搜索  | www.so.com        |
| YodaoBot    | 有道      | www.youdao.com    |
| ia_archiver | Alexa     | www.alexa.cn      |
| Scooter     | altavista | www.altavista.com |
| Bingbot     | 必应      | www.bing.com      |

**robotparser**

使用 robotparser 模块来解析 robots.txt 了。该模块提供了一个类 `RobotFileParser`，它可以根据某网站的 robots.txt 文件来判断一个爬虫是否有权限来爬取这个网页。

该类的一些方法：

- `set_url`：用来设置 robots.txt 文件的链接。如果在创建 `RobotFileParser` 对象时传入了链接，那么就不需要再使用这个方法设置了。
- `read`：读取 robots.txt 文件并进行分析。注意，这个方法执行一个读取和分析操作，如果不调用这个方法，接下来的判断都会为 `False`，所以一定记得调用这个方法。这个方法不会返回任何内容，但是执行了读取操作。
- `parse`：用来解析 robots.txt 文件，传入的参数是 robots.txt 某些行的内容，它会按照 robots.txt 的语法规则来分析这些内容。
- `can_fetch`：该方法用两个参数，第一个是 `User-Agent`，第二个是要抓取的 URL。返回的内容是该搜索引擎是否可以抓取这个 URL，返回结果是 `True` 或 `False`。
- `mtime`：返回的是上次抓取和分析 robots.txt 的时间，这对于长时间分析和抓取的搜索爬虫是很有必要的，你可能需要定期检查来抓取最新的 robots.txt。
- `modified`：它同样对长时间分析和抓取的搜索爬虫很有帮助，将当前时间设置为上次抓取和分析 robots.txt 的时间。

```python
from urllib.robotparser import RobotFileParser

rp = RobotFileParser()
rp.set_url('https://www.baidu.com/robots.txt')
rp.read()
print(rp.can_fetch('Baiduspider', 'https://www.baidu.com'))
print(rp.can_fetch('Baiduspider', 'https://www.baidu.com/homepage/'))
print(rp.can_fetch('Googlebot', 'https://www.baidu.com/homepage/'))

#True
#True
#False
```

### requests库

#### GET请求

使用requests库构造GET请求：

```python
import requests

data = {
    'name': 'germey',
    'age': 25
}
#params传递参数
r = requests.get('https://httpbin.org/get', params=data)
print(r.text)
#直接返回解析结果可以用json方法
print(r.json())
```

其他类型的请求：

```python
import requests

r = requests.get('https://httpbin.org/get')
r = requests.post('https://httpbin.org/post')
r = requests.put('https://httpbin.org/put')
r = requests.delete('https://httpbin.org/delete')
r = requests.patch('https://httpbin.org/patch')
```

#### 抓取网页

抓取一个普通的网页并提取出特定信息：

```python
import requests
import re

r = requests.get('https://ssr1.scrape.center/')
pattern = re.compile('<h2.*?>(.*?)</h2>', re.S)
titles = re.findall(pattern, r.text)
print(titles)

#['肖申克的救赎 - The Shawshank Redemption', '霸王别姬 - Farewell My Concubine', '泰坦尼克号 - Titanic', '罗马假日 - Roman Holiday', '这个杀手不太冷 - Léon', '魂断蓝桥 - Waterloo Bridge', '唐伯虎点秋香 - Flirting Scholar', '喜剧之王 - The King of Comedy', '楚门的世界 - The Truman Show', '活着 - To Live']
```

**抓取二进制数据**

```python
import requests

r = requests.get('https://scrape.center/favicon.ico')
print(r.text)
print(r.content)

#r.text由于将二进制转化为str会出现乱码，r.context会输出原始二进制数据，可以保存起来，作为图片、音频等。
```

**添加header**

为页面的请求添加header信息：

```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
}
r = requests.get('https://ssr1.scrape.center/', headers=headers)
```

#### POST请求

使用requests实现POST请求：

```python
import requests

data = {'name': 'germey', 'age': '25'}
r = requests.post("https://httpbin.org/post", data=data)
```

#### 响应

在上述过程中，使用了text、content获取响应内容。此外，还有很多属性与方法可以获得其他信息，包括状态码、响应头、Cookie等。

```python
import requests

r = requests.get('https://ssr1.scrape.center/')
print(type(r.status_code), r.status_code)
print(type(r.headers), r.headers)
print(type(r.cookies), r.cookies)
print(type(r.url), r.url)
print(type(r.history), r.history)

#<class 'int'> 200
#<class 'requests.structures.CaseInsensitiveDict'> {'Server': 'nginx/1.17.8', 'Date': 'Sat, 30 May 2020 16:56:40 GMT', 'Content-Type': 'text/html; charset=utf-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'Vary': 'Accept-Encoding', 'X-Frame-Options': 'DENY', 'X-Content-Type-Options': 'nosniff', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains', 'Content-Encoding': 'gzip'}
#<class 'requests.cookies.RequestsCookieJar'> <RequestsCookieJar[]>
#<class 'str'> https://ssr1.scrape.center/
#<class 'list'> []
```

其中，headers 和 cookies 这两个属性得到的结果分别是 CaseInsensitiveDict 和 RequestsCookieJar 类型。

requests还提供了一个内置的状态码查询对象requests.codes，对象的各个属性表示状态码如下：

```python
import requests

r = requests.get('https://ssr1.scrape.center/')
exit() if not r.status_code == requests.codes.ok else print('Request Successfully')

# 信息性状态码
100: ('continue',),
101: ('switching_protocols',),
102: ('processing',),
103: ('checkpoint',),
122: ('uri_too_long', 'request_uri_too_long'),

# 成功状态码
200: ('ok', 'okay', 'all_ok', 'all_okay', 'all_good', '\\o/', '✓'),
201: ('created',),
202: ('accepted',),
203: ('non_authoritative_info', 'non_authoritative_information'),
204: ('no_content',),
205: ('reset_content', 'reset'),
206: ('partial_content', 'partial'),
207: ('multi_status', 'multiple_status', 'multi_stati', 'multiple_stati'),
208: ('already_reported',),
226: ('im_used',),

# 重定向状态码
300: ('multiple_choices',),
301: ('moved_permanently', 'moved', '\\o-'),
302: ('found',),
303: ('see_other', 'other'),
304: ('not_modified',),
305: ('use_proxy',),
306: ('switch_proxy',),
307: ('temporary_redirect', 'temporary_moved', 'temporary'),
308: ('permanent_redirect',
      'resume_incomplete', 'resume',), # These 2 to be removed in 3.0

# 客户端错误状态码
400: ('bad_request', 'bad'),
401: ('unauthorized',),
402: ('payment_required', 'payment'),
403: ('forbidden',),
404: ('not_found', '-o-'),
405: ('method_not_allowed', 'not_allowed'),
406: ('not_acceptable',),
407: ('proxy_authentication_required', 'proxy_auth', 'proxy_authentication'),
408: ('request_timeout', 'timeout'),
409: ('conflict',),
410: ('gone',),
411: ('length_required',),
412: ('precondition_failed', 'precondition'),
413: ('request_entity_too_large',),
414: ('request_uri_too_large',),
415: ('unsupported_media_type', 'unsupported_media', 'media_type'),
416: ('requested_range_not_satisfiable', 'requested_range', 'range_not_satisfiable'),
417: ('expectation_failed',),
418: ('im_a_teapot', 'teapot', 'i_am_a_teapot'),
421: ('misdirected_request',),
422: ('unprocessable_entity', 'unprocessable'),
423: ('locked',),
424: ('failed_dependency', 'dependency'),
425: ('unordered_collection', 'unordered'),
426: ('upgrade_required', 'upgrade'),
428: ('precondition_required', 'precondition'),
429: ('too_many_requests', 'too_many'),
431: ('header_fields_too_large', 'fields_too_large'),
444: ('no_response', 'none'),
449: ('retry_with', 'retry'),
450: ('blocked_by_windows_parental_controls', 'parental_controls'),
451: ('unavailable_for_legal_reasons', 'legal_reasons'),
499: ('client_closed_request',),

# 服务端错误状态码
500: ('internal_server_error', 'server_error', '/o\\', '✗'),
501: ('not_implemented',),
502: ('bad_gateway',),
503: ('service_unavailable', 'unavailable'),
504: ('gateway_timeout',),
505: ('http_version_not_supported', 'http_version'),
506: ('variant_also_negotiates',),
507: ('insufficient_storage',),
509: ('bandwidth_limit_exceeded', 'bandwidth'),
510: ('not_extended',),
511: ('network_authentication_required', 'network_auth', 'network_authentication')
```

#### 高级用法

requests的高级用法，如文件上传、Cookie设置、代理设置等。

**文件上传**

```python
import requests

files = {'file': open('favicon.ico', 'rb')}
r = requests.post('https://httpbin.org/post', files=files)
```

**Cookie设置**

获取Cookie：

```python
import requests

r = requests.get('https://www.baidu.com')
print(r.cookies)
for key, value in r.cookies.items():
    print(key + '=' + value)
    
#<RequestsCookieJar[<Cookie BDORZ=27315 for .baidu.com/>]>
#BDORZ=27315
```

也可以用Cookie模拟登录状态：

```python
import requests

headers = {
    'Cookie': '...',
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36',
}
r = requests.get('https://github.com/', headers=headers)
print(r.text)

#输出的text中包含着登录才有的信息
```

也可以通过为RequestsCookieJar对象传递参数，构造Cookie：

```python
import requests

jar = requests.cookies.RequestsCookieJar()
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36'
}
for cookie in cookies.split(';'):
    key, value=...
    jar.set(key, value)
r = requests.get('https://github.com/', cookies=jar, headers=headers)
print(r.text)
```

**Session维持**

在requests中，如果直接利用get等方法模拟网页请求多次，实际上相当于不同的session，属于不同的会话。可以利用requests的Session类来模拟同一次会话。

```python
import requests

s = requests.Session()
s.get('https://httpbin.org/cookies/set/number/123456789')
r = s.get('https://httpbin.org/cookies')
print(r.text)

#可以获取之前的Cookie
#{
#  "cookies": {"number": "123456789"}
#}
```

**SSL证书验证**

某些网站的CA证书无效，正常访问会包SSL错误。在爬取过程中可以verify参数设置不验证URL的证书。

```python
import requests

response = requests.get('https://ssr2.scrape.center/', verify=False)
print(response.status_code)

#200
```

**超时设置**

```python
import requests

r = requests.get('https://httpbin.org/get', timeout=1)
#r = requests.get('https://httpbin.org/get', timeout=(5, 30))
print(r.status_code)
```

通过以上的方式，将超市时间设置为1秒。实际上，请求分为两个阶段，即连接和读取。timeout设置的时间是二者之和，如果要分别指定可以传入一个元组。

**身份认证**

对于启用了基本身份认证的网站，可以使用get的auth来处理，其中默认auth参数为HTTPBasicAuth类，如果是其他认证方式，也可以指定其他类。

```python
import requests
from requests.auth import HTTPBasicAuth

r = requests.get('https://ssr3.scrape.center/', auth=('admin', 'admin'))
#r = requests.get('https://ssr3.scrape.center/', auth=HTTPBasicAuth('admin', 'admin'))
print(r.status_code)
```

**代理设置**

对于某些网站，在测试的时候请求几次，能正常获取内容。但是一旦开始大规模爬取，对于大规模且频繁的请求，网站可能会弹出验证码，或者跳转到登录认证页面，更甚者可能会直接封禁客户端的 IP，导致一定时间段内无法访问。为了防止这种情况，需要设置代理（使用get的proxies来处理）来解决。

```python
import requests

proxies = {
  'http': 'http://10.10.10.10:1080',
  'https': 'http://10.10.10.10:1080',
}
requests.get('https://httpbin.org/get', proxies=proxies)
```

除了基本的 HTTP 代理外，requests 还支持 SOCKS 协议的代理。

```python
import requests

proxies = {
    'http': 'socks5://user:password@host:port',
    'https': 'socks5://user:password@host:port'
}
requests.get('https://httpbin.org/get', proxies=proxies)
```

### 正则表达式

python的正则表达式使用的是re库。

匹配规则

| 模　　式 | 描　　述                                                     |
| :------- | :----------------------------------------------------------- |
| `\w`     | 匹配字母、数字及下划线                                       |
| `\W`     | 匹配不是字母、数字及下划线的字符                             |
| `\s`     | 匹配任意空白字符，等价于 `[\t\n\r\f]`                        |
| `\S`     | 匹配任意非空字符                                             |
| `\d`     | 匹配任意数字，等价于 `[0-9]`                                 |
| `\D`     | 匹配任意非数字的字符                                         |
| `\A`     | 匹配字符串开头                                               |
| `\Z`     | 匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串     |
| `\z`     | 匹配字符串结尾，如果存在换行，同时还会匹配换行符             |
| `\G`     | 匹配最后匹配完成的位置                                       |
| `\n`     | 匹配一个换行符                                               |
| `\t`     | 匹配一个制表符                                               |
| `^`      | 匹配一行字符串的开头                                         |
| `$`      | 匹配一行字符串的结尾                                         |
| `.`      | 匹配任意字符，除了换行符，当 `re.DOTALL` 标记被指定时，则可以匹配包括换行符的任意字符 |
| `[...]`  | 用来表示一组字符，单独列出，比如 `[amk]` 匹配 `a`、`m` 或 `k` |
| `[^...]` | 不在 `[]` 中的字符，比如 匹配除了 `a`、`b`、`c` 之外的字符   |
| `*`      | 匹配 0 个或多个表达式                                        |
| `+`      | 匹配 1 个或多个表达式                                        |
| `?`      | 匹配 0 个或 1 个前面的正则表达式定义的片段，非贪婪方式       |
| `{n}`    | 精确匹配 n 个前面的表达式                                    |
| `{n, m}` | 匹配 n 到 m 次由前面正则表达式定义的片段，贪婪方式           |
| `a|b`    | 匹配 a 或 b                                                  |
| `()`     | 匹配括号内的表达式，也表示一个组                             |

#### match

使用match方法可以匹配字符串与正则表达式，并且**`match` 方法是从字符串的开头开始匹配的，一旦开头不匹配，那么整个匹配就失败了**。

```python
import re

content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^Hello\s\d\d\d\s\d{4}\s\w{10}', content)
print(result.group())
print(result.span())

#Hello 123 4567 World_This
#(0, 25)
```

**匹配目标**

使用`()`可将正则表达式匹配的子字符串提取出来，`()` 实际上标记了一个子表达式的开始和结束位置，被标记的每个子表达式会依次对应每一个分组，调用 `group` 方法传入分组的索引即可获取提取的结果。

```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^Hello\s(\d+)\sWorld', content)
print(result.group())
print(result.group(1))
print(result.span())

#Hello 1234567 World
#1234567
#(0, 19)
```

`group()`会输出完整的匹配结果，`group(1)`会输出第一个`()`包围的结果。假如正则表达式后面还有 `()` 包括的内容，那么可以依次用 `group(2)`、`group(3)` 等来获取。

**贪婪与非贪婪**

在贪婪匹配下，`.*`会匹配尽可能多的字符。正则表达式中，如果要使用非贪婪匹配，可以使用`.*?`，匹配尽可能少的字符。**需要注意，如果匹配的结果在字符串结尾，`.*?` 就有可能匹配不到任何内容了，因为它会匹配尽可能少的字符。**

**修饰符**

正则表达式可以包含一些可选标志修饰符来控制匹配模式。修饰符被指定为一个可选的标志。

| 修饰符 | 描　　述                                                     |
| :----- | :----------------------------------------------------------- |
| `re.I` | 使匹配对大小写不敏感                                         |
| `re.L` | 做本地化识别（locale-aware）匹配                             |
| `re.M` | 多行匹配，影响 `^` 和 `$`                                    |
| `re.S` | 使.匹配包括换行符在内的所有字符                              |
| `re.U` | 根据 Unicode 字符集解析字符。这个标志影响 `\w`、`\W`、`\b` 和 `\B` |
| `re.X` | 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解 |

**转义匹配**

对于一些特殊字符，可以使用`\`转义字符匹配。

#### search

search在匹配时会扫描整个字符串，然后返回第一个成功匹配的结果。

#### findall

findall用于返回匹配正则表达式的所有内容。

```python
results = re.findall('<li.*?href="(.*?)".*?singer="(.*?)">(.*?)</a>', html, re.S)
print(results)
for result in results:
    print(result)
    print(result[0], result[1], result[2])
#[('/2.mp3', ' 任贤齐 ', ' 沧海一声笑 '), ('/3.mp3', ' 齐秦 ', ' 往事随风 ')]
#('/2.mp3', ' 任贤齐 ', ' 沧海一声笑 ')
#/2.mp3 任贤齐 沧海一声笑
```

可以看到，返回的列表中的每个元素都是元组类型，我们用对应的索引依次取出即可。

#### sub

使用sub可以替换正则表达式匹配的字符串：

```python
import re

content = '54aK54yr5oiR54ix5L2g'
content = re.sub('\d+', '', content)
print(content)

#aKyroiRixLg
```

#### compile

compile方法可以将正则字符串编译成正则表达式对象，以便在后面的匹配中复用。

```python
import re

content1 = '2019-12-15 12:00'
content2 = '2019-12-17 12:55'
content3 = '2019-12-22 13:21'
pattern = re.compile('\d{2}:\d{2}')
result1 = re.sub(pattern, '', content1)
result2 = re.sub(pattern, '', content2)
result3 = re.sub(pattern, '', content3)
print(result1, result2, result3)

#2019-12-15  2019-12-17  2019-12-22
```

此外，compile还可以传入修饰符，相当于对正则表达式做了一层封装。

### XPath网页解析

XPath可以用于HTML文档的搜索，在爬虫中，可用来做相应的信息抽取，使用lxml库。

#### XPath常用规则

| 表　达　式 | 描　　述                 |
| :--------- | :----------------------- |
| `nodename` | 选取此节点的所有子节点   |
| `/`        | 从当前节点选取直接子节点 |
| `//`       | 从当前节点选取子孙节点   |
| `.`        | 选取当前节点             |
| `..`       | 选取当前节点的父节点     |
| `@`        | 选取属性                 |

选择所有名称为 `title`，同时属性 `lang` 的值为 `eng` 的节点。

```xml
//title[@lang='eng']
```

#### 实例

lxml库的etree模块可以自动修正HTML文本：

```python
from lxml import etree
text = '''
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </ul>
 </div>
'''
html = etree.HTML(text)
result = etree.tostring(html)
print(result.decode('utf-8'))

#<html>
#  <body>
#    <div>
#      <ul>
#        <li class="item-0"><a href="link1.html">first item</a></li>
#        <li class="item-1"><a href="link2.html">second item</a></li>
#        <li class="item-inactive"><a href="link3.html">third item</a></li>
#        <li class="item-1"><a href="link4.html">fourth item</a></li>
#        <li class="item-0"><a href="link5.html">fifth item</a></li>
#      </ul>
#    </div>
#  </body>
#</html>
```

也可以直接读取文本进行解析：

```python
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = etree.tostring(html)
```

#### 节点

用 `//` 开头的 XPath 规则来选取所有符合要求的节点：

```python
from lxml import etree
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//*')
print(result)
#[<Element html at 0x10510d9c8>, <Element body at 0x10510da08>
```

返回形式是一个列表，每个元素是 `Element` 类型，其后跟了节点的名称，如 `html`、`body`等。

此处匹配也可以指定节点名称：

```python
#获取li节点
from lxml import etree
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li')
print(result)
#[<Element li at 0x105849208>, <Element li at 0x105849248>]
```

**子结点**

通过`/`与`//`可以查找元素的子结点与子孙节点。

```python
#获取li节点下的子结点a使用/
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li/a')
#获取ul节点下所有子结点a使用//
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//ul//a')
```

此处的 `/` 用于选取直接子节点，如果要获取所有子孙节点，就可以使用 `//`。

**父节点**

使用`..`获取父节点：

```python
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//a[@href="link4.html"]/../@class')
print(result)

#['item-1']
```

同时，也可以通过`parent::`来获取父节点。

```python
from lxml import etree
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//a[@href="link4.html"]/parent::*/@class')
print(result)
#['item-1']
```

#### 属性匹配

选取时可用`@`进行属性过滤：

```python
from lxml import etree
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li[@class="item-0"]')
print(result)
#<Element li at 0x10a399288>, <Element li at 0x10a3992c8>
```

这里通过加入`[@class="item-0"]`，筛选了节点的属性。

**属性多值匹配**

有时一个属性里会含有多个值，此时就无法使用`@`来匹配，应当采用`contains()`方法：

```python
from lxml import etree
text = '''
<li class="li li-first"><a href="link.html">first item</a></li>
'''
html = etree.HTML(text)
result = html.xpath('//li[contains(@class, "li")]/a/text()')
print(result)
#['first item']
```

此种方式在某个节点的某个属性有多个值时经常用到，如某个节点的 `class` 属性通常有多个。

**多属性匹配**

当遇到多个属性确定一个节点，这是需要同时匹配多个属性，此时使用`and`连接：

```python
from lxml import etree
text = '''
<li class="li li-first" name="item"><a href="link.html">first item</a></li>
'''
html = etree.HTML(text)
result = html.xpath('//li[contains(@class, "li") and @name="item"]/a/text()')
print(result)

#['first item']
```

这里的`and`其实是XPath中的运算符，此外还有很多运算符：

|        |                |                     |                                                              |
| :----- | :------------- | :------------------ | :----------------------------------------------------------- |
| 运算符 | 描　　述       | 实　　例            | 返　回　值                                                   |
| `or`   | 或             | `age=19 or age=20`  | 如果 `age` 是 19，则返回 `true`。如果 `age` 是 21，则返回 `false` |
| `and`  | 与             | `age>19 and age<21` | 如果 `age` 是 20，则返回 `true`。如果 `age` 是 18，则返回 `false` |
| `mod`  | 计算除法的余数 | `5 mod 2`           | 1                                                            |
| `|`    | 计算两个节点集 | //book\|//cd        | 返回所有拥有 `book` 和 `cd` 元素的节点集                     |
| `+`    | 加法           | `6 + 4`             | 10                                                           |
| `-`    | 减法           | `6 - 4`             | 2                                                            |
| `*`    | 乘法           | `6 * 4`             | 24                                                           |
| `div`  | 除法           | `8 div 4`           | 2                                                            |
| `=`    | 等于           | `age=19`            | 如果 `age` 是 19，则返回 `true`。如果 `age` 是 20，则返回 `false` |
| `!=`   | 不等于         | `age!=19`           | 如果 `age` 是 18，则返回 `true`。如果 `age` 是 19，则返回 `false` |
| `<`    | 小于           | `age<19`            | 如果 `age` 是 18，则返回 `true`。如果 `age` 是 19，则返回 `false` |
| `<=`   | 小于或等于     | `<=19`              | 如果 `age` 是 19，则返回 `true`。如果 `age` 是 `20`，则返回 `false` |
| `>`    | 大于           | `age>19`            | 如果 `age` 是 20，则返回 `true`。如果 `age` 是 19，则返回 `false` |
| `>=`   | 大于或等于     | `age>=19`           | 如果 `age` 是 19，则返回 `true`。如果 `age` 是 18，则返回 `false` |

#### 文本获取

使用`text()`获取节点中的文本：

```python
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li[@class="item-0"]/a/text()')
print(result)
#['first item', 'fifth item']
```

另一种就是使用 `//`，选取所有子孙节点的文本：

```python
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li[@class="item-0"]//text()')
print(result)
#['first item', 'fifth item']
```

#### 属性获取

通过`@`符号来获取节点的属性值：

```python
from lxml import etree

html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li/a/@href')
print(result)
#['link1.html', 'link2.html']
```

#### 按序选择

在选择某些属性时，可能同时匹配了多个节点，但可以按次序选择其中的一个：

```python
from lxml import etree

text = '''
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </ul>
 </div>
'''
html = etree.HTML(text)
result = html.xpath('//li[1]/a/text()')
print(result)
result = html.xpath('//li[last()]/a/text()')
print(result)
result = html.xpath('//li[position()<3]/a/text()')
print(result)
result = html.xpath('//li[last()-2]/a/text()')
print(result)
#['first item']
#['fifth item']
#['first item', 'second item']
#['third item']
```

这里使用了 `last`、`position` 等方法。在 XPath 中，提供了 100 多个方法，包括存取、数值、字符串、逻辑、节点、序列等处理功能.

#### 节点轴选择

XPath提供了很多节点轴选择方法，包括获取子元素，兄弟元素，父元素等。

### Scrapy

<img src="./images/image-20230905215744325.png" alt="image-20230905215744325" style="zoom:67%;" />

- **Scrapy Engine(引擎)**: 负责Spider、ItemPipeline、Downloader、Scheduler中间的通讯，信号、数据传递等。
- **Scheduler(调度器)**: 它负责接受引擎发送过来的Request请求，并按照一定的方式进行整理排列，入队，当引擎需要时，交还给引擎。
- **Downloader（下载器）**：负责下载Scrapy Engine(引擎)发送的所有Requests请求，并将其获取到的Responses交还给Scrapy Engine(引擎)，由引擎交给Spider来处理，
- **Spider（爬虫）**：它负责处理所有Responses,从中分析提取数据，获取Item字段需要的数据，并将需要跟进的URL提交给引擎，再次进入Scheduler(调度器).
- **Item Pipeline(管道)**：它负责处理Spider中获取到的Item，并进行进行后期处理（详细分析、过滤、存储等）的地方。
- **Downloader Middlewares（下载中间件）**：你可以当作是一个可以自定义扩展下载功能的组件。
- **Spider Middlewares（Spider中间件）**：你可以理解为是一个可以自定扩展和操作引擎和Spider中间通信的功能组件（比如进入Spider的Responses;和从Spider出去的Requests）

#### 制作Scrapy爬虫步骤

1. 新建项目 (scrapy startproject xxx)：新建一个新的爬虫项目
2. 明确目标 （编写items.py）：明确你想要抓取的目标
3. 制作爬虫 （spiders/xxspider.py）：制作爬虫开始爬取网页
4. 存储内容 （pipelines.py）：设计管道存储爬取内容

#### 1.新建项目

```shell
scrapy startproject Spider
```

项目创建后将会生成以下文件：

- scrapy.cfg: 项目的配置文件。
- mySpider/: 项目的Python模块，将会从这里引用代码。
- mySpider/items.py: 项目的目标文件。
- mySpider/pipelines.py: 项目的管道文件。
- mySpider/settings.py: 项目的设置文件。
- mySpider/spiders/: 存储爬虫代码目录。

#### 2.明确目标

创建一个 ItcastItem 类，和构建 item 模型：

```python
import scrapy

class ItcastItem(scrapy.Item):
   name = scrapy.Field()
   title = scrapy.Field()
   info = scrapy.Field()
```

#### 3.制作爬虫

爬虫功能主要分为两步：

**爬数据**

在spider文件夹下创建itcast.py文件，代码框架如下：

```python
import scrapy

class ItcastSpider(scrapy.Spider):
    name = "" #爬虫的唯一识别名称
    allowed_domains = [""]#搜索的域名范围，规定爬虫只爬取该域名下的网页
    start_urls = (
        '',
    )#爬取的URL元组/列表，爬虫从此开始抓取数据，第一次下载的数据将会从这些URLs开始，其他子URL将会从这些URLs中继承。

    def parse(self, response):
        pass
    #解析方法
    #主要作用如下：
    #负责解析返回的网页数据（response.body），提取结构化数据（生成item）。
    #生成需要下一页的URL请求
    #将start_urls修改为需要爬取的第一个url
```

使用如下命令运行：

```shell
scrapy crawl itcast
```

**取数据**

爬取完网页，接下来就是取过程了，使用XPath读取信息。

**保存数据**

scrapy保存信息的最简单的方法主要有四种，-o 输出指定格式的文件：

```
scrapy crawl itcast -o teachers.json
```

json lines格式，默认为Unicode编码

```
scrapy crawl itcast -o teachers.jsonl
```

csv 逗号表达式，可用Excel打开

```
scrapy crawl itcast -o teachers.csv
```

xml格式

```
scrapy crawl itcast -o teachers.xml
```

