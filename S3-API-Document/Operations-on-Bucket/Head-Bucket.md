# Head-Bucket

## 描述
此操作用于确定指定Bucket是否存在。

## 请求

## 语法
```
HEAD / HTTP/1.1     
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))    
```
### 请求参数
无请求参数

### 请求元素
无请求元素

### 请求Header
该操作仅使用通用的请求Header，请参阅常见请求Header。

## 响应
### 响应Header
该操作仅使用通用的响应Header，请参阅常见响应Header。
### 响应元素
无响应元素

## 示例
### 请求示例
```
HEAD / HTTP/1.1
Date: Fri, 10 Feb 2012 21:34:55 GMT
Authorization: <authorization string>
Host: oss-example.s3.<region>.jcloudcs.com 
Connection: Keep-Alive
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 32FE2CEB32F5EE25
Date: Fri, 10 2012 21:34:56 GMT
Server: JDCloudOSS
```
