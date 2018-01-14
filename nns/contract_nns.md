# 顶级域名合约

## 顶级域名合约的函数签名

函数签名如下

    public static object Main(string method, object[] args)
部署时采用 参数 0710，返回值 05 的配置

## 顶级域名合约的接口
顶级域名的接口分为三部分

一种为 通用接口，不需要权限验证，所有人都可以调用

一种为 所有者接口，仅接受所有者签名，或者由所有者脚本调用有效

一种为 注册器接口，仅接受由注册器脚本调用有效

## 通用接口
通用接口，不需要权限验证。
代码如下

    if (method == "rootName")
        return rootName();
    if (method == "rootNameHash")
        return rootNameHash();
    if (method == "getInfo")
        return getInfo((byte[])args[0]);
    if (method == "nameHash")
        return nameHash((string)args[0]);
    if (method == "nameHashSub")
        return nameHashSub((byte[])args[0], (string)args[1]);
    if (method == "nameHashArray")
        return nameHashArray((string[])args[0]);
    if (method == "resolve")
        return resolve((string)args[0], (byte[])args[1], (string)args[2]);
    if (method == "resolveFull")
        return resolveFull((string)args[0], (string[])args[1]);

#### rootName
返回当前顶级域名合约对应的根域名
返回值为string

#### rootNameHash
返回当前顶级域名合约对应的NameHash
返回值为byte[]

#### getInfo(byte[] namehash)
返回一个域名的信息
返回值为一个如下数组

    [
        byte[] owner//所有者
        byte[] register//注册器
        byte[] resolver//解析器
        BigInteger ttl//到期时间
    ]

#### nameHash(string domain)
将域名的一节转换为NameHash
  比如 

    nameHash("test") 
    nameHash("abc")

返回值为byte[]

#### nameHashSub(byte[] domainhash,string subdomain)
计算子域名的NameHash,
 比如

    var hash = nameHash("test");
    var hashsub = nameHashSub(hash,"abc")//计算abc.test 的namehash

返回值为byte[]

#### nameHashArray(string[] nameArray)
计算NameArray的NameHash，aa.bb.cc.test 对应的nameArray是["test","cc","bb","aa"]

    var hash = nameHashArray(["test","cc","bb","aa"]);

#### resolve(string protocol,byte[] hash,string or int(0) subdomain)
解析域名
    
    第一个参数为协议
        比如http是将域名映射为一个网络地址
        比如addr是将域名映射为一个NEO地址（这可能是最常用的映射）
    第二个参数为要解析的域名Hash
    第三个参数为要解析的子域名Name

应用代码如下
    
    var hash = nameHashArray(["test","cc","bb","aa"]);//客户端计算好
    resolve("http",hash,0)//合约解析 http://aa.bb.cc.test
    
    or
    
    var hash = nameHashArray(["test","cc","bb");//客户端计算好
    resolve("http",hash,“aa")//合约解析 http://aa.bb.cc.test

返回类型为byte[]，具体byte[]如何解读，由不同的协议定义，addr协议，byte[]就存的字符串。
协议约定另外撰文。

除了二级域名的解析，必须使用resolve("http",hash,0)的方式，其余的解析建议都使用resolve("http",hash,“aa")的方式。
#### resolveFull(string protocol,string[] nameArray)
解析域名，完整模式
    
    第一个参数为协议
    第二个参数为NameArray

这种解析方式唯一的不同就是会逐级验证一下所有权是否和登记的一致，一般用resolve即可

返回类型同resolve

## 所有者接口
所有者接口全部为 owner_SetXXX(byte[] srcowner,byte[] nnshash,byte[] xxx)的形式。
xxx 均是scripthash。

返回值均为 一个byte array [0] 表示失败 [1] 表示成功

所有者接口均接受账户地址直接签名调用和智能合约所有者调用。

如果所有者是智能合约，那么所有者应该自己判断权限，不满足条件，不要发起对顶级域名合约的appcall

#### owner_SetOwner(byte[] srcowner,byte[] nnshash,byte[] newowner)

转让域名所有权，域名的所有者可以是一个账户地址，也可以是一个智能合约。

srcowner 仅在 所有者是账户地址时用来验证签名，他是地址的scripthash

nnshash 是要操作的域名namehash

newowner 是新的所有者的地址的scripthash


#### owner_SetRegister(byte[] srcowner,byte[] nnshash,byte[] newregister)

设置域名注册器合约（域名注册器为一个智能合约）

域名注册器参数形式必须也是0710，返回05

必须实现如下接口

        public static object Main(string method, object[] args)
        {
            if (method == "getSubOwner")
                return getSubOwner((byte[])args[0], (string)args[1]);
            ...

        getSubOwner(byte[] nnshash,string subdomain)

任何人可调用注册器的接口检查子域的所有者

对于域名注册器的其他接口形式不做规范，官方提供的注册器会另外撰文说明。

用户自己实现的域名注册器，仅需实现getSubOwner接口

#### owner_SetResolve(byte[] srcowner,byte[] nnshash,byte[] newresolver)

设置域名解析器合约（域名解析器为一个智能合约）

域名解析器参数形式必须也是0710，返回05

必须实现如下接口

        public static byte[] Main(string method, object[] args)
        {
            if (method == "resolve")
                return resolve((string)args[0], (byte[])args[1]);
            ...
        
        resolve(string protocol,byte[] nnshash)

任何人可调用解析器接口进行解析

对于域名解析器的其它接口形式不做规范，官方提供的解析器会另外撰文说明。

用户自己实现的域名解析器，仅需实现resolve 接口

## 注册器接口 

注册器接口由注册器智能合约进行调用，只有一个

### register_SetSubdomainOwner(byte[] nnshash,string subdomain,byte[] newowner,BigInteger ttl)

注册一个子域名

nnshash 是要操作的域名namehash

subdomain 是要操作的子域名

newowner 是新的所有者的地址的scripthash

ttl 是域名过期时间（区块高度）

成功返回 [1] ,失败返回 [0]