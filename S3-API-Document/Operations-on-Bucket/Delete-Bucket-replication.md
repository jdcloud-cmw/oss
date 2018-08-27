# Delete Bucket replication

## 描述
删除指定Bucket的复制配置。

## 请求
### 语法
```
DELETE /?replication HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> 
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

## 示例
### 请求示例
```
DELETE /?replication HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Wed, 11 Feb 2015 05:37:16 GMT
20150211T171320Z

Authorization: <authorization string> 
```
### 响应示例
```
HTTP/1.1 204 No Content  
x-amz-request-id: 656c76696e672example  
Date: Wed, 11 Feb 2015 05:37:16 GMT
Connection: keep-alive  
Server: JDCloudOSS    
```
