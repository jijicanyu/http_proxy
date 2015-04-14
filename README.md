#httpproxy.py
python socket 实现的http代理   


##TODO
###
+ 以任务队列形式传递分配的socket，只有分配socket的主线程和一两个执行代理的线程（而不是像0.1版本中那样，来一个请求，分配一个socket时，每个socket都启动一个新线程）   
+ 完善对各种status_code的响应报文的判断（已测试通过：200,201,204,206,301,302,303,304,307,404,413,414,500,501,503,505）   
+ 尝试Connection  keep-alive   
+ 完善对http请求的各种method的支持（已测试通过：GET,POST,HEAD)
+ 添加https支持   

##更新日志
###0.4
+ 支持HEAD请求   
+ 确认支持204,206,303,307,413,414,500,501,505响应   
+ 已经能正常浏览http网页（常见的还有CONNECT方法未支持，应该是在https中用到；连着多打开几个可怕的门户网站首页:内存11M，cpu单核100%）

###0.3
+ 修改请求的接收方式，支持post请求
+ 确认支持201,503,404,301,302响应

###0.2 
+ 正确完善http响应报文已接收完的判断方式，已经能正常代理响应为200,304的get和post请求。   
+ 连接接目标主机失败（原因是类似目标网站的服务器压根没开。。之类的），返回自定义的错误信息   
+ 200的通过Content-Length, Transfer-Encoding判断。都没有的默认Transfer-Encoding。   
+ 304的是not modify，直接返回报文头   
+ 404的响应也行，只是在收完报文头就直接返回了，还没有管它收完了没，正常的404报文如果够短的话是没问题的。   

###0.1.1
+ 增加http响应报文已接收完的判断方式，至少校内办公可用了   


###0.1
+ 实现基本功能，能解析get和post请求，并转发请求报文给目标主机，收到响应报文后转回给请求方   
+ 主线程接收请求，把分配的socket传给代理线程；代理线程转发请求报文和响应报文   
+ 性能极烂！！请求一多（只是打开一个大网页）cpu占用率就爆满。原因是，来一个请求，分配一个socket时，每个socket都启动一个新线程。而且判断响应报文是否接收完整的方式不对，会在接收响应报文时无限循环   
+ 不知如何判断http响应报文已接收完   

