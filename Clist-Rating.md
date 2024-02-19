获取 clist.by 网站的 Rating 分数据，并展示。

## 凭证

访问 Clist 数据需要有效的登录凭证，你有两种选择：

- 填写 [clist.by](https://clist.by/) 官方的 [API key](https://clist.by/api/v4/doc/)

- 使浏览器中的 clist.by 网站处于登录状态（即 Cookie 有效）。

    > [!WARNING]
    >
    > **隐私安全**
    >
    > 脚本并不会去获取 clist.by 的具体 cookie ，而是由浏览器在发送请求时自动携带，cookie 有效性通过尝试请求所返回的状态码判断

### 说明

由于 clist 官方的API请求存在每分钟10次的限制，而题目集页面需要短时间请求大量数据，

因此对于题目集（problemset）页面，是通过模拟页面访问来实现的。

而对于题目（problem）页和问题集（contest）页，则使用官方的 API