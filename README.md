# DouBanSpider
豆瓣小组数据爬取流程记录

1. 获取豆瓣登录页面地址
2. 分析在页面中已登录过的请求中的差异，发现重复登录后浏览器cookie会保存图形验证码的信息，再次登录后将不会再验证图形验证码，以此将cookie copy到api工具中绕过图形验证码并获取账号信息
3. 在页面中输入要搜索的小组名称，如需通过爬虫的方式搜索小组的名称请自行排查搜索使用的请求。获取已显示的小组URL跳转到小组首页
4. 注意在跳转到目标小组首页的html response中是已页面的形式返回的数据，注意过滤项。其中页面中初始化的window._GLOBAL_NAV中包含获取事实推送的参数，如需通过长连接获取较为事实的数据需要注意window._GLOBAL_NAV json数据中的SSE_TOKEN和SSE_TIMESTAMP
5. 获取长连接SSE中较为实时的数据，参照API请求结构
   ```json
   login
   https://accounts.douban.com/j/mobile/login/basic
   method: post
   headers: cookie:***
   content-type: application/x-www-form-urlencoded

   sse
   https://push.douban.com:4397/sse?channel=notification:user:$userId&auth=$userId_$SSE_TIMESTAMP:$SSE_TOKEN
   userId: 在成功登录后 response中获取
   ```
