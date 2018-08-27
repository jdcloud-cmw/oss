# Upload Part-Copy

## 描述
复制已存在的Object作为源数据上传分片。你可以通过x-amz-copy-source请求头来指定源数据，x-amz-copy-source-range请求头指定数据范围。在上传分片之前必须启动一个分片上传，此时OSS会返回一个Upload ID，你在上传分片请求中必须带上此ID。

## 请求
### 语法
```
PUT /ObjectName?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: <target-bucket>.s3.<region>.jcloudcs.com 
x-amz-copy-source: /source_bucket/sourceObject
x-amz-copy-source-range:bytes=<first-last>
Date: <date>
Authorization: <authorization string>
```

### 请求参数

名称|描述|必须
---|---|---
partNumber|分片编号，分片号是一个分片的唯一标识，同时也是该分片在整个Object中的位置标识。<br>Type: String|是
uploadId|上传ID，初始化分片上传请求中返回的作为唯一标识符的Upload ID。<br>Type: String|是
### 请求Header
除公共请求Header外，还可以使用以下Header

名称|描述|必须
---|---|---
x-amz-copy-source|源Bucket和源Object，使用"/"分隔。<br>Type: String<br>Default: None|是
x-amz-copy-source-range|源Object的数据范围。<br>Type: Integer<br>Default: None|否

### 请求元素
无请求元素

## 响应
### 响应Header
无特殊响应Header

### 响应元素

名称|描述
---|---
CopyPartResult|响应元素集合。<br>Type: Container<br>Ancestor: None
ETag|返回新的分片的ETag。<br>Type: String<br>Ancestor: CopyPartResult
LastModified|返回分片修改的时间。<br>Type: String<br>Ancestor: CopyPartResult

### 特殊错误

Error Code|描述|HTTP Status Code
---|---|---
NoSuchUpload|指定的分片上传ID不存在，可能已失效。|404 Not Found
InvalidRequest|指定的复制源不支持Range复制。|400 Bad Request

## 示例
### 请求示例
```
PUT /newobject?partNumber=2&uploadId=VCVsb2FkIElEIGZvciBlbZZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZR HTTP/1.1
Host: target-bucket.s3.<region>.jcloudcs.com
Date:  Mon, 11 Apr 2011 20:34:56 GMT
x-amz-copy-source: /source-bucket/sourceobject
x-amz-copy-source-range:bytes=500-6291456
Authorization: <authorization string>
```

### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date:  Mon, 11 Apr 2011 20:34:56 GMT
Server: JDCloudOSS 

<CopyPartResult>
   <LastModified>2011-04-11T20:34:56.000Z</LastModified>
   <ETag>"9b2cf535f27731c974343645a3985328"</ETag>
</CopyPartResult>
```
