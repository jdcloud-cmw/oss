# DELETE Bucket policy

## 描述
该操作删除指定Bukcet的policy，仅Bukcet的Owner可操作。

## 请求
### 语法
```
DELETE /?policy HTTP/1.1
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
包含DELETE操作的状态，包括请求失败的错误代码。

## 示例
### 请求示例
```
DELETE /?policy HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Tue, 04 Apr 2010 20:34:56 GMT  
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 204 No Content 
x-amz-request-id: 656c76696e672SAMPLE5657374  
Date: Tue, 04 Apr 2010 20:34:56 GMT  
Connection: keep-alive  
Server: JDCloudOSS
```
