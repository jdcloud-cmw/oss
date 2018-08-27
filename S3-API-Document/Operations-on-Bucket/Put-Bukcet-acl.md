# Put Bucket acl

## 描述
该操作可指定Bucket的访问控制列表（acl），仅Bukcet的Owner可操作。

您可以使用以下两种方法指定Bucket的权限（无法同时使用）：
* 在请求body中指定ACL
* 在请求Header指定permissions

## 请求
### 语法
以下请求为在请求body中发送ACL。如要使用Header指定permissions，请参阅“请求Header”部分。
```
PUT /?acl HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))

<AccessControlPolicy>
  <Owner>  // Owner目前为无关项，OSS不做检查
    <ID>ID</ID>
    <DisplayName>EmailAddress</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>ID</ID>
        <DisplayName>EmailAddress</DisplayName>
      </Grantee>
      <Permission>Permission</Permission>
    </Grant>
    ...
  </AccessControlList>
</AccessControlPolicy> 
```
### 请求参数
无请求参数
### 请求Header
除公共请求Header外，您还可以使用以下Header：

通过Header您可以使用以下两种方法设置访问权限：
* 指定固定的ACL
* 明确指定每个被授权者的权限

OSS支持一组预定义的ACL（固定ACL），每种固定ACL都有一组预定义的被授权者和权限。要使用固定ACL设置访问权限，请使用以下Header，并将固定ACL名称指定与其值。如果使用此Header，则无法在请求中使用其他用于访问控制的Header。

名称|描述|必须
---|---|---
x-amz-acl|指定固定ACL设置Bucket的ACL。<br>Type: String<br>Valid Values: private、public-read、public-read-write<br>Default: private|否

如果需要指定每个被授权者的权限，需指定"x-amz-grant-permission",如果使用了该Header，则不能使用"x-amz-acl"设置固定ACL。

名称|描述|必须
---|---|---
x-amz-grant-read|对被授权者指定READ权限。<br>Type: String<br>Default: None<br>Constraints: None|否
x-amz-grant-write|对被授权者指定WRITE权限。<br>Type: String<br>Default: None<br>Constraints: None|否
x-amz-grant-full-control|对被授权者指定全部权限（READ、WRITE）<br>Type: String<br>Default: None<br>Constraints: None|否

每个Header的值是一个或多个被授予者。您可指定每个被授予者为一对type-value。支持type如下：

* id — if value specified is the canonical User ID of an JDCLOUD account

* uri — if granting permission to a predefined JDCLOUD group

注：当前OSS支持的Grantee类型是CanonicalUser（用户UserID）；仅支持一种Group类型：AllUsers。

### 请求元素
如果您使用请求body指定acl，则必须使用以下元素：

名称|描述|必须
---|---|---
AccessControlList|acl信息集合<br>Type: Container<br>Ancestors: AccessControlPolicy|否
AccessControlPolicy|每个被授予人的ACL元素集合。<br>Type: String<br>Ancestors: None|否
DisplayName|Bucket拥有者名称。<br>Type: String<br>Ancestors: AccessControlPolicy.Owner|否
Grant|被授权者及权限信息集合<br>Type: Container<br>Ancestors: AccessControlPolicy.AccessControlList|否
Grantee|被授权者<br>Type: String<br>Ancestors: AccessControlPolicy.AccessControlList.Grant|否
ID|bucket拥有者或者被授权者的ID<br>Type: String<br>Ancestors: AccessControlPolicy.Owner<br>AccessControlPolicy.AccessControlList.Grant|否
Owner|bucket拥有者的信息集合<br>Type: Container<br>Ancestors: AccessControlPolicy|是
Permission|指定的权限<br>Type: String<br>Valid Values: FULL_CONTROL、WRITE、READ <br>Ancestors: AccessControlPolicy.AccessControlList.Grant|否

### Grantee Values
您可以通过以下方式使用请求为被授权者授予权限：
* 通过person's ID
```
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser"><ID><replaceable>ID</replaceable></ID><DisplayName><replaceable>GranteesEmail</replaceable></DisplayName>
</Grantee>
```
* 通过URL
```
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group"><URI><replaceable>http://acs.amazonaws.com/groups/global/AllUsers</replaceable></URI></Grantee>
```
注：当前OSS支持的Grantee类型是CanonicalUser（用户UserID）；仅支持一种Group类型：AllUsers。

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例

### 示例（在请求body中指定acl）
#### 请求示例
```
PUT ?acl HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Content-Length: 1660
x-amz-date: Thu, 12 Apr 2012 20:04:21 GMT
Authorization: <authorization string>

<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>  // 无关项
    <ID>852b113e7a2f25102679df27bb0ae12b3f85be6BucketOwnerCanonicalUserID</ID>  
    <DisplayName>OwnerDisplayName</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>852b113e7a2f25102679df27bb0ae12b3f85be6BucketOwnerCanonicalUserID</ID>
        <DisplayName>OwnerDisplayName</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group">
        <URI xmlns="">http://acs.amazonaws.com/groups/global/AllUsers</URI>
      </Grantee>
      <Permission xmlns="">READ</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```
#### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: C651BC9B4E1BD401
Date: Thu, 12 Apr 2012 20:04:28 GMT
Content-Length: 0
Server: JDCloudOSS
```

### 示例（在Header中指定acl）
#### 请求示例
```
PUT ?acl HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: Sun, 29 Apr 2012 22:00:57 GMT
x-amz-grant-write: uri="http://acs.amazonaws.com/groups/s3/AllUsers"
x-amz-grant-read: uri="http://acs.amazonaws.com/groups/global/AllUsers"
Accept: */*
Authorization: <authorization string>
```
#### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: A6A8F01A38EC7138
Date: Sun, 29 Apr 2012 22:01:10 GMT
Content-Length: 0
Server: JDCloudOSS
```
