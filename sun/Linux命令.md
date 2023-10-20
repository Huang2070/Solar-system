## less
* /string 向前搜索指定字符串
* ?string 向后搜索指定字符串
* n 搜索指定字符串的下一个出现位置
* N 搜索指定字符串的上一个出现位置


## curl

### 模拟post请求 
body体方式传参数, json格式

curl -X POST -H "Content-Type:application/json" -H "Data_Type:msg" --data '{"positionCodes":"ALIPAY_MINI_APP_HOME_BANNER_V2"}' http://127.0.0.1:7001/distribute/getResourceListWithToken?

### get请求
get请求要把加引号, 不然传参只能传第一个参数.


## 查看出口IP
* curl ipinfo.io
* curl cip.cc


## 查看目录或文件大小
* 找出大文件: du -sh /home/admin/* | grep G
* df -HTP 查看磁盘使用情况
* ll -H 人性化友好的方式展示目录下文件大小
* du -H 展示当前目录文件之和


## grep
* -v, 排除某个字符串. 示例:cat access.log | grep -v nginx_status  排除文件中包含nginx_status的行.
* 
