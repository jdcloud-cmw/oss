# Put Bucket

## 描述
该操作可实现创建一个Bucket；创建的Bucket所在的Region和发送请求的Endpoint所对应的Region一致。Bucket所在Region确定后，该Bucket下的所有Object将一直存放在对应的地区。每个Region最多创建20个Bucket。

Bucket命名规则：<br>
1.长度必须在3-63字符之间<br>
2.名称仅能由小写字母、数字、中划线(-)组成<br>
3.名称必须以小写字母或数字开头和结尾<br>

## 请求

## 语法
```
PUT / HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Content-Length: <length>
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))

<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"> 
  <LocationConstraint>BucketRegion</LocationConstraint> 
</CreateBucketConfiguration>
```
### 请求参数
该操作无请求参数

### 请求Header
该操作仅使用通用的请求Header，请参阅常见请求Header。

### 请求元素
无请求元素

## 响应

## 响应元素
该操作无响应元素

## 示例

### 请求示例
```
PUT / HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Content-Length: 0
Date: Wed, 01 Mar  2006 12:00:00 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 236A8905248E5A01
Date: Wed, 01 Mar  2006 12:00:00 GMT

Location: /colorpictures
Content-Length: 0
Connection: close
Server: JDCloudOSS
```
