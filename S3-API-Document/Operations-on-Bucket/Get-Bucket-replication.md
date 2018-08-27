# Get Bucket replication

## 描述
返回指定Bucket的复制配置信息。

## 请求
### 语法
```
GET /?replication HTTP/1.1
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
### 响应元素
该操作响应元素同Put Bucket replication中请求元素。

### 特殊错误

Error Code|描述|Http Status Code|SOAP Fault Code Prefix
---|---|---|---
NoSuchReplicationConfiguration|The replication configuration does not exist.|404 Not Found|Client

## 示例
### 请求示例
```
GET /?replication HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: Tue, 10 Feb 2015 00:17:21 GMT
Authorization: <authorization string>
```

### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 51991C342example
Date: Tue, 10 Feb 2015 00:17:23 GMT
Server: JDCloudOSS
Content-Length: <length>

<?xml version="1.0" encoding="UTF-8"?>
<ReplicationConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Rule>
    <ID>rule1</ID>
    <Status>Enabled</Status>
    <Prefix></Prefix>
    <Destination>
      <Bucket>arn:aws:s3:::exampletargetbucket</Bucket>
      <StorageClass>STANDARD_IA</StorageClass>
    </Destination>
  </Rule>
  <Role>arn:aws:iam::35667example:role/CrossRegionReplicationRoleForS3</Role>
</ReplicationConfiguration>
```
