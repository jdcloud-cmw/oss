# Put Bucket replication

## 描述
该操作在指定Bucket中创建新的复制配置（或替换原有复制配置）。

## 请求
### 语法
```
PUT /?replication HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Content-Length: <length>
Date: <date>
Authorization: <authorization string> 
Content-MD5: <MD5>

Replication configuration XML in the body
```

### 请求参数
无请求参数
### 请求Header

名称|描述|必须
---|---|---
Content-MD5|对128位MD5进行base64编码。该Header用来确定请求实体在传输中没有损坏。<br>Type: String<br>Default: None|是

### 请求实体
可在请求实体中指定一个或多个复制规则。
```
<ReplicationConfiguration>
    <Role>IAM-role-ARN</Role>
    <Rule>
        <ID>Rule-1</ID>
        <Status>rule-status</Status>
        <Prefix>key-prefix</Prefix>
        <Destination>        
           <Bucket>arn:aws:s3:::bucket-name</Bucket>
           <StorageClass>optional-destination-storage-class-override</StorageClass>     
        </Destination>    
    </Rule>
    <Rule>
        <ID>Rule-2</ID>
         ...
    </Rule>
     ...
</ReplicationConfiguration>
```

名称|描述|需要
---|---|---
ReplicationConfiguration|复制规则的集合。最多1000条规则。总大小不超过2MB。<br>Type: Container<br>Children: Rule<br>Ancestor: None|是
Rule|特性规则的信息集合。<br>Type: Container<br>Ancestor:ReplicationConfiguration|是
ID|规则标识。<br>Type: String<br>Ancestor: Rule|否
Status|规则状态<br>Type: String<br>Ancestor: Rule<br>Valid values: Enabled, Disabled|是
Prefix|匹配前缀，最长不超过1024个字符，前缀不能重叠。<br>Type: String<br>Ancestor: Rule|是
Destination|目的信息集合<br>Type: Container<br>Ancestor: Rule|是
Bucket|目的地Bucket，多个复制规则必须指定同一个Bucket。<br>Type: String<br>Ancestor: Destination|是
StorageClass|指定复制Object的存储类型，未指定存储类型，则使用源Object存储类型。<br>Type: String<br>Ancestor: Destination<br>Default: Storage class of the source object.<br>Valid values: STANDARD、REDUCED_REDUNDANCY|否

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素
### 特殊错误

HTTP Error|Code|Cause
---|---|---
HTTP 400|InvalidRequest|<Account> element must be specified if the <Owner> in <AccessControlTranslation> has a value.
HTTP 400|InvalidArgument|<Account> element is empty and must contain a valid account ID.
HTTP 400|InvalidArgument|The AWS account specified in the <Account> element must match the destination bucket owner.

## 示例
### 请求示例
```
PUT /?replication HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Wed, 11 Feb 2015 02:11:21 GMT
Content-MD5: q6yJDlIkcBaGGfb3QLY69A==
Authorization: <authorization string>
Content-Length: 406

<ReplicationConfiguration>
  <Role>arn:aws:iam::35667example:role/CrossRegionReplicationRoleForS3</Role>
  <Rule>
    <ID>rule1</ID>
    <Prefix>TaxDocs</Prefix>
    <Status>Enabled</Status>
    <Destination>
      <Bucket>arn:aws:s3:::exampletargetbucket</Bucket>
    </Destination>
  </Rule>
</ReplicationConfiguration>
```

### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 9E26D08072A8EF9E
Date: Wed, 11 Feb 2015 02:11:22 GMT
Content-Length: 0
Server: JDCloudOSS
```
