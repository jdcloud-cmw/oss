# Delete Bucket cors

## 描述
删除指定Bucket中的cors配置信息。仅Bukcet的Owner可操作。

## 请求
### 语法
```
DELETE /?cors HTTP/1.1
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

## 示例
### 请求示例
```
DELETE /?cors HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com
Date: Tue, 13 Dec 2011 19:14:42 GMT
Authorization: <authorization string>
```

### 响应示例
```
HTTP/1.1 204 No Content
x-amz-request-id: 0CF038E9BCF63097
Date: Tue, 13 Dec 2011 19:14:42 GMT
Server: JDCloudOSS
Content-Length: 0
```
