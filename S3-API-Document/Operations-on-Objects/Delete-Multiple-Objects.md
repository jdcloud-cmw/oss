# Delete Multiple Objects

## 描述
该操作可以通过单个HTTP请求删除多个Object，最多包含1000个Object，如果未找到请求中指定的Object，OSS将会返回为已删除。

批量删除支持两种响应方式，分别为verbose方式、quiet方式。默认为verbose方式，该响应包含所有Object的删除结果；如果指定quiet模式，则响应仅包含发生错误的Object。

## 请求
### 语法
```
POST /?delete HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Authorization: <authorization string>
Content-Length: <Size>
Content-MD5: <MD5>

<?xml version="1.0" encoding="UTF-8"?>
<Delete>
    <Quiet>true</Quiet>
    <Object>
         <Key>Key</Key>
         <VersionId>VersionId</VersionId>
    </Object>
    <Object>
         <Key>Key</Key>
    </Object>
    ...
</Delete>			
```

### 请求参数
无请求参数
### 请求Header

名称|描述|必须
---|---|---
Content-MD5|对128位MD5进行base64编码。该Header用来确定请求实体在传输中没有损坏。<br>Type: String<br>Default: None|是
Content-Length|实体长度<br>Type: String<br>Default: None|是

### 请求元素

名称|描述|请求
---|---|---
Delete|请求集合。<br>Ancestor: None<br>Type: Container<br>一个或多个Object元素和可选的Quiet元素。|是
Quiet|开启quiet模式，当添加此元素时，必须指定该值为true。<br>Ancestor: Delete<br>Type: Boolean<br>Default: false|否
Object|删除请求中指定Object的集合。<br>Ancestor: Delete<br>Type: Container<br>Children: Key是
Key|object名称<br>Ancestor: Object<br>Type: String|是

## 响应
### 请求Header
无请求Header
### 请求元素

名称|描述
---|---
DeleteResult|响应集合<br>Children: Deleted, Error<br>Type: Container<br>Ancestor: None
Deleted|已成功删除的元素集合。<br>Children: Key<br>Type: Container<br>Ancestor: DeleteResult
Key|执行删除操作的Object名称。<br>Type: String<br>Ancestor: Deleted, or Error
Error|错误信息集合。<br>Children: Key, Code, Message.<br>Type: String<br>Ancestor: DeleteResult
Code|删除失败返回的状态码。<br>Type: String<br>Values: AccessDenied, InternalError<br>Ancestor: Error
Message|错误描述<br>Type: String<br>Ancestor: Error

## 示例
### 请求示例
```
POST /?delete HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Accept: */*
x-amz-date: Wed, 30 Nov 2011 03:39:05 GMT
Content-MD5: p5/WA/oEr30qrEEl21PAqw==
Authorization: <authorization string>
Content-Length: 125
Connection: Keep-Alive

<Delete>
  <Object>
    <Key>sample1.txt</Key>
  </Object>
  <Object>
    <Key>sample2.txt</Key>
  </Object>
</Delete>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: A437B3B641629AEE
Date: Fri, 02 Dec 2011 01:53:42 GMT
Content-Type: application/xml
Server: JDCloudOSS
Content-Length: 251

<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Deleted>
    <Key>sample1.txt</Key>
  </Deleted>
  <Error>
    <Key>sample2.txt</Key>
    <Code>AccessDenied</Code>
    <Message>Access Denied</Message>
  </Error>
</DeleteResult>
```
