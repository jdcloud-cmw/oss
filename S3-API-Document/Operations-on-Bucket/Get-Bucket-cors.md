# Get Bucket cors

## 描述
返回指定Bucket中cors配置信息，仅Bukcet的Owner可操作。

## 请求
### 语法
```
GET /?cors HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
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

名称|描述
---|---
CORSConfiguration|CORSRule元素的集合，最多100个。<br>Type: Container<br>Children: CORSRules<br>Ancestor: None
CORSRule|允许的来源和方法。最多100个。<br>Type: Container<br>Children: AllowedOrigin, AllowedMethod, MaxAgeSeconds, ExposeHeader, ID.<br>Ancestor: CORSConfiguration|是
ID|每条规则的唯一标识ID。ID最长255个字符。<br>Type: String<br>Ancestor: CORSRule
AllowedMethod|允许来源执行的HTTP方法。<br>Type: Enum (GET, PUT, HEAD, POST, DELETE)<br>Ancestor: CORSRule
AllowedOrigin|允许跨域请求的来源，支持通配符"*",且最多包含一个通配符<br>Type: String<br>Ancestor: CORSRule
AllowedHeader|指定预请求中Access-Control-Request-Headers允许的Header。OSS将会返回请求中允许的Header。<br>Type: String<br>Ancestor: CORSRule
MaxAgeSeconds|指定浏览器缓存OSS对预请求的响应时间，最多1个<br>Type: Integer (seconds)<br>Ancestor: CORSRule
ExposeHeader|暴露给浏览器的header列表，即用户从应用程序中访问的响应头。<br>Type: String<br>Ancestor: CORSRule

## 示例
### 请求示例
```
GET /?cors HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Tue, 13 Dec 2011 19:14:42 GMT
Authorization: <authorization string>
```

### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 0CF038E9BCF63097
Date: Tue, 13 Dec 2011 19:14:42 GMT
Server: JDCloudOSS
Content-Length: 280

<CORSConfiguration>
     <CORSRule>
       <AllowedOrigin>http://www.example.com</AllowedOrigin>
       <AllowedMethod>GET</AllowedMethod>
       <MaxAgeSeconds>3000</MaxAgeSec>
       <ExposeHeader>x-amz-server-side-encryption</ExposeHeader>
     </CORSRule>
</CORSConfiguration>
```
