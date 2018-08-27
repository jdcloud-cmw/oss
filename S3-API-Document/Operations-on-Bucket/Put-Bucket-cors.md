# Put Bucket Cors

## 描述
为指定Bucket添加cors配置。如果配置存在，OSS将会替换它，仅Bukcet的Owner可操作。您可以在Bucket上设置此配置，以便存储桶可以为跨域访问提供服务。

cors规则以XML文本形式展示，包含来源和HTTP方法。该文本最大64KB。例如，Bucket的cors配置有以下两个规则：
* 第一条CORSRule允许来自https://www.example.com 的跨域PUT、POST、DELETE请求。该规则还允许通过Access-Control-Request-Headers发起OPTIONS预请求，因此，为了响应OPTIONS预请求，OSS将会返回所有的请求Header。
* 第二条CORSRule允许所有跨域的GET请求，"*"通配符指所有来源。
```
<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>http://www.example.com</AllowedOrigin>

   <AllowedMethod>PUT</AllowedMethod>
   <AllowedMethod>POST</AllowedMethod>
   <AllowedMethod>DELETE</AllowedMethod>

   <AllowedHeader>*</AllowedHeader>
 </CORSRule>
 <CORSRule>
   <AllowedOrigin>*</AllowedOrigin>
   <AllowedMethod>GET</AllowedMethod>
 </CORSRule>
</CORSConfiguration>
```
cors配置还允许其他可选参数，如下所示：
```
<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>http://www.example.com</AllowedOrigin>
   <AllowedMethod>PUT</AllowedMethod>
   <AllowedMethod>POST</AllowedMethod>
   <AllowedMethod>DELETE</AllowedMethod>
   <AllowedHeader>*</AllowedHeader>
   <MaxAgeSeconds>3000</MaxAgeSeconds>
   <ExposeHeader>x-amz-server-side-encryption</ExposeHeader>
 </CORSRule>
</CORSConfiguration>
```
CORSRule包括以下附加可选参数：
* MaxAgeSeconds-指定浏览器缓存OSS对预请求的响应时间，在示例中，该参数为3000秒。缓存能避免浏览器重复像OSS发起OPTIONS预请求。
* ExposeHeader-暴露给浏览器的header列表，即用户从应用程序中访问的响应头。
当OSS收到跨域请求（OPTIONS预请求），会根据Bucket中cors配置，并根据CORSRule依次匹配来自浏览器的请求。匹配成功需以下条件：
* 请求中的Origin头必须匹配AllowedOrigin元素
* 请求方法或预请求方法必须匹配AllowedMethod元素
* 所有预请求中的Access-Control-Request-Headers头必须匹配AllowedHeader元素

## 请求
### 语法
```
PUT /?cors HTTP/1.1
Host: <Bucket>.s3.<region>.jcloudcs.com 
Content-Length: <length>
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
Content-MD5: <MD5>

<CORSConfiguration>
  <CORSRule>
    <AllowedOrigin>Origin you want to allow cross-domain requests from</AllowedOrigin>
    <AllowedOrigin>...</AllowedOrigin>
    ...
    <AllowedMethod>HTTP method</AllowedMethod>
    <AllowedMethod>...</AllowedMethod>
    ...
    <MaxAgeSeconds>Time in seconds your browser to cache the pre-flight OPTIONS response for a resource</MaxAgeSeconds>
    <AllowedHeader>Headers that you want the browser to be allowed to send</AllowedHeader>
    <AllowedHeader>...</AllowedHeader>
     ...
    <ExposeHeader>Headers in the response that you want accessible from client application</ExposeHeader>
    <ExposeHeader>...</ExposeHeader>
     ...
  </CORSRule>
  <CORSRule>
    ...
  </CORSRule>
    ...
</CORSConfiguration>
```
### 请求参数
无请求参数
### 请求Header

名称|描述|必须
---|---|---
Content-MD5|对报文主体进行MD5算法获得128位二进制数，在通过Base64编码写入Content-MD5。可用于数据完整性检查。|是

### 请求元素

名称|描述|必须
---|---|---
CORSConfiguration|CORSRule元素的集合，最多100个。<br>Type: Container<br>Children: CORSRules<br>Ancestor: None|是
CORSRule|允许的来源和方法。最多100个。<br>Type: Container<br>Children: AllowedOrigin, AllowedMethod, MaxAgeSeconds, ExposeHeader, ID.<br>Ancestor: CORSConfiguration|是
ID|每条规则的唯一标识ID。ID最长255个字符。<br>Type: String<br>Ancestor: CORSRule|否
AllowedMethod|允许来源执行的HTTP方法。<br>Type: Enum (GET, PUT, HEAD, POST, DELETE)<br>Ancestor: CORSRule|是
AllowedOrigin|允许跨域请求的来源，支持通配符"*",且最多包含一个通配符<br>Type: String<br>Ancestor: CORSRule|是
AllowedHeader|指定预请求中Access-Control-Request-Headers允许的Header。OSS将会返回请求中允许的Header。<br>Type: String<br>Ancestor: CORSRule|否
MaxAgeSeconds|指定浏览器缓存OSS对预请求的响应时间，最多1个<br>Type: Integer (seconds)<br>Ancestor: CORSRule|否
ExposeHeader|暴露给浏览器的header列表，即用户从应用程序中访问的响应头。<br>Type: String<br>Ancestor: CORSRule|否

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例
### 请求示例
```
PUT /?cors HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: Tue, 21 Aug 2012 17:54:50 GMT
Content-MD5: 8dYiLewFWZyGgV2Q5FNI4W==
Authorization: authorization string
Content-Length: 216

<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>http://www.example.com</AllowedOrigin>
   <AllowedMethod>PUT</AllowedMethod>
   <AllowedMethod>POST</AllowedMethod>
   <AllowedMethod>DELETE</AllowedMethod>
   <AllowedHeader>*</AllowedHeader>
   <MaxAgeSeconds>3000</MaxAgeSec>
   <ExposeHeader>x-amz-server-side-encryption</ExposeHeader>
 </CORSRule>
 <CORSRule>
   <AllowedOrigin>*</AllowedOrigin>
   <AllowedMethod>GET</AllowedMethod>
   <AllowedHeader>*</AllowedHeader>
   <MaxAgeSeconds>3000</MaxAgeSeconds>
 </CORSRule>
</CORSConfiguration>
```

### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: BDC4B83DF5096BBE
Date: Tue, 21 Aug 2012 17:54:50 GMT
Server: JDCloudOSS
```
