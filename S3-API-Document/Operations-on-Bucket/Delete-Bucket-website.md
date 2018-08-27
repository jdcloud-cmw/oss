# Delete Bukcet website

## 描述
该操作删除指定Bucket中的静态网站托管配置，仅Bukcet的Owner可操作。

## 请求
### 语法
```
DELETE /?website HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```

### 请求参数
无请求参数
### 请求Header
无特殊请求Header
### 请求元素
无请求元素

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例
### 请求示例
```
DELETE ?website HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Thu, 27 Jan 2011 12:00:00 GMT
Authorization: <authorization string>
```

### 响应示例
```
HTTP/1.1 204 No Content
x-amz-request-id: AF1DD829D3B49707
Date: Thu, 03 Feb 2011 22:10:26 GMT
Server: JDCloudOSS
```
