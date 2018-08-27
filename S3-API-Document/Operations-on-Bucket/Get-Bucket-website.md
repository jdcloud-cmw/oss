# Get Bucket website

## 描述
该操作返回指定Bucket的静态网站托管配置，仅Bukcet的Owner可操作。

## 请求
### 语法
```
GET /?website HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```
### 请求参数
无请求参数
### 请求Header
无特殊Header
### 请求元素
无请求元素

## 响应
### 响应Header
无特殊响应Header
### 响应元素
响应XML与Put Bucket website的请求元素相同。

## 示例
### 请求示例
```
GET ?website HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Thu, 27 Jan 2011 00:49:20 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 3848CD259D811111
Date: Thu, 27 Jan 2011 00:49:26 GMT
Content-Length: 240
Content-Type: application/xml
Transfer-Encoding: chunked
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <IndexDocument>
    <Suffix>index.html</Suffix>
  </IndexDocument>
  <ErrorDocument>
    <Key>404.html</Key>
  </ErrorDocument>
</WebsiteConfiguration>
```

