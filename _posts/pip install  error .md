 系统环境已经安装好了，运行flask项目时，需要安装一些扩展包。出现了一个问题，百度查了一下，终于找到原因了。

1、使用 pip install flask_mail ,出现如下错误：

[root@localhost ~]# pip install flask_mail
Collecting flask_mail
  Retrying (Retry(total=4, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f2ebb138710>: Failed to establish a new connection: [Errno -3] \xe5\x9f\x9f\xe5\x90\x8d\xe8\xa7\xa3\xe6\x9e\x90\xe6\x9a\x82\xe6\x97\xb6\xe5\xa4\xb1\xe8\xb4\xa5',)': /simple/flask-mail/
  Retrying (Retry(total=3, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f2ebb138490>: Failed to establish a new connection: [Errno -3] \xe5\x9f\x9f\xe5\x90\x8d\xe8\xa7\xa3\xe6\x9e\x90\xe6\x9a\x82\xe6\x97\xb6\xe5\xa4\xb1\xe8\xb4\xa5',)': /simple/flask-mail/
  Retrying (Retry(total=2, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f2ebb0f8fd0>: Failed to establish a new connection: [Errno -3] \xe5\x9f\x9f\xe5\x90\x8d\xe8\xa7\xa3\xe6\x9e\x90\xe6\x9a\x82\xe6\x97\xb6\xe5\xa4\xb1\xe8\xb4\xa5',)': /simple/flask-mail/
  Retrying (Retry(total=1, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f2ebb0f8b50>: Failed to establish a new connection: [Errno -3] \xe5\x9f\x9f\xe5\x90\x8d\xe8\xa7\xa3\xe6\x9e\x90\xe6\x9a\x82\xe6\x97\xb6\xe5\xa4\xb1\xe8\xb4\xa5',)': /simple/flask-mail/
  Retrying (Retry(total=0, connect=None, read=None, redirect=None)) after connection broken by 'NewConnectionError('<pip._vendor.requests.packages.urllib3.connection.VerifiedHTTPSConnection object at 0x7f2ebb0f8290>: Failed to establish a new connection: [Errno -3] \xe5\x9f\x9f\xe5\x90\x8d\xe8\xa7\xa3\xe6\x9e\x90\xe6\x9a\x82\xe6\x97\xb6\xe5\xa4\xb1\xe8\xb4\xa5',)': /simple/flask-mail/
  Could not find a version that satisfies the requirement flask_mail (from versions: )
No matching distribution found for flask_mail


2、使用正确的安装源就可以了 

# pip install flask_mail -i https://pypi.tuna.tsinghua.edu.cn/simple/


目前常用源有：

清华大学　http://pypi.tuna.tsinghua.edu...

中国科学技术大学　https://pypi.mirrors.ustc.edu...
--------------------- 
作者：凡夫俗子66 
来源：CSDN 
原文：https://blog.csdn.net/m0_38061194/article/details/79466585 
版权声明：本文为博主原创文章，转载请附上博文链接！