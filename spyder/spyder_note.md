# 爬虫学习笔记

```python
# 查看模块各内容的类型
for k,v in request.__dict__.items():
	print(k,'----',v)
```

### 1.常用请求方法
- get:一般情况下，只从服务器获取资源，不会对服务器资源产生任何影响
- post:会对服务器资源产生一定的影响，如登录，上传文件等

### 2.请求头常见参数

- User-agent
    - 浏览器名称，一般是需要的。没有此设置，将会是默认`User-agent`即`python`
- Referer
    - 表明当前这个请求是来自哪一个url,就是说明自己是从哪里过来的......因为有些站点只能通过某些特定url访问
- Cookie
    - 简单粗暴型 见案例 [s03.py](oneDay/s03.py)
    - 
    - cookie的格式
    `Set-Cookie:NAME=VALUE; Expres/Max-age=DATE; Path=PATH;Domain=DOMAIN_NAME; SECURE`
    - ##### 参数意义：
    - **NAME=VALUE**：这是每一个Cookie均必须有的部分。NAME是该Cookie的名称，VALUE是该  Cookie的值。在字符串“NAME=VALUE”中，不含分号、逗和空格等字符。
    - **Expires=DATE**：Expires变量是一个只写变量，它确定了Cookie有效终止日期。该属性值DATE必须以特定的格式来书写：星期 几DD－MM－YY HH:MM:SS GMT，GMT表示这是格林尼治时间。反之，不以这样的格式来书写，系统将无法识别。该变量可省，如果缺省时，则Cookie的属性值不保存在用户的硬 盘中，而仅仅保存在内存当中，Cookie文件将随着浏览器的关闭而自动消失。
    - **Domain=DOMAIN－NAME:Domain** 该变量是 一个只写变量，它确定了哪些Internet域中的Web服务器可读取浏览器所存取的Cookie，即只有来自这个的页面才可以使用Cookie中的信 息。这项设置是可选的，如果缺省时，设置Cookie的属性值为该Web服务器的域名。
    - **Path=PATH**：Path属性定义了Web服务器 上哪些路径下的页面可获取服务器设置的Cookie。一般如果用户输入的URL中的路径部分从第一个字符开始包含Path属性所定义的字符串，浏览器就认 为通过检查。如果Path属性的值为“/”，则We服务器上所有的WWW资源均可读取该Cookie。同样该项设置是可选的，如果缺省时，则Path的 属性值为Web服务器传给浏览器的资源的路径名。　以看出我们借助对Domain和Path两个变量的设置，即可有效地控制Cookie文件被访问的范围。
    - **Secure**：在Cookie中标记该变量，表明只有当浏览器WebServer之间的通信协议为加密认证协议时，浏览器才向服务器提交相应的Cookie。当前这种协议只有一种，即为HTTPS。 


### 3.urllib库
- #### request模块 网络请求相关的
    - urlopen函数 (常规用法)
        - urlopen(url,data=None),不设置data或data为None时使用get方法，设置了data则使用post请求
        - 返回值是一个类文件句柄对象，有`read(size), readline, readlines以及getcode`等方法
    - urlretrieve函数
        - 方便的将网页上的文件保存到本地
        - ```python
            urllib.request.urlretrieve('https://tse2-mm.cn.bing.net/th/id/OIP.xr3SILJaiLRbyr2B-Dp0wgHaKl?w=203&h=289&c=7&o=5&dpr=1.25&pid=1.7','yifei.jpg')
          ```

    - Request类
        - 当需要增加`headers`等信息时使用，根据传进去的信息返回一个请求头用来下一步的请求
        - 或者想更加正式一点，都可以用Request来创建请求的对象
- #### parse模块
    - urlencode函数
        - 请求时，如果url中包含了其他特殊字符(中文等), 将进行编码
    - parse_qs函数
        - 将经编码过的url解码
    - ```python
        url = 'https://www.baidu.com/s?'
        params = {"wd":"刘亦菲"}
        qs = urllib.parse.urlencode(params)
        url = url + qs
        print(url)
        url = urllib.parse.parse_qs(url)
        print(url)
      ```
    - urlparse函数
        - 将一个完整的url拆分成由`scheme、netloc、path`等
    - urlsplit函数
        - 同上，不过少一个`params`


### 4、requests库
- 安装和 [中文文档](https://requests.readthedocs.io/zh_CN/latest/)
- 





### 4、cookielib库和HTTPCookieProcessor模拟登录
- cookielib主要作用是用来提供存储cookie的对象
- 主要作用是处理这些用来保存cookie(上面)的对象，并构建handler对象
- [CookieJar案例 s04.py](oneDay/s04.py)
- 将cookie保存到本地  [案例s05.py](oneDay/s05.py)





- ## 反反爬虫

- #### ProxyHander处理器 (代理设置)
    - request的一个类



### 拓展补充
- 编码与解码 
    - `encode`和`decode`是在str和byte类型之间转换。str是我们人能看到的，byte是计算机能够理解的，所以encode应为str到byte，即将内容编写出计算机能够存储的码，decode应为byte到str，即从计算机码中解析出内容
    - 字符串通过编码转换为字节码，字节码通过解码转换为字符串
        > str--->(encode)--->bytes，bytes--->(decode)--->str

    - 所以`encode`意思就是 以某种编码方式对`str`进行编码，使其变成`byte`

- Handler处理器和自定义Opener
    - urlopen方法不支持代理、cookie等其他的HTTP/HTTPS高级功能, 所以为了支持这些功能一般都会使用下面的方法
        1. 使用相关的Handler处理器来创建特定功能的处理器对象；就是需要哪个功能就创建哪个功能。

        2. 然后通过urllib.build_opener()方法将这些处理器对象(支持的功能)传参进去来创建自定义opener对象；

        3. 使用自定义的opener对象，调用open()方法发送请求。