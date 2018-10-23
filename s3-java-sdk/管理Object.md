# 管理Obcjet

本节介绍如何使用S3 SDK完成管理Objcet常见操作，包括上传Obcjet、下载Obcjet、删除Obcjet、批量删除Object、复制\移动\重命名Object等。

## 上传Obcjet
```java
String bucket_name = "<your bucketname>";
String file_path = "<your path>";
String key = Paths.get(file_path).getFileName().toString();

try {
    s3.putObject(bucket_name, key, new File(file_path));
    System.out.format("Uploading %s to S3 bucket %s...\n", key, bucket_name);
} catch (AmazonServiceException e) {
    System.err.println(e.getErrorMessage());
    System.exit(1);
} 
System.out.println("Done!");
```

## 下载Object

```java
String bucket_name = "<your bucketname>";
String key = "your keyname";
System.out.format("Downloading %s from S3 bucket %s...\n", key, bucket_name);

try {
    S3Object o = s3.getObject(bucket_name, key);
    S3ObjectInputStream s3is = o.getObjectContent();
    FileOutputStream fos = new FileOutputStream(new File(key));
    byte[] read_buf = new byte[1024];
    int read_len = 0;
    while ((read_len = s3is.read(read_buf)) > 0) {
        fos.write(read_buf, 0, read_len);
    }
    s3is.close();
    fos.close();
} catch (AmazonServiceException e) {
    System.err.println(e.getErrorMessage());
    System.exit(1);
} catch (FileNotFoundException e) {
    System.err.println(e.getMessage());
    System.exit(1);
} catch (IOException e) {
    System.err.println(e.getMessage());
    System.exit(1);
}
    System.out.println("Done!");    	
```

## 删除Object

```java
String bucket_name = "<your bucketname>";
String key = "<your keyname>";

try {
    s3.deleteObject(bucket_name, key);
} catch (AmazonServiceException e) {
    System.err.println(e.getErrorMessage());
    System.exit(1);
}
```

## 复制\移动\重命名Object

```java
String from_bucket = "<your source bucketname>";
String to_bucket = "<your destination bucket name>";
String object_key = "<your keyname>";

try {
    s3.copyObject(from_bucket, object_key, to_bucket, object_key1);
} catch (AmazonServiceException e) {
    System.err.println(e.getErrorMessage());
    System.exit(1);
}
```
注：您可以将 copyObject 与 deleteObject 配合使用来移动或重命名Object，方式是先将Object复制到新名称 (您可以使用与源和目标相同的Bucket)，然后从旧位置删除Obcjet。

## 批量删除Obcjets

```java
String bucket_name = "<your bucketname>";
String[] object_keys = {"keyname1","keyname2","..." };

try {
    DeleteObjectsRequest dor = new DeleteObjectsRequest(bucket_name)
        .withKeys(object_keys);
    s3.deleteObjects(dor);
} catch (AmazonServiceException e) {
    System.err.println(e.getErrorMessage());
    System.exit(1);
}
```
