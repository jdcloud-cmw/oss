# GET Bucket notification

## 描述
该操作可返回指定Bucket的回调通知配置（NotificationConfiguration），如果未配置通知，则操作将返回空NotificationConfiguration元素。

## 请求
### 语法
```
GET /?notification HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version 4))
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
同PUT Bucket notification中请求元素

## 示例
### 请求示例
```
GET ?notification HTTP/1.1 
Host: oss-example.s3.<region>.jcloudcs.com
Date: Wed, 15 Oct 2014 16:59:03 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 236A8905248E5A02
Date: Wed, 15 Oct 2014 16:59:04 GMT
Server: JDcloudOSS
<?xml version="1.0" encoding="UTF-8"?>

<NotificationConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <TopicConfiguration>
        <Id>1</Id>
        <Filter>
            <S3Key>
                <FilterRule>
                    <Name>prefix</Name>
                    <Value>images/</Value>
                </FilterRule>
                <FilterRule>
                    <Name>suffix</Name>
                    <Value>.jpg</Value>
                </FilterRule>
           </S3Key>
        </Filter>
        <Topic>NS:http://116.196.97.17:1601/post/callback</Topic>
        <Event>s3:ObjectCreated:Put</Event>
    </TopicConfiguration>
</NotificationConfiguration>
```
