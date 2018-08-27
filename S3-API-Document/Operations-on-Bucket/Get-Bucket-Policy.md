# Get Bucket Policy
  
## 描述
该操作可返回指定Bukcet的policy。仅Bukcet的Owner可操作。
## 请求
### 语法
```
GET /?policy HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```
### 请求参数
无请求参数
### 请求Header
无特殊响应Header
### 请求元素
无请求元素

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例
### 请求示例
```
GET ?policy HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Wed, 28 Oct 2009 22:32:00 GMT
Authorization: <authorization string>
```
### 响应示例
```
HTTP/1.1 200 OK   
x-amz-request-id: 656c76696e67SAMPLE57374  
Date: Tue, 04 Apr 2010 20:34:56 GMT  
Connection: keep-alive  
Server: JDCloudOSS  

{
"Version":"2008-10-17",
"Id":"aaaa-bbbb-cccc-dddd",
"Statement" : [
    {
        "Effect":"Deny",
        "Sid":"1", 
        "Principal" : {
            "AWS":["111122223333","444455556666"]
        },
        "Action":["s3:*"],
        "Resource":"arn:aws:s3:::bucket/*"
    }
 ] 
}
```
