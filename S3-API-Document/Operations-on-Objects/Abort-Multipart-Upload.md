# Abort Multipart Upload

## 描述
该操作将终止分片上传。终止分片上传后，Upload ID将不可用，之前存储的分片将被释放。您可以使用List Parts 确保分片已全部释放。

## 请求
### 语法
```
DELETE /ObjectName?uploadId=UploadId HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <Date>
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
### 响应元素
无响应元素
### 特殊错误

Error Code|描述|HTTP Status Code|SOAP Fault Code Prefix
---|---|---|---
NoSuchUpload|指定的分片上传不存在，Upload ID可能是无效的。|404 Not Found|Client

## 示例
### 请求示例
```
DELETE /example-object?uploadId=VXBsb2FkIElEIGZvciBlbHZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZ HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date:  Mon, 1 Nov 2010 20:34:56 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 204 OK
x-amz-request-id: 996c76696e6727732072657175657374
Date:  Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 0
Connection: keep-alive
Server: JDCloudOSS
```
