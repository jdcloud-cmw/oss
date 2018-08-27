# Delete Object

## 描述
该操作可以删除Object，需有该Object的DELETE权限。

## 请求
### 语法
```
DELETE /ObjectName HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Content-Length: <length>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```
### 请求参数
无请求参数
### 请求Header
无特殊请求Header
### 请求元素
无特殊元素

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例
### 请求示例
```
DELETE /my-second-image.jpg HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Wed, 12 Oct 2009 17:50:00 GMT
Authorization: <authorization string>
Content-Type: text/plain
```
### 响应示例
```
HTTP/1.1 204 NoContent
x-amz-request-id: 0A49CE4060975EAC
Date: Wed, 12 Oct 2009 17:50:00 GMT
Content-Length: 0
Connection: close
Server: JDCloudOSS
```



