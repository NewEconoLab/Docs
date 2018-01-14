# 注册器合约

## 注册器合约工作方式

注册合约采用Appcall的形式调用顶级域名合约的register_SetSubdomainOwner接口

        [Appcall("dffbdd534a41dd4c56ba5ccba9dfaaf4f84e1362")]
        static extern object rootCall(string method, object[] arr);

顶级域名合约会检查调用栈，取出调用它的合约和顶级域名合约管理的注册器进行比对。

所以只有指定的注册器合约可以实现管理。

## 注册器接口

注册器参数形式必须也是0710，返回05

        public static object Main(string method, object[] args)
        {
            if (method == "getSubOwner")
                return getSubOwner((byte[])args[0], (string)args[1]);
            if (method == "requestSubDomain")
                return requestSubDomain((byte[])args[0], (byte[])args[1], (string)args[2]);
            ...

### getSubOwner(byte[] nnshash,string subdomain)

此接口为注册器规范要求，必须实现，完整解析域名时会调用此接口验证权利

nnshash 为域名hash

subdomain 为子域名

返回 byte[] 所有者地址，或者空
 
### requestSubDomain(byte[] who,byte[] nnshash,string subdomain)

此接口为演示的先到先得注册器使用，用户调用注册器的这个接口申请域名

who谁在申请

nnshash 申请哪个域名

subdomain 申请的子域名  