# Delete Bucket

## 描述
该操作可删除指定Bucket。删除Bucket前需将其中的所有Object删除。

## 请求
### 语法
```
DELETE / HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```

### 请求参数
无请求参数

### 请求Header
该操作仅使用通用的请求Header，请参阅常见请求Header。

### 请求元素
无请求元素

## 响应
### 响应Header
该操作仅使用通用的响应Header，请参阅常见响应Header。

## 示例
### 请求示例
该请求删除名为"oss-example"的bucket。
```
DELETE / HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Wed, 01 Mar  2006 12:00:00 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 204 No Content
x-amz-request-id: 32FE2CEB32F5EE25
Date: Wed, 01 Mar  2006 12:00:00 GMT
Connection: close
Server: JDCloudOSS
```
