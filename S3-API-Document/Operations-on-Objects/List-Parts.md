# List Parts

## 描述
该操作可列出指定分片上传下所有的分片。该请求必须包含Upload ID。该请求最多返回1000个分片，返回分片数量默认为1000，可通过指定max-parts参数来限制返回数量。如果分段上传超过1000个分片，响应中将返回IsTruncated，且值为true，以及NextPartNumberMarker元素。在之后的请求中，您可以使用part-number-marker参数并其值设置为上一个响应中NextPartNumberMarker中字段值。

## 请求
### 语法
```
GET /ObjectName?uploadId=UploadId HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string>
```

### 请求参数

参数|描述|必须
---|---|---
encoding-type|请求OSS对响应进行编码并指定编码方法。<br>Type: String<br>Default: None<br>Valid value: url|否
uploadID|标识某一分片上传。<br>Type: String<br>Default: None|是
max-parts|设置分片的最大返回数。<br>Type: String<br>Default: 1,000|否
part-number-marker|指定列出列表某处之后的分片。<br>Type: String<br>Default: None|否

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
x-amz-abort-date|如果Bucket的生命周期规则配置了中止不完成的分片上传（put bucket lifecycle上线后支持），且规则中前缀与请求中Object名称匹配，则响应包含此Header。<br>Type: String
x-amz-abort-rule-id|与x-amz-abort-date一起返回，用来标识生命周期规则。<br>Type: String
ListPartsResult|响应信息集合。<br>Children: Bucket, Key, UploadId, Initiator, Owner, StorageClass, PartNumberMarker, NextPartNumberMarker, MaxParts, IsTruncated, Part<br>Type: Container
Bucket|分片上传所在Bucket。Type: String<br>Ancestor: ListPartsResult
Encoding-Type|在XML响应中Object名称的编码类型，如果请求指定encoding-type参数，则响应中包含此元素。Type: String<br>Ancestor: ListBucketResult
key|分片上传的Object。<br>Type: String<br>Ancestor: ListPartsResult
UploadID|标识分片上传。<br>Type: String<br>Ancestor: ListPartsResult
Initiator|标识启动的分片上传的用户信息的集合。<br>Children: ID, DisplayName<br>Type: Container<br>Ancestor: ListPartsResult
ID|用户ID<br>Type: String<br>Ancestor: Initiator
DisplayName|用户名<br>Type: String<br>Ancestor: Initiator
Owner|Object拥有者的信息集合。<br>Children: ID, DisplayName<br>Type: Container<br>Ancestor: ListPartsResult
StorageClass|存储类型<br>Type: String<br>Ancestor: ListPartsResult
PartNumberMarker|列出某一number后的分片。<br>Type: Integer<br>Ancestor: ListPartsResult
NextPartNumberMarker|当part数超出后，该元素指定列表中下一个分片，该值作为下个请求中的part-number-marker参数。<br>Type: Integer<br>Ancestor: ListPartsResult
MaxParts|返回分片最大数量。<br>Type: Integer<br>Ancestor: ListPartsResult
IsTruncated|指示返回的分片列表是否截断。true代表被截断。<br>Type: Boolean<br>Ancestor: ListPartsResult
Part|分片信息集合。<br>Children: PartNumber, LastModified, ETag, Size<br>Type: String<br>Ancestor: ListPartsResult
PartNumber|分片标识号。<br>Type: Integer<br>Ancestor: Part
LastModified|分片上传到的时间。<br>Type: Date<br>Ancestor: Part
ETag|分片上传返回的实体标签。<br>Type: String<br>Ancestor: Part
Size|分片大小<br>Type: Integer<br>Ancestor: Part

## 示例
### 请求示例
```
GET /example-object?uploadId=XXBsb2FkIElEIGZvciBlbHZpbmcncyVcdS1tb3ZpZS5tMnRzEEEwbG9hZA&max-parts=2&part-number-marker=1 HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Mon, 1 Nov 2010 20:34:56 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 656c76696e6727732072657175657374
Date: Mon, 1 Nov 2010 20:34:56 GMT
Content-Length: 985
Connection: keep-alive
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<ListPartsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Bucket>example-bucket</Bucket>
  <Key>example-object</Key>
  <UploadId>XXBsb2FkIElEIGZvciBlbHZpbmcncyVcdS1tb3ZpZS5tMnRzEEEwbG9hZA</UploadId>
  <Initiator>
      <ID>arn:aws:iam::111122223333:user/some-user-11116a31-17b5-4fb7-9df5-b288870f11xx</ID>
      <DisplayName>umat-user-11116a31-17b5-4fb7-9df5-b288870f11xx</DisplayName>
  </Initiator>
  <Owner>
    <ID>75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a</ID>
    <DisplayName>someName</DisplayName>
  </Owner>
  <StorageClass>STANDARD</StorageClass>
  <PartNumberMarker>1</PartNumberMarker>
  <NextPartNumberMarker>3</NextPartNumberMarker>
  <MaxParts>2</MaxParts>
  <IsTruncated>true</IsTruncated>
  <Part>
    <PartNumber>2</PartNumber>
    <LastModified>2010-11-10T20:48:34.000Z</LastModified>
    <ETag>"7778aef83f66abc1fa1e8477f296d394"</ETag>
    <Size>10485760</Size>
  </Part>
  <Part>
    <PartNumber>3</PartNumber>
    <LastModified>2010-11-10T20:48:33.000Z</LastModified>
    <ETag>"aaaa18db4cc2f85cedef654fccc4a4x8"</ETag>
    <Size>10485760</Size>
  </Part>
</ListPartsResult>
```
