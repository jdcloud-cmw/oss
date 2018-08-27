# Get Service

## 描述

该操作对服务地址进行Get请求，可以返回请求者拥有的所有Bucket。

该操作要求对请求者进行身份验证，您必须使用在京东云注册的AccessKey与AccessKeySecret完成验证，匿名请求无法列出Bucket。

## 请求

### 语法

```
GET / HTTP/1.1
Host: s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```

### 请求参数

该操作不需要请求参数。

### 请求Header

该操作仅使用通用的请求Header，请参阅常见请求Header。

### 请求元素

该操作不需要请求元素。

## 响应

### 响应元素

 名称 | 描述
---|---
 Bucket | Bucket信息的集合<br>Type: Container<br>Children:Name,CreationDate<br>Ancestor:ListAllMyBucketsResult.Buckets
 Buckets | 多个Bukcet信息的集合<br>Type: Container<br>Children: Bucket<br>Ancestor: ListAllMyBucketsResult
 CreationDate | Bucket的创建日期<br>Type: date ( of the form yyyy-mm-ddThh:mm:ss.timezone, e.g., 2009-02-03T16:45:09.000Z)<br>Ancestor: ListAllMyBucketsResult.Buckets.Bucket
 DispalyName | Bucket拥有者的名称<br>Type: String<br>Ancestor: ListAllMyBucketsResult.Owner
 ID | Bucket拥有者的ID<br>Type: String<br>Ancestor: ListAllMyBucketsResult.Owner
 ListAllMyBucketsResult | 响应集合<br>Children: Owner, Buckets<br>Ancestor: None
 Name | Bucket名称<br>Type: String<br>Ancestor: ListAllMyBucketsResult.Buckets.Bucket
 Owner | Bucket拥有者信息集合<br>Type: Container<br>Ancestor: ListAllMyBucketsResult

## 示例

### 请求示例
```
GET / HTTP/1.1
Host: s3.<region>.jcloudcs.com
Date: Wed, 01 Mar  2006 12:00:00 GMT
Authorization: <authorization string>
```

### 返回示例
```
<?xml version="1.0" encoding="UTF-8"?>
<ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01">
  <Owner>
    <ID>bcaf1ffd86f461ca5fb16fd081034f</ID>
    <DisplayName>webfile</DisplayName>
  </Owner>
  <Buckets>
    <Bucket>
      <Name>quotes</Name>
      <CreationDate>2006-02-03T16:45:09.000Z</CreationDate>
    </Bucket>
    <Bucket>
      <Name>samples</Name>
      <CreationDate>2006-02-03T16:41:58.000Z</CreationDate>
    </Bucket>
  </Buckets>
</ListAllMyBucketsResult>
```
