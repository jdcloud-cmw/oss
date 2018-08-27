# Upload Part

## 描述
分片上传中上传单个分片。您必须初始化一个新的分片上传事件。在初始化请求中，OSS将会返回作为唯一标识符的Upload ID，您的上传分片请求中需包含该ID。

分片编号是1-10000的任何数字，分片号是一个分片的唯一标识，同时也是该分片在整个Object中的位置标识。

为了确保数据在传输过程中未损坏，可以在上传分片请求中指定Content-MD5，OSS将根据MD5值检查分片数据，如果不匹配，将会返回错误。

注：初始化分片上传后并上传一个或多个分片后，OSS会对分片所占的存储空间按量收费，您必须完成或者终止分片上传后才可停止OSS对分片的存储费用。

## 请求
### 语法
```
PUT /ObjectName?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: <Bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Content-Length: <Size>
Authorization: <authorization string>
```
### 请求参数

名称|描述|必须
---|---|---
partNumber|分片编号，分片号是一个分片的唯一标识，同时也是该分片在整个Object中的位置标识。<br>Type: String|是
uploadId|上传ID，初始化分片上传请求中返回的作为唯一标识符的Upload ID。<br>Type: String|是

### 请求Header
除公用请求Header外，还可使用以下Header。

名称|描述|必须
---|---|---
Content-Length|Object的大小，单位为byte；更详细描述请参照RFC2616。<br>Type: String<br>Default: None<br>Constraints: None|是
Content-MD5|对报文主体进行MD5算法获得128位二进制数，在通过Base64编码写入Content-MD5。可用于数据完整性检查。<br>Type: String<br>Default: None<br>Constraints: None|否
Expect|客户端使用Expect告知OSS，期望出现某种特定的行为。若OSS无法做出回应而发生错误时，请求报文主体将不会发送。<br>Type: String<br>Default: None<br>Valid Values: 100-continue<br>Constraints: None|否

### 请求元素
无请求元素

## 响应
### 响应Header
除了通用响应Header外，还包括以下响应Header。

名称|描述
---|---
x-amz-storage-class|Object的存储类型。OSS为非标准存储的Object返回该Header。<br>Type: String<br>Default: None

### 响应元素
无响应元素

### 特殊错误

Error Code|描述|HTTP Status Code|SOAP Fault Code Prefix
---|---|---|---
NoSuchUpload|指定的分片上传不存在。Upload ID可能是无效的。|404 Not Found|Client

## 示例
### 请求示例
```
PUT /my-movie.m2ts?partNumber=1&uploadId=VCVsb2FkIElEIGZvciBlbZZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZR HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date:  Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 10485760
Content-MD5: pUNXr/BjKK5G2UKvaRRrOA==
Authorization: <authorization string>

***part data omitted***
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date:  Mon, 1 Nov 2010 20:34:56 GMT
ETag: "b54357faf0632cce46e942fa68356b38"
Content-Length: 0
Connection: keep-alive
Server: JDCloudOSS
```

