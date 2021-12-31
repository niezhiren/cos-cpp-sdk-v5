## 下载与安装

#### 编译选项

根目录下的CMakeLists.txt可以配置编译选项，有如下编译选项：

```shell
option(BUILD_UNITTEST "Build unittest" OFF) #配置编译单元测试
option(BUILD_DEMO "Build demo" ON) #配置编译demo测试代码
option(BUILD_SHARED_LIB "Build shared library" OFF) #配置编译动态库
```

#### 库依赖

依赖动态库：poco、openssl、crypto。

#### 直接使用SDK（无需编译)

下载 [XML C++ SDK 源码](https://github.com/tencentyun/cos-cpp-sdk-v5) ，libs目录中有编译好的库，可以直接使用，使用时请选择对应的系统版本。

```shell
libs/linux/libcossdk.a #linux的静态库，libcossdk.a是基于gcc version 4.8.5版本编译的，如果客户编译环境的gcc版本不同，需要重新编译libcossdk.a
libs/linux/libcossdk-shared.so #linux动态库
libs/Win32/cossdk.lib #Win32库
libs/x64/cossdk.lib #Win64库
libs/macOS/libcossdk.a #macOS静态库
libs/macOS/libcossdk-shared.dylib #macOS动态库
```

> 使用时请将对应系统的库文件以及sdk include头文件拷贝至您的工程中。

third-party目录下有第三方依赖库

```shell
third_party/lib/linux/poco/ #linux下依赖的poco动态库，poco库是基于OpenSSL 1.0.2版本编译的，如果客户编译环境的openssl版本不同，需要重新编译poco
third_party/lib/Win32/openssl/ #Win32依赖的openssl库
third_party/lib/Win32/poco/ #Win32依赖的poco库
third_party/lib/x64/openssl/ #Win64依赖的openssl库
third_party/lib/x64/poco/ #Win64依赖的poco库
third_party/lib/macOS/poco/ #macOS依赖的poco库
```

> 使用时也请将对应系统的依赖库拷贝至您的工程中。

#### 编译 Linux 版本 SDK

#### 1. 安装编译工具及依赖库

```shell
yum install -y gcc gcc-c++ make cmake openssl
#cmake版本要求大于2.8
```

#### 2. 编译SDK 

下载 [XML C++ SDK 源码](https://github.com/tencentyun/cos-cpp-sdk-v5) 到您的开发环境，并执行以下命令：

```shell
cd ${cos-cpp-sdk} 
mkdir -p build 
cd build 
cmake .. 
make
```

#### 3. 安装Poco 库

```shell
cd ${cos-cpp-sdk} 
sh install-libpoco.sh
```

> 该脚本将Poco动态库安装到/usr/lib64目录中，并创建软链接，如果要在生产环境中使用cos sdk，也需要安装Poco库到生产环境中。

#### 4. 测试demo 

如果不需要测试demo，可跳过此步骤。

```shell
cd ${cos-cpp-sdk} 
vim demo/cos_demo.cpp  #修改demo中的存储桶名以及测试代码
vim CMakeLists.txt #修改根目录下的CMakeLists.txt中的BUILD_DEMO为ON，开启编译demo
cd build && make #编译demo
ls bin/cos_demo #生成的可执行文件在bin目录
vim bin/config.json #修改密钥和园区
./bin/cos_demo #运行demon
```

#### 5. 使用SDK 

编译生成的库文件在build/lib目录中，静态库名称为`libcossdk.a`， 动态库名称为`libcossdk-shared.so`。使用时请将库拷贝至您的工程中，同时将include目录拷贝至您的工程中的 include 路径下。


### 编译 Windows 版本 SDK

#### 1. 安装 visual studio 2017

安装 visual studio 2017 开发环境。

#### 2. 安装 CMake 工具

从 [CMake官网](https://cmake.org/download/) 下载 Windows 版本的 CMake 编译工具，并将 `${CMake的安装路径}\bin`，配置在 Windows 的系统环境变量 Path 中。

#### 3. 编译SDK 

i. 下载 [XML C++ SDK 源码](https://github.com/tencentyun/cos-cpp-sdk-v5) 到您的开发环境。

ii. 打开 Windows 的命令行，cd 到 C++ SDK 的源码目录下，执行命令

```shell
mkdir build
cd build
cmake .. #生成Win32 makefile
cmake -G "Visual Studio 15 Win64" .. #生成Win64 makefile
```

iii. 使用 visual studio 2017 打开解决方案，进行编译。

#### 4. 测试demo 

如果不需要测试demo，可跳过此步骤。

修改demo代码，并编译，生成的cos_demo.exe在bin目录中，修改bin/config.json可运行cos_demo.exe。

#### 5. 使用SDK 

编译生成的库文件在build/Release目录中，静态库名称为`cossdk.lib`。使用时请将库拷贝至您的工程中，同时将include目录拷贝至您的工程中的 include 路径下。

### 编译 Mac 版本 SDK

#### 1. 安装编译工具及依赖库
```shell
brew install gcc make cmake openssl
```

#### 2. 编译SDK 

下载 [XML C++ SDK 源码](https://github.com/tencentyun/cos-cpp-sdk-v5) 到您的开发环境，并执行以下命令：

```shell
cd ${cos-cpp-sdk} 
mkdir -p build 
cd build 
cmake .. 
make
```

#### 3. 安装Poco 库

Poco库在third_party/lib/macOS/poco目录下，请自行安装。

#### 4. 测试demo 

如果不需要测试demo，可跳过此步骤。

修改demo代码，并编译，生成的cos_demo在bin目录中，修改bin/config.json可运行cos_demo。

#### 5. 使用SDK 

编译生成的库文件在build/lib目录中，静态库名称为`libcossdk.a`， 动态库名称为`libcossdk-shared.dylib`。使用时请将库拷贝至您的工程中，同时将include目录拷贝至您的工程中的 include 路径下。

### 常见编译问题

1. 编译可执行程序的时候提示错误:

   PocoCrypto.so.64: undefined reference to `PEM_write_bio_PrivateKey@libcrypto.so.10'
   libPocoNetSSL.so.64: undefined reference to `X509_check_host@libcrypto.so.10'
   ibPocoCrypto.so.64: undefined reference to `ECDSA_sign@OPENSSL_1.0.1_EC'
   libPocoCrypto.so.64: undefined reference to `CRYPTO_set_id_callback@libcrypto.so.10'
   ibPocoCrypto.so.64: undefined reference to `EVP_PKEY_id@libcrypto.so.10'
   libPocoNetSSL.so.64: undefined reference to `SSL_get1_session@libssl.so.10'
   libPocoNetSSL.so.64: undefined reference to `SSL_get_shutdown@libssl.so.10'
   libPocoCrypto.so.64: undefined reference to `EVP_PKEY_set1_RSA@libcrypto.so.10'
   libPocoCrypto.so.64: undefined reference to `SSL_load_error_strings@libssl.so.10'

   这种情况一般是工程里自带的poco库的编译依赖的ssl版本与客户机器上的版本不一致导致的，需要用户重新编译poco库，并替换掉third_party里的poco库。

```shell
wget https://github.com/pocoproject/poco/archive/refs/tags/poco-1.9.4-release.zip
cd poco-poco-1.9.4-release/
./configure --omit=Data/ODBC,Data/MySQL
mkdir my_build
cd my_build
cmake .. 
make -j5
```

2. 编译poco库的时候无法编译出PocoNetSSL库，一般是因为机器没装openssl-devel库

```shell
yum install -y openssl-devel
```

3. 编译可执行程序的时候提示错误：

   undefined reference to `qcloud_cos::CosConfig::CosConfig(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)

   这种情况一般是因为工程自带的libcossdk.a编译使用的gcc版本与客户机器上的gcc版本不一致导致的，需要客户重新编译poco库和libcossdk。

## 开始使用

下面为您介绍如何使用 COS C++ SDK 完成一个基础操作，如初始化客户端、创建存储桶、查询存储桶列表、上传对象、查询对象列表、下载对象和删除对象。

>? 关于文章中出现的 SecretId、SecretKey、Bucket 等名称的含义和获取方式请参见 [COS 术语信息](https://cloud.tencent.com/document/product/436/7751#.E6.9C.AF.E8.AF.AD.E4.BF.A1.E6.81.AF)。
>

### 初始化

配置文件各字段介绍：

```
"SecretId":"********************************",  // sercret_id替换为用户的 SecretId，登录访问管理控制台查看密钥，https://console.cloud.tencent.com/cam/capi
"SecretKey":"*******************************", // sercret_key替换为用户的 SecretKey，登录访问管理控制台查看密钥，https://console.cloud.tencent.com/cam/capi
"Region":"ap-guangzhou",                 // 存储桶地域, 替换为客户存储桶所在地域，可以在COS控制台指定存储桶的概览页查看存储桶地域信息，参考 https://console.cloud.tencent.com/cos5/bucket/ ，关于地域的详情见 https://cloud.tencent.com/document/product/436/6224
"SignExpiredTime":360,              // 签名超时时间, 单位s
"ConnectTimeoutInms":6000,          // connect超时时间, 单位ms
"ReceiveTimeoutInms":60000,         // recv超时时间, 单位ms
"UploadPartSize":10485760,          // 上传文件分片大小，1M~5G, 默认为10M
"UploadCopyPartSize":20971520,      // 上传复制文件分片大小，5M~5G, 默认为20M
"UploadThreadPoolSize":5,           // 单文件分块上传线程池大小
"DownloadSliceSize":4194304,        // 下载文件分片大小
"DownloadThreadPoolSize":5,         // 单文件下载线程池大小
"AsynThreadPoolSize":2,             // 异步上传下载线程池大小
"LogoutType":1,                     // 日志输出类型,0:不输出,1:输出到屏幕,2输出到syslog
"LogLevel":3,                       // 日志级别:1: ERR, 2: WARN, 3:INFO, 4:DBG
"IsDomainSameToHost":false,         // 是否使用专有的host
"DestDomain":"",                    // 特定host
"IsUseIntranet":false,              // 是否使用特定ip和端口号
"IntranetAddr":""                   // 特定ip和端口号,例如“127.0.0.1:80”               
```

### 使用自定义域名访问 COS

在 config.json 中增加如下配置：

```cpp
"IsDomainSameToHost":true,
"DestDomain":"mydomain.com",
```

### 使用临时密钥访问 COS

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"
int main(int argc, char *argv[]) {
    qcloud_cos::CosConfig config("./config.json");
    // 如果使用永久密钥不需要填入token，如果使用临时密钥需要填入，临时密钥生成和使用指引参见https://cloud.tencent.com/document/product/436/14048
    config.SetTmpToken("xxx");
    qcloud_cos::CosAPI cos(config);
}
```

### 自定义Log回调

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"
void TestLogCallback(const std::string& log) {
    std::ofstream ofs;
    ofs.open("test.log", std::ios_base::app);
    ofs << log;
    ofs.close();
}
int main(int argc, char** argv) {
    qcloud_cos::CosConfig config("./config.json");
    config.SetLogCallback(&TestLogCallback);
    qcloud_cos::CosAPI cos(config);
}
```

### 创建存储桶

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造创建存储桶的请求
    std::string bucket_name = "examplebucket-1250000000"; // Bucket名称
    qcloud_cos::PutBucketReq req(bucket_name);
    qcloud_cos::PutBucketResp resp;
    
    // 3. 调用创建存储桶接口
    qcloud_cos::CosResult result = cos.PutBucket(req, &resp);
    
    // 4. 处理调用结果
    if (result.IsSucc()) {
        // 创建成功
    } else {
        // 创建存储桶失败，可以调用 CosResult 的成员函数输出错误信息，例如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}

```

### 查询存储桶列表

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造查询存储桶列表的请求
    qcloud_cos::GetServiceReq req;
    qcloud_cos::GetServiceResp resp;
    qcloud_cos::CosResult result = cos.GetService(req, &resp);
    
    // 3. 获取响应信息
    const qcloud_cos::Owner& owner = resp.GetOwner();
    const std::vector<qcloud_cos::Bucket>& buckets = resp.GetBuckets();
    std::cout << "owner.m_id=" << owner.m_id << ", owner.display_name=" << owner.m_display_name << std::endl;
    
    for (std::vector<qcloud_cos::Bucket>::const_iterator itr = buckets.begin(); itr != buckets.end(); ++itr) {
        const qcloud_cos::Bucket& bucket = *itr;
        std::cout << "Bucket name=" << bucket.m_name << ", location="
            << bucket.m_location << ", create_date=" << bucket.m_create_date << std::endl;
    }
    
    // 4. 处理调用结果
    if (result.IsSucc()) {
        // 查询存储桶列表成功
    } else {
        // 查询存储桶列表失败，可以调用 CosResult 的成员函数输出错误信息，比如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}
```

### 上传对象

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造上传文件的请求
    std::string bucket_name = "examplebucket-1250000000"; // 上传的目的 Bucket 名称
    std::string object_name = "exampleobject"; //exampleobject 即为对象键（Key），是对象在存储桶中的唯一标识。例如，在对象的访问域名 examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/pic.jpg 中，对象键为 doc/pic.jpg。
    // request 的构造函数中需要传入本地文件路径
    qcloud_cos::PutObjectByFileReq req(bucket_name, object_name, "/path/to/local/file");
    req.SetXCosStorageClass("STANDARD_IA")； // 调用 Set 方法设置元数据等
    qcloud_cos::PutObjectByFileResp resp;
    
    // 3. 调用上传文件接口
    qcloud_cos::CosResult result = cos.PutObject(req, &resp);
    
    // 4. 处理调用结果
    if (result.IsSucc()) {
        // 上传文件成功
    } else {
        // 上传文件失败，可以调用 CosResult 的成员函数输出错误信息，比如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}
```

### 查询对象列表

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造查询对象列表的请求
    std::string bucket_name = "examplebucket-1250000000"; // 上传的目标存储桶名称
    qcloud_cos::GetBucketReq req(bucket_name);
    qcloud_cos::GetBucketResp resp;
    qcloud_cos::CosResult result = cos.GetBucket(req, &resp);   
    
    std::vector<qcloud_cos::Content> cotents = resp.GetContents();
    for (std::vector<qcloud_cos::Content>::const_iterator itr = cotents.begin(); itr != cotents.end(); ++itr) {
    	const qcloud_cos::Content& content = *itr;
        std::cout << "key name=" << content.m_key << ", lastmodified ="
            << content.m_last_modified << ", size=" << content.m_size << std::endl;
    }
    
    // 3. 处理调用结果
    if (result.IsSucc()) {
        // 查询对象列表成功
    } else {
        // 查询对象列表失败，可以调用 CosResult 的成员函数输出错误信息，例如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}
```

### 下载对象

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造下载对象的请求
    std::string bucket_name = "examplebucket-1250000000"; // 上传的目的 Bucket 名称
    std::string object_name = "exampleobject"; // exampleobject 即为对象键（Key），是对象在存储桶中的唯一标识。例如，在对象的访问域名 examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/pic.jpg 中，对象键为 doc/pic.jpg。
    std::string local_path = "/tmp/exampleobject";
    // request 需要提供 appid、bucketname、object,以及本地的路径（包含文件名）
    qcloud_cos::GetObjectByFileReq req(bucket_name, object_name, local_path);
    qcloud_cos::GetObjectByFileResp resp;
    
    // 3. 调用下载对象接口
    qcloud_cos::CosResult result = cos.GetObject(req, &resp);
    
    // 4. 处理调用结果
    if (result.IsSucc()) {
        // 下载文件成功
    } else {
        // 下载文件失败，可以调用 CosResult 的成员函数输出错误信息，例如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}
```

### 删除对象

```cpp
#include "cos_api.h"
#include "cos_sys_config.h"
#include "cos_defines.h"

int main(int argc, char *argv[]) {
    // 1. 指定配置文件路径，初始化 CosConfig
    qcloud_cos::CosConfig config("./config.json");
    qcloud_cos::CosAPI cos(config);
    
    // 2. 构造删除对象的请求
    std::string bucket_name = "examplebucket-1250000000"; // 上传的目标存储桶名称
    std::string object_name = "exampleobject"; // exampleobject 即为对象键（Key），是对象在存储桶中的唯一标识。例如，在对象的访问域名 examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com/doc/pic.jpg 中，对象键为 doc/pic.jpg。
    // 3. 调用删除对象接口
	qcloud_cos::DeleteObjectReq req(bucket_name, object_name);
	qcloud_cos::DeleteObjectResp resp;
	qcloud_cos::CosResult result = cos.DeleteObject(req, &resp); 
    
    // 4. 处理调用结果
    if (result.IsSucc()) {
        // 删除对象成功
    } else {
        // 删除对象失败，可以调用 CosResult 的成员函数输出错误信息，例如 requestID 等
        std::cout << "ErrorInfo=" << result.GetErrorMsg() << std::endl;
        std::cout << "HttpStatus=" << result.GetHttpStatus() << std::endl;
        std::cout << "ErrorCode=" << result.GetErrorCode() << std::endl;
        std::cout << "ErrorMsg=" << result.GetErrorMsg() << std::endl;
        std::cout << "ResourceAddr=" << result.GetResourceAddr() << std::endl;
        std::cout << "XCosRequestId=" << result.GetXCosRequestId() << std::endl;
        std::cout << "XCosTraceId=" << result.GetXCosTraceId() << std::endl;
    }
}
```
