# Complete Multipart Upload

## 描述
该操作通过组合之前上传的分片来完成分片上传。

首先初始化分片上传，然后使用Upload Parts上传分片。成功上传所有分片后，可调用该API完成分片上传。OSS收到该请求后，会按照分片编号进行升序连接以创建Object。在该请求中，需要提供完整的分片列表，以及每个分片的编号及Etag。

合并所以分片需要几分钟时间，所以OSS在开始处理该请求后会回复200 OK。在处理过程中，OSS会定期发送空白字符以防止连接超时。该请求在200 OK响应发送后可能会失败，您需要检查响应主体来确定请求是否成功。

## 请求
### 语法
```
POST /ObjectName?uploadId=UploadId HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <Date>
Content-Length: <Size>
Authorization: <authorization string>

<CompleteMultipartUpload>
  <Part>
    <PartNumber>PartNumber</PartNumber>
    <ETag>ETag</ETag>
  </Part>
  ...
</CompleteMultipartUpload>
```
### 请求参数
无请求参数
### 请求Header
无特殊Header
### 请求元素

名称|描述|必须
---|---|---
CompleteMultipartUpload|请求集合。<br>Ancestor: None<br>Type: Container<br>Children: One or more Part elements|是
Part|之前上传的分片的PartNumber及ETag信息的集合。<br>Ancestor: CompleteMultipartUpload<br>Type: Container<br>Children: PartNumber, ETag|是
PartNumber|分片的唯一标识符。<br>Ancestor: Part<br>Type: Integer|是
ETag|分片上传后返回的实体标签。<br>Ancestor: Part<br>Type: String|是

## 响应
### 响应Header
除公共响应Header外，还有以下Header:

Header|描述
---|---
x-amz-expiration|如果该Object设置了过期时间(Put Bucket lifecycle上线后支持)，响应中将包含包含该Header。<br>Type: String

### 响应元素

名称|描述
---|---
CompleteMultipartUploadResult|响应集合。<br>Type: Container<br>Children: Bucket, Key, UploadId<br>Ancestors: None
Location|新创建的Bucket的URL。<br>Type: URI<br>Ancestors: CompleteMultipartUploadResult
Bucket|所在的Bucket名称<br>Type: String<br>Ancestors: CompleteMultipartUploadResult
Key|新创建的Object名称。<br>Type: String<br>Ancestors: CompleteMultipartUploadResult
Etag|Etag可以标识新创建的Object数据。不同Object有不同的Etag。<br>Type: String<br>Ancestors: CompleteMultipartUploadResult

### 特殊错误

Error Code|描述|HTTP Status Code
---|---|---
EntityTooSmall|每个分片至少5MB，最后一个分片没有大小限制。|400 Bad Request
InvalidPart|一个或多个指定的分片不存在。分片可能没有上传或者指定的Etag与分片的Etag不匹配。|400 Bad Request
InvalidPartOrder|分片列表没有按照升序排列。|400 Bad Request
NoSuchUpload|指定的分片上传不存在，Upload ID是无效的。|404 Not Found

## 示例
### 请求示例
```
POST /example-object?uploadId=AAAsb2FkIElEIGZvciBlbHZpbmcncyWeeS1tb3ZpZS5tMnRzIRRwbG9hZA HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date:  Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 391
Authorization: <authorization string>

<CompleteMultipartUpload>
  <Part>
    <PartNumber>1</PartNumber>
    <ETag>"a54357aff0632cce46d942af68356b38"</ETag>
  </Part>
  <Part>
    <PartNumber>2</PartNumber>
    <ETag>"0c78aef83f66abc1fa1e8477f296d394"</ETag>
  </Part>
  <Part>
    <PartNumber>3</PartNumber>
    <ETag>"acbd18db4cc2f85cedef654fccc4a4d8"</ETag>
  </Part>
</CompleteMultipartUpload>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
Connection: close
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Location>http://Example-Bucket.s3.amazonaws.com/Example-Object</Location>
  <Bucket>Example-Bucket</Bucket>
  <Key>Example-Object</Key>
  <ETag>"3858f62230ac3c915f300c664312c11f-9"</ETag>
</CompleteMultipartUploadResult>
```


