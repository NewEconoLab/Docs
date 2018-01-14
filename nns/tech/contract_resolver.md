# 解析器合约

## 解析器合约工作方式

2.解析器自己保存解析信息

2.顶级域名合约会以nep4的方式调用解析器的解析接口获取解析信息。

3.解析器设置解析数据时采用Appcall的形式调用顶级域名合约的getInfo接口来验证域名所有权

        [Appcall("dffbdd534a41dd4c56ba5ccba9dfaaf4f84e1362")]
        static extern object rootCall(string method, object[] arr);

任何合约都可以通过  appcall 顶级域名合约getInfo接口的方式来验证域名所有权

## 解析器接口

注册器参数形式必须也是0710，返回05

        public static byte[] Main(string method, object[] args)
        {
            if (method == "resolve")
                return resolve((string)args[0], (byte[])args[1]);
            if (method == "setResolveData")
                return setResolveData((byte[])args[0], (byte[])args[1], (string)args[2], (string)args[3], (byte[])args[4]);
            ...

### resolve(string protocol,byte[] nnshash)

此接口为解析器规范要求，必须实现，完整解析域名时会调用此接口最终解析

protocol为协议字符串

nnshash为解析的域名

返回byte[] 解析数据
 
### setResolveData(byte[] owner,byte[] nnshash,string or int[0] subdomain,string protocol,byte[] data)

此接口为演示的标准解析器所有，所有者（目前还只支持账户地址所有者）可以调用此接口配置解析数据

owner 所有者

nnshash 设置哪个域名

subdomain 设置的子域名（可以传0，如果设置的就是域名解析，非子域名传0）  

protocol 协议字符串

data 解析数据

返回[1]表示成功 或者[0]表示失败