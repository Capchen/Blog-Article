# Cross-site scripting(跨站脚本攻击)

### 背景和现状
当网景（Netscape）最初推出JavaScript语言时，他们也察觉到准许网页服务器传送可执行的程式码给一个浏览器的安全风险（即使仅是在一个浏览器的沙盒里）。它所造成的一个关键的问题在于使用者同时开启多个浏览器视窗时，在某些例子里，网页里的片断程式码被允许从另一个网页或物件取出资料，而因为恶意的网站可以用这个方法来尝试窃取机密资讯，所以在某些情形，这应是完全被禁止的。为了解决这个问题，浏览器采用了同源决策——仅允许来自相同网域名称系统和使用相同协定的物件与网页之间的任何互动。

XSS漏洞可以追溯到1990年代。大量的网站曾遭受XSS漏洞攻击或被发现此类漏洞，如Twitter，Facebook，MySpace，Orkut ,新浪微博和百度贴吧 。研究表明，最近几年XSS已经超过缓冲区溢出成为最流行的攻击方式，有68%的网站可能遭受此类攻击。根据开放网页应用安全计划（Open Web Application Security Project）公布的2010年统计数据，在Web安全威胁前10位中，XSS排名第2，仅次于代码注入（Injection）。

### 什么是XSS
Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种代码注入攻击。它允许攻击者规避同源策略,利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是JavaScript，但实际上也可以包括Java，VBScript，ActiveX，Flash或者甚至是普通的HTML。攻击成功后，攻击者可能得到更高的权限（如执行一些操作）、私密网页内容、会话和cookie等各种内容。

### 工作原理

![work](https://gyenno-test.oss-cn-beijing.aliyuncs.com/pub/miniprogram/demo-test/cross-site-scripting.svg)

不仅仅是业务上的“用户的 UGC 内容”可以进行注入，包括 URL 上的参数等都可以是攻击的来源。在处理输入时，以下内容都不可信：

- 来自用户的 UGC 信息
- 来自第三方的链接
- URL 参数
- POST 参数
- Referer （可能来自不可信的来源）
- Cookie （可能来自其他子域注入）

### 演示环节

举一个🌰

vue中的v-html

`<a href="javascript:alert('XSS')">Money</a>`

### 注入方法

- 在 HTML 中内嵌的文本中，恶意内容以 script 标签形成注入。
- 在内联的 JavaScript 中，拼接的数据突破了原本的限制（字符串，变量，方法名等）。
- 在标签属性中，恶意内容包含引号，从而突破属性值的限制，注入其他属性或者标签。
- 在标签的 href、src 等属性中，包含 javascript: 等可执行代码。
- 在 onload、onerror、onclick 等事件中，注入不受控制代码。
- 在 style 属性和标签中，包含类似 background-image:url("javascript:..."); 的代码（新版本浏览器已经可以防范）。
- 在 style 属性和标签中，包含类似 expression(...) 的 CSS 表达式代码（新版本浏览器已经可以防范）。

### 分类

#### 存储型XSS(Stored XSS)

恶意脚本来自数据库

[演示地址](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

存储型 XSS 的攻击步骤：

1. 攻击者将恶意代码提交到目标网站的数据库中。
2. 用户打开目标网站时，网站服务端将恶意代码从数据库取出，拼接在 HTML 中返回给浏览器。
3. 用户浏览器接收到响应后解析执行，混在其中的恶意代码也被执行。
4. 恶意代码窃取用户数据并发送到攻击者的网站，或者冒充用户的行为，调用目标网站接口执行攻击者指定的操作。

这种攻击常见于带有用户保存数据的网站功能，如论坛发帖、商品评论、用户私信等。

#### 反射行XSS(Reflected XSS)

恶意脚本来自当前的http请求

假设一个网站有一个搜索功能，它接收用户在 URL 参数中提供的搜索词：
```
https://www.website.com/search?value=gift
```
应用程序会在对此 URL 的响应中回显所提供的搜索词：

```
<p>搜索关键词: gift</p>
```
如果将搜索词变成一段脚本

```
https://www.website.com/search?value=<script>do something</script>
```

[演示地址](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

反射型 XSS 的攻击步骤：

1. 攻击者构造出特殊的 URL，其中包含恶意代码。
2. 用户打开带有恶意代码的 URL 时，网站服务端将恶意代码从 URL 中取出，拼接在 HTML 中返回给浏览器。
3. 用户浏览器接收到响应后解析执行，混在其中的恶意代码也被执行。
4. 恶意代码窃取用户数据并发送到攻击者的网站，或者冒充用户的行为，调用目标网站接口执行攻击者指定的操作。

> 反射型 XSS 跟存储型 XSS 的区别是：存储型 XSS 的恶意代码存在数据库里，反射型 XSS 的恶意代码存在 URL 里。

> 反射型 XSS 漏洞常见于通过 URL 传递参数的功能，如网站搜索、跳转等。

> POST 的内容也可以触发反射型 XSS，只不过其触发条件比较苛刻（需要构造表单提交页面，并引导用户点击），所以非常少见。

#### DOM型XSS(DOM-based XSS)

漏洞存在于客户端代码而不是服务器端代码中。

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="safe">我的银行卡密码是123456</div>
  <input id="demo"></input>
  <button id="btn">搜索</button>
  <div id="content" style="min-height: 20px;"></div>
  <script>
    // 获取url参数
    function getUrlParam(name) {
      const reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)')
      const r = window.location.search.substr(1).match(reg)
      if (r != null) return unescape(r[2])
      return null
    }
    const data = getUrlParam('value')
    if (data) {
      const div = document.querySelector('#content')
      div.innerHTML = data
    }
    const btn = document.querySelector('#btn')
    const demo = document.querySelector('#demo')
    btn.addEventListener('click', () => {
      const value = demo.value
      window.location.href = `demo.html?value=${value}`
    })
  </script>
</body>
</html>
```

DOM 型 XSS 的攻击步骤：

1. 攻击者构造出特殊的 URL，其中包含恶意代码。
2. 用户打开带有恶意代码的 URL。
3. 用户浏览器接收到响应后解析执行，前端 JavaScript 取出 URL 中的恶意代码并执行。
4. 恶意代码窃取用户数据并发送到攻击者的网站，或者冒充用户的行为，调用目标网站接口执行攻击者指定的操作。

DOM 型 XSS 跟前两种 XSS 的区别：DOM 型 XSS 攻击中，取出和执行恶意代码由浏览器端完成，属于前端 JavaScript 自身的安全漏洞，而其他两种 XSS 都属于服务端的安全漏洞。

## 检测方法

### 工具扫描

#### w3af

#### nikto

### 手动检测

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

## 预防措施

从上面三种类型的xss攻击方式中，我们可以看出：

- 恶意代码被提交到后端
- 浏览器执行了恶意代码

### （阻止恶意代码被提交到后端）--> 输入过滤转义

#### 前端进行过滤

可行，但是如果绕过前端直接请求接口，还是可以提交恶意代码。

#### 后端进行过滤

可行，但是要确保，消费数据的端能够正常解析转义后的内容。

例如，浏览器可以直接渲染转义后的`&lt; `,
但是在vue模版中，`&lt; `被认为是个字符串。

所以数据消费，有一定的心智负担。

### （阻止浏览器执行恶意代码）--> 防止HTML中出现注入

#### Reflected XSS/ Stored XSS

- 纯前端渲染

  浏览器直接明确渲染方式，例如innerText，{{ res.data }},避免使用innerHtml，v-html

- 转义HTML

  有时候确实要拼接HTML，使用转义库。

#### Dom XSS

- 避免使用v-html，innerHTML，outerHTML，

- 避免将不可信数据直接应用于内联事件监听器，如href，onload，onclick等

### 内容安全策略（CSP）

