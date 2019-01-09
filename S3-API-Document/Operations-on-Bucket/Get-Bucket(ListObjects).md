# Get Bucket(List Objects)Version 2

## 描述
此操作可列举出Bukcet中的部分或全部（最多1000个）Object。您可以使请求参数作为选择过滤条件来列举部分对象。

注意：即使Bucket的访问权限为public，也不允许匿名使用此操作，需要指定Authorization。

## 请求

### 语法
```
GET /?list-type=2 HTTP/1.1
Host: <bucket>.s3.<region>.jcloudcs.com 
Date: <date>
Authorization: <authorization string> (see Authenticating Requests (AWS Signature Version4))
```

### 请求参数
参数|描述|必须
---|---|---
delimiter| 是一个用于对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现delimiter字符之间的object作为一组元素——CommonPrefixes。如果未指定prefix，则从名字开头算起。<br>Type: String<br>Default: None|否
encoding-type| 对响应进行编码并指定编码类型。<br>Objcet Key可以包含任何Unicode字符。但是，XML1.0无法解析某些字符，例如ASCII值为0到10的字符。对于XML 1.0中不支持的字符，您可以添加此参数以请求OSS对响应中的键进行编码。<br>Type: String<br>Default: None<br>Valid value: url|否
max-keys| 限定此次返回object的最大数。如果要检索少于默认的1,000个object，可以将其添加到您的请求中。<br>响应中object数量如果大于设定的最大数量，则超出的object不会返回，且响应中将包含<IsTruncated>true</IsTruncated>。如果要返回额外超出的object，请参阅 NextContinuationToken。<br>Type: String<br>Default: 1000|否
prefix| 限定返回的object必须以prefix作为前缀。您可以使用prefix将object分成不同的组。<br>Type: String<br>Default: None|否
list-type| API Version 2需要此参数，且该参数值必须设置为2。<br>Type: String<br>Default:2|是
continuation-token| 当响应元素 IsTruncated 为true时，则响应中会包含 NextContinuationToken 元素，您可以使用 NextContinuationToken 作为下一个请求中的 continuation-token 返回下一批object。<br>Type: String<br>Default: None|否
fetch-owner| 默认情况下不会返回Bucket所有者信息，如果需要返回所有者信息，需指定fetch-owner值为ture<br>Type: String<br>Default: false|否
start-after| 如果您需要返回某个特定的object后的相关object，你可以将该特定object名称设为参数值。<br>Type: String<br>Default: None|否

### 请求元素
无请求元素

### 请求Header
该操作仅使用通用的请求Header，请参阅常见请求Header。

## 响应
### 响应Header
该操作仅使用通用的响应Header，请参阅常见响应Header。

### 响应元素
名称|描述
---|---
Contents| 每个object返回的元数据<br>Type: XML metadata<br>Ancestor: ListBucketResult
CommonPrefixes| 在返回时，将所有包含指定prefix且第一次出现delimiter的object作为单个CommonPrefixes返回。<br>只有在指定delimiter时返回CommonPrefixes。<br>Type: String<br>Ancestor: ListBucketResult
Delimiter| 是一个用于对Object名字进行分组的字符。<br>Type: String<br>Ancestor: ListBucketResult
DisplayName| object拥有者的名称<br>Type: String<br>Ancestor: ListBucketResult.Contents.Owner
Encoding-Type| 对响应进行编码并指定编码类型。<br>Type: String<br>Ancestor: ListBucketResult
ETag | ETag 是京东云存储中Object的Hash值，其仅反映Object的数据内容，而不包括元数据(Metadata)。<br>Type: String<br>Ancestor: ListBucketResult.Contents
ID| object拥有者的ID。<br>Type: String<br>Ancestor: ListBucketResult.Contents.Owner
IsTruncated| 如果object数量超过MaxKeys值，则更多的object可能返回，为ture；如果所有结果都返回，则为false。<br>Type: Boolean<br>Ancestor: ListBucketResult
Key| object key<br>Type: String<br>Ancestor: ListBucketResult.Contents
LastModified | object最后修改时间。<br>Type: Date<br>Ancestor: ListBucketResult.Contents
MaxKeys| 限定此次返回object的最大数。<br>Type: Date<br>Ancestor: ListBucketResult.Contents
Name| bucket的名称。<br>Type: String<br>Ancestor: ListBucketResult
Owner| bucket拥有者。<br>Type: String<br>Children: DisplayName, ID<br>Ancestor: ListBucketResult.Contents | CommonPrefixes
Prefix |限定返回的object key必须以prefix作为前缀。<br>Type: String<br>Ancestor: ListBucketResult
Size| object大小。<br>Type: String<br>Ancestor: ListBucketResult.Contents
StorageClass| 存储类型：STANDARD、REDUCED_REDUNDANCY<br>Type: String<br>Ancestor: ListBucketResult.Contents
ContinuationToken| 请求中包含此元素，则响应中也包含此元素。<br>Type: String<br>Ancestor: ListBucketResult
KeyCount| 响应中返回的object数量。<br>Type: String<br>Ancestor: ListBucketResult
NextContinuationToken| 如果响应被截断，OSS将会返回延续Token。你可以使用该Token作为下一条请求中的continuation-token来取回剩余的object。<br>Type: String<br>Ancestor: ListBucketResult
StartAfter| 请求中包含此元素，则响应中也包含此元素。<br>Type: String<br>Ancestor: ListBucketResult

## 示例

### 示例1：Listing Keys
该请求可返回某bucket中的object。该请求指定list-type元素为2。
#### 请求示例
```
GET /?list-type=2 HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: 20160430T233541Z
Authorization: <authorization string>
Content-Type: text/plain
```
#### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 3B3C7C725673C630
Date: Sat, 30 Apr 2016 23:29:37 GMT
Content-Type: application/xml
Content-Length: length
Connection: close
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Name>oss-example</Name>
    <Prefix/>
    <KeyCount>205</KeyCount>
    <MaxKeys>1000</MaxKeys>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>my-image.jpg</Key>
        <LastModified>2009-10-12T17:50:30.000Z</LastModified>
        <ETag>&quot;fba9dede5f27731c9771645a39863328&quot;</ETag>
        <Size>434234</Size>
        <StorageClass>STANDARD</StorageClass>
    </Contents>
    <Contents>
       ...
    </Contents>
    ...
</ListBucketResult>
```
### 示例2：Listing Keys（使用max-keys,prefix,start-after元素）
#### 请求示例
```
GET /?list-type=2&max-keys=3&prefix=E&start-after=ExampleGuide.pdf HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: 20160430T232933Z
Authorization: <authorization string>
```
#### 响应示例
```
HTTP/1.1 200 OK
x-amz-request-id: 3B3C7C725673C630
Date: Sat, 30 Apr 2016 23:29:37 GMT
Content-Type: application/xml
Content-Length: length
Connection: close
Server: JDCloudOSS

<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>oss-example</Name>
  <Prefix>E</Prefix>
  <StartAfter>ExampleGuide.pdf</StartAfter>
  <KeyCount>1</KeyCount>
  <MaxKeys>3</MaxKeys>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>ExampleObject.txt</Key>
    <LastModified>2013-09-17T18:07:53.000Z</LastModified>
    <ETag>&quot;599bab3ed2c697f1d26842727561fd94&quot;</ETag>
    <Size>857</Size>
    <StorageClass>REDUCED_REDUNDANCY</StorageClass>
  </Contents>
</ListBucketResult>
```
### 示例3：Listing Keys（使用prefix和delimiter元素）
该示例请求中使用prefix和delimiter元素，假如bucket中有以下object：<br>
sample.jpg<br>
photos/2006/January/sample.jpg<br>
photos/2006/February/sample2.jpg<br>
photos/2006/February/sample3.jpg<br>
photos/2006/February/sample4.jpg<br>

**以下示例指定delimiter值为"/"：**
```
GET /?list-type=2&delimiter=/ HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: 20160430T235931Z
Authorization: <authorization string>			
```
sample.jpg不包含delimiter字符，所以OSS将它返回到Contents元素中。其他object包含delimiter字符，且包含共同的prefix:photos/，所以OSS将其作为单个CommonPrefixes元素返回。
```
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>oss-example</Name>
  <Prefix></Prefix>
  <KeyCount>2</KeyCount>
  <MaxKeys>1000</MaxKeys>
  <Delimiter>/</Delimiter>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>sample.jpg</Key>
    <LastModified>2011-02-26T01:56:20.000Z</LastModified>
    <ETag>&quot;bf1d737a4d46a19f3bced6905cc8b902&quot;</ETag>
    <Size>142863</Size>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
  <CommonPrefixes>
    <Prefix>photos/</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

**以下示例指定delimiter值为"/"，prefix值为"photos/2006/"**
```
GET /?list-type=2&prefix=photos/2006/&delimiter=/ HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
x-amz-date: 20160501T000433Z
Authorization: <authorization string>
```
在响应中，OSS将会返回指定的prefix，并将包含prefix且第一次出现delimiter字符的不同字符串作为不同的CommonPrefixes进行分组并返回。
```
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>oss-example</Name>
  <Prefix>photos/2006/</Prefix>
  <KeyCount>3</KeyCount>
  <MaxKeys>1000</MaxKeys>
  <Delimiter>/</Delimiter>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>photos/2006/</Key>
    <LastModified>2016-04-30T23:51:29.000Z</LastModified>
    <ETag>&quot;d41d8cd98f00b204e9800998ecf8427e&quot;</ETag>
    <Size>0</Size>
    <StorageClass>STANDARD</StorageClass>
  </Contents>

  <CommonPrefixes>
    <Prefix>photos/2006/February/</Prefix>
  </CommonPrefixes>
  <CommonPrefixes>
    <Prefix>photos/2006/January/</Prefix>
  </CommonPrefixes>
</ListBucketResult>
```

### 示例4：使用Continuation Token
在这个示例中，初次请求返回object数量超过1000个。在响应中，OSS返回了值为true的IsTruncated元素以及NextContinuationToken元素。
```
GET /?list-type=2 HTTP/1.1
Host: oss-example.s3.<region>.jcloudcs.com 
Date: Mon, 02 May 2016 23:17:07 GMT
Authorization: <authorization string>
```

以下为响应示例：
```
HTTP/1.1 200 OK
x-amz-request-id: 3B3C7C725673C630
Date: Sat, 30 Apr 2016 23:29:37 GMT
Content-Type: application/xml
Content-Length: length
Connection: close
Server: JDCloudOSS

<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>oss-example</Name>
  <Prefix></Prefix>
  <NextContinuationToken>1ueGcxLPRx1Tr/XYExHnhbYLgveDs2J/wm36Hy4vbOwM=</NextContinuationToken>
  <KeyCount>1000</KeyCount>
  <MaxKeys>1000</MaxKeys>
  <IsTruncated>true</IsTruncated>
  <Contents>
    <Key>happyface.jpg</Key>
    <LastModified>2014-11-21T19:40:05.000Z</LastModified>
    <ETag>&quot;70ee1738b6b21e2c8a43f3a5ab0eee71&quot;</ETag>
    <Size>11</Size>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
   ...
</ListBucketResult>
```

如下随后的请求中，我们加入了continuation-token作为请求参数，并将之前返回的<NextContinuationToken> 作为该参数值。
```
GET /?list-type=2 HTTP/1.1
GET /?list-type=2&continuation-token=1ueGcxLPRx1Tr/XYExHnhbYLgveDs2J/wm36Hy4vbOwM= HTTP/1.1

Host: oss-example.s3.<region>.jcloudcs.com 
Date: Mon, 02 May 2016 23:17:07 GMT
Authorization: <authorization string>  
```
在如下返回示例中，OSS将返回上次请求超出未返会的object。
```
HTTP/1.1 200 OK
x-amz-request-id: 3B3C7C725673C630
Date: Sat, 30 Apr 2016 23:29:37 GMT
Content-Type: application/xml
Content-Length: length
Connection: close
Server: JDCloudOSS

<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Name>oss-example</Name>
  <Prefix></Prefix>
  <ContinuationToken>1ueGcxLPRx1Tr/XYExHnhbYLgveDs2J/wm36Hy4vbOwM=</ContinuationToken>
  <KeyCount>112</KeyCount>
  <MaxKeys>1000</MaxKeys>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>happyfacex.jpg</Key>
    <LastModified>2014-11-21T19:40:05.000Z</LastModified>
    <ETag>&quot;70ee1738b6b21e2c8a43f3a5ab0eee71&quot;</ETag>
    <Size>1111</Size>
    <StorageClass>STANDARD</StorageClass>
  </Contents>
   ...
</ListBucketResult>
```
