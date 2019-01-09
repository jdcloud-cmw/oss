# List Multipart Uploads

## 描述
该操作列出正在进行的分片上传。最多返回1000个分段上传。

## 请求
### 语法
```
GET /?uploads HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <Date>
Authorization: <authorization string>			
```
### 请求参数

参数|描述|必须
---|---|---
delimiter|用于分组的字符。使用perfix和第一次出现delimiter的位置之间的字符对所有object进行分组，并作为CommonPrefixes。如果你不指定prefix，则从首字符开始。被分组于CommonPrefixes下的object不会再响应的其他地方返回。<br>Type: String|否
encoding-type|请求OSS编码响应并指定指定编码方法。<br>Type: String<br>Default: None<br>Valid value: url|否
prefix|列出与指定前缀匹配的正在分片上传的object。<br>Type: String|否

### 请求Header
无特殊请求Header
### 请求元素
无请求元素

## 响应
### 响应Header
无特殊响应Header
### 响应元素

名称|描述
---|---
ListMultipartUploadsResult|响应的集合。<br>Children: Bucket, KeyMarker, UploadIdMarker, NextKeyMarker, NextUploadIdMarker, MaxUploads, Delimiter, Prefix, CommonPrefixes, IsTruncated<br>Type: Container<br>Ancestor: None
Bucket|分片上传所在的Buckt。<br>Type: String<br>Ancestor: ListMultipartUploadsResult
Encoding-Type|OSS编码Object的编码类型。<br>如果指定encoding-type请求参数，则OSS会在响应中包含此元素，并在以下响应元素中返回已编码的值：Delimiter，Prefix，Key。<br>Type: String<br>Ancestor: ListBucketResult
Upload|特定的分片上传的信息集合，一个响应中包含零个或多个Upload元素。<br>Type: ContainerChildren: Key, UploadId, InitiatorOwner, StorageClass, Initiated<br>Ancestor: ListMultipartUploadsResult
Key|执行分片上传的Object。<br>Type: Integer<br>Ancestor: Upload
UploadID|标识分片上传的ID。<br>Type: Integer<br>Ancestor: Upload
Initiator|标识启动的分片上传的用户信息的集合。<br>Children: ID, DisplayName<br>Type: Container<br>Ancestor: Upload
ID|用户ID<br>Type: String<br>Ancestor: Initiator,Owner
DisplayName|用户名<br>Type: String<br>Ancestor: Initiator,Owner
Owner|Object拥有者的信息集合。Type: Container<br>Children: ID, DisplayName<br>Ancestor: Upload
StorageClass|存储类型<br>Type: String<br>Ancestor: Upload
Initiated|分片上传创建的时间。<br>Type: Date<br>Ancestor:Upload
ListMultipartUploadsResult.Prefix|当请求中指定前缀时，这该字段包含指定前缀。结果仅含指定前缀开始的Object。Type: String<br>Ancestor: ListMultipartUploadsResult
Delimiter|请求中指定的分隔符。<br>Type: String<br>Ancestor: ListMultipartUploadsResult
CommonPrefixes|如果在请求中指定分隔符，则CommonPrefixes中返回包含delimiter的前缀。Type: Container<br>Ancestor: ListMultipartUploadsResult
CommonPrefixes.Prefix|如果请求中不包含Prefix参数，则此元素仅显示从第一次出现分隔符之前的字符串。如果请求中包含Prefix参数，则此元素显示从前缀开始到第一次出现分隔符之前的字符串。<br>Type: String<br>Ancestor: CommonPrefixes

## 示例
### 请求示例
```
GET /?uploads&max-uploads=3 HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Mon, 1 Nov 2010 20:34:56 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 1330
Connection: keep-alive
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Bucket>bucket</Bucket>
  <KeyMarker></KeyMarker>
  <UploadIdMarker></UploadIdMarker>
  <NextKeyMarker>my-movie.m2ts</NextKeyMarker>
  <NextUploadIdMarker>YW55IGlkZWEgd2h5IGVsdmluZydzIHVwbG9hZCBmYWlsZWQ</NextUploadIdMarker>
  <MaxUploads>3</MaxUploads>
  <IsTruncated>true</IsTruncated>
  <Upload>
    <Key>my-divisor</Key>
    <UploadId>XMgbGlrZSBlbHZpbmcncyBub3QgaGF2aW5nIG11Y2ggbHVjaw</UploadId>
    <Initiator>
      <ID>arn:aws:iam::111122223333:user/user1-11111a31-17b5-4fb7-9df5-b111111f13de</ID>
      <DisplayName>user1-11111a31-17b5-4fb7-9df5-b111111f13de</DisplayName>
    </Initiator>
    <Owner>
      <ID>75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a</ID>
      <DisplayName>OwnerDisplayName</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
    <Initiated>2010-11-10T20:48:33.000Z</Initiated>  
  </Upload>
  <Upload>
    <Key>my-movie.m2ts</Key>
    <UploadId>VXBsb2FkIElEIGZvciBlbHZpbmcncyBteS1tb3ZpZS5tMnRzIHVwbG9hZA</UploadId>
    <Initiator>
      <ID>b1d16700c70b0b05597d7acd6a3f92be</ID>
      <DisplayName>InitiatorDisplayName</DisplayName>
    </Initiator>
    <Owner>
      <ID>b1d16700c70b0b05597d7acd6a3f92be</ID>
      <DisplayName>OwnerDisplayName</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
    <Initiated>2010-11-10T20:48:33.000Z</Initiated>
  </Upload>
  <Upload>
    <Key>my-movie.m2ts</Key>
    <UploadId>YW55IGlkZWEgd2h5IGVsdmluZydzIHVwbG9hZCBmYWlsZWQ</UploadId>
    <Initiator>
      <ID>arn:aws:iam::444455556666:user/user1-22222a31-17b5-4fb7-9df5-b222222f13de</ID>
      <DisplayName>user1-22222a31-17b5-4fb7-9df5-b222222f13de</DisplayName>
    </Initiator>
    <Owner>
      <ID>b1d16700c70b0b05597d7acd6a3f92be</ID>
      <DisplayName>OwnerDisplayName</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
    <Initiated>2010-11-10T20:49:33.000Z</Initiated>
  </Upload>
</ListMultipartUploadsResult>
```
