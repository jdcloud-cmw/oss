# PUT Bucket notification

## 描述
OSS支持[回调通知](https://docs.jdcloud.com/cn/object-storage-service/callback-notification-2)功能，您可以指定某些资源发生相关操作时及时进行消息通知。OSS回调通知是异步进行的，不影响OSS操作。

您可通过PUT Bucket notification指定某个Bucket创建或更改回调通知配置。回调通知配置(NotificationConfiguration)为XML格式，默认情况下，您的Bucket未配置回调通知，所以回调通知配置(NotificationConfiguration)为空，您可以通过添加空的NotificationConfiguration来关闭回调通知。
```
<NotificationConfiguration>
</NotificationConfiguration>
```

## 请求
### 语法
```xml
PUT /?notification HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version 4))

<NotificationConfiguration>
    <TopicConfiguration>
        <Id>ConfigurationId</Id>
        <Filter>
            <S3Key>
                <FilterRule>
                    <Name>prefix</Name>
                    <Value>prefix-value</Value>
                </FilterRule>
                <FilterRule>
                    <Name>suffix</Name>
                    <Value>suffix-value</Value>
                </FilterRule>
           </S3Key>
        </Filter>
        <Topic>NS:endpoint1,endpoint2</Topic>
        <Event>event-type</Event>
        <Event>event-type</Event>
        ...
    </TopicConfiguration>
    <CloudFunctionConfiguration>   
        <Id>ConfigurationId</Id>
        <Filter>
	        ...
        </Filter>
        <CloudFunction>function-id</CloudFunction>    
        <Event>event-type</Event> 
        ...         
    </CloudFunctionConfiguration>  
    ...
</NotificationConfiguration>
```

### 请求参数
无请求参数
### 请求Header
无特殊请求Header
### 请求元素

名称|描述|必须
---|---|---
NotificationConfiguration|指定bucket通知配置的容器，如果这个元素为空，那么此bucket的通知功能被关闭。<br>Type: Container<br>Children: one or more TopicConfiguration elements.<br>Ancestor: None|是
TopicConfiguration|包含通知服务的相关配置，通过配置可以使OSS发送事件消息给topic。<br>事件触发时，将通过Object和事件类型对TopicConfiguration按序依次匹配，匹配成功则发出消息通知，且匹配终止。<br>Type: Container<br>Children: An Id, Filter, Topic, and one, or more Event.<br>Ancestor: NotificationConfiguration|否
Id|TopicConfiguration的唯一标识符，是可选的，如果没有设置OSS将会随机分配Id ，多个TopicConfiguration的id不能重复。<br>Type: String<br>Ancestor: TopicConfiguration|否
Topic|当指定事件发生时，OSS会向此topic发布一条消息。格式 NS:endpoint1,endpoint2,endpoint3 (其中必须以 'NS:' 开头，后面紧跟回调服务器的地址，多个服务器地址用 ‘,’ 分隔，最多允许配置5个回调服务器)<br>Type: String<br>Ancestor: TopicConfiguration|当TopicConfiguration存在时必须存在
Event|事件类型，用于当事件发生时OSS发送通知。 <br>Type: String<br>Valid Values: 详细见《OSS事件通知支持事件类型》<br>Ancestor: TopicConfiguration|当TopicConfiguration存在时必须存在
Filter|S3Key的容器。其中S3Key包含着object key name的筛选规则。Type: Container<br>Children: S3Key<br>Ancestor: TopicConfiguration|否
FilterRule|包含定义筛选规则标准的键值对。<br>Type: Container<br>Children: Name and Value<br>Ancestor: S3Key|否
Name|prefix 或 suffix，即用于根据object key name筛选一个或多个object。前缀和后缀最大长度为1022个字节。<br>Type: String<br>Ancestor: FilterRule<br>Valid values: prefix or suffix|否
Value|指定要筛选的object key name的前缀或后缀。Type: String<br>Ancestor: FilterRule|否
CloudFunction|需要触发的Function ID。当指定的事件发生时调用函数服务。<br>Type: String<br>Ancestor: CloudFunctionConfiguration|否
CloudFunctionConfiguratio|CloudFunction触发规则。<br>Type: Container<br>Children: An Id,Filter, CloudFunction, and one, or more Event.<br>Ancestor: NotificationConfiguration|否

## 响应
### 响应Header
无特殊响应Header
### 响应元素
无响应元素
### 特殊错误
HTTP Error|Code|描述
---|---|---
HTTP 400 Bad Request|InvalidArgument|下列条件可导致错误：<br>1.指定的事件类型不支持<br>2.Topic命名不符合要求<br>3.Topic不存在，验证失败
HTTP 403 Forbidden|AccessDenied|您不是该Bucket的拥有者

## 示例
### 请求示例
```xml
PUT /?notification HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com
Date: <date>
Authorization: <authorization string> 

<NotificationConfiguration>
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
### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: BB1BA8E12D6A80B7
Date: Mon, 13 Oct 2014 22:58:44 GMT
Content-Length: 0
Server: JDCloudOSS
```


