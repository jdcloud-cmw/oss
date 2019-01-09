# Put Bucket website

# 描述
指定某个Bucket设置静态网站托管配置，包括网站索引文件、错误页面及重定向规则，仅Bukcet的Owner可操作。

## 请求
### 语法
```
PUT /?website HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Content-Length: <ContentLength>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))

<WebsiteConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <!-- website configuration information. -->
</WebsiteConfiguration>
```
### 请求参数
无请求参数
### 请求Header
无特殊Header
### 请求元素
您可以将Bucket设置为静态网站托管模式，可指定默认首页、错误返回页及访问发生错误后的重定向地址。

名称|描述|必须
---|---|---
WebsiteConfiguration|website信息集合。<br>Type: Container<br>Ancestors: None|是
IndexDocument|suffix元素信息集合<br>Type: Container<br>Ancestors: WebsiteConfiguration|是
Suffix|指定索引文件，不得为空且不能包含"/"字符，如index.html。<br>Type: String<br>Ancestors: WebsiteConfiguration.IndexDocument|是
ErrorDocument|key元素信息集合<br>Type: Container<br>Ancestors: WebsiteConfiguration|否
Key|当4XX错误发生时，OSS返回的自定义错误页面。<br>Type: String<br>Ancestors: WebsiteConfiguration.ErrorDocument<br>Condition: Required when ErrorDocument is specified.|指定ErrorDocument是必须
RoutingRules|RoutingRule元素信息集合。<br>Type: Container<br>Ancestors: WebsiteConfiguration|否
RoutingRule|指定重定向规则。在RoutingRules规则中，至少包含一个RoutingRule规则。<br>Type: String<br>Ancestors: WebsiteConfiguration.RoutingRules|否
Condition|重定向条件信息集合。<br>Type: Container<br>Ancestors: WebsiteConfiguration.RoutingRules.RoutingRule|否
HttpErrorCodeReturnedEquals|HTTP错误代码，指定错误码发生时，则进行重定向。范围为400-499，不能<br>Type: String<br>Ancestors: WebsiteConfiguration.RoutingRules.RoutingRule.Condition|否
Redirect|重定向信息的集合。<br>Type: String<br>Ancestors: WebsiteConfiguration.RoutingRules.RoutingRule|否
HostName|重定向请求的Host。Type: String<br>Ancestors: WebsiteConfiguration.RoutingRules.RoutingRule.Redirect|否
ReplaceKeyPrefixWith|将替换重定向请求中的 KeyPrefixEquals 值的对象键名称的前缀。<br>Type: String<br>Ancestors: WebsiteConfiguration.RoutingRules.RoutingRule.Redirect|否

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素

## 示例
### 示例1
以下请求将example.com配置为website。请求中将index.html指定为索引页，将SomeErrorDocument.html指定为错误页面。
```
PUT ?website HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Content-Length: 256
Date: Thu, 27 Jan 2011 12:00:00 GMT
Authorization: <authorization string>

<WebsiteConfiguration xmlns='http://s3.amazonaws.com/doc/2006-03-01/'>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>SomeErrorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```
返回示例
```
HTTP/1.1 200 OK
x-amz-request-id: 80CD4368BD211111
Date: Thu, 27 Jan 2011 00:00:00 GMT
Content-Length: 0
Server: JDCloudOSS
```

### 示例2
该请求中指定RoutingRule中HTTP错误码为特定条件，并指定重定向到ec2-11-22-333-44.compute-1.amazonaws.com/reprot-404/ 下。例如，如果请求ExamplePage.html发生404错误时，将会重定向到 report-404/testPage.html。如果无重定向规则，则返回Error.html。
```
PUT ?website HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Content-Length: 580
Date: Thu, 27 Jan 2011 12:00:00 GMT
Authorization: <authorization string>

<WebsiteConfiguration xmlns='http://s3.amazonaws.com/doc/2006-03-01/'>
  <IndexDocument>
    <Suffix>index.html</Suffix>
  </IndexDocument>
  <ErrorDocument>
    <Key>Error.html</Key>
  </ErrorDocument>

  <RoutingRules>
    <RoutingRule>
    <Condition>
      <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals >
    </Condition>
    <Redirect>
      <HostName>ec2-11-22-333-44.compute-1.amazonaws.com</HostName>
      <ReplaceKeyPrefixWith>report-404/</ReplaceKeyPrefixWith>
    </Redirect>
    </RoutingRule>
  </RoutingRules>
</WebsiteConfiguration>
```
