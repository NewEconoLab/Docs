# Namehash 算法

## Namehash
NNS中存储的域名为32字节散列值，而不是域名原文的文本。这有几个设计原因：
1.	处理过程统一，允许任意长度的域名。
2.	一定程度保留了域名的隐私
3.	将域名转换为散列的算法称为NameHash

## 域名domain与名字数组domainarray 与协议
通常我们在互联网上使用的url如下 

    http://aaa.bbb.test/xxx 
其中

1.  http是协议(protocol)，NNS服务请求时会把域名和协议分开传递.
2.  aaa.bbb.test是域名，NNS服务请求时使用域名的hash
3.  xxx是路径，路径不是在dns的层次处理，对于nns也一样，如果有路径，交由其他的方式处理

NNS服务使用的不是域名，而是域名的名字数组，这样处理起来更加直接

域名 aaa.bb.test 转成字节数组就是["test","bb","aa"]

你可以这样调用解析
>NNS.ResolveFull("http",["test","bb","aa"]);

交由合约去计算出namehash
## NameHash算法
NameHash算法是将域名转成DomainArray以后，逐级连接计算hash的方法，代码如下

        //域名转hash算法
        static byte[] nameHash(string domain)
        {
            return SmartContract.Sha256(domain.AsByteArray());
        }
        static byte[] nameHashSub(byte[] roothash, string subdomain)
        {
            var domain = SmartContract.Sha256(subdomain.AsByteArray()).Concat(roothash);
            return SmartContract.Sha256(domain);
        }
        static byte[] nameHashArray(string[] domainarray)
        {
            byte[] hash = nameHash(domainarray[0]);
            for (var i = 1; i < domainarray.Length; i++)
            {
                hash = nameHashSub(hash, domainarray[i]);
            }
            return hash;
        }

## 快速解析
完整的解析传入整个DomainArray,由智能合约去逐个检查一层层解析。
计算NameHash的过程也可以挪到客户端算好，再传入只能合约。
调用方式如下

    //查询 http://aaa.bbb.test
    var hash = nameHashArray(["test","bb"]);//可以客户端计算
    NNS.Resolve("http",hash,"aaa");//调用智能合约

或者

    //查询 http://bbb.test
    var hash = nameHashArray(["test","bb"]);//可以客户端计算
    NNS.Resolve("http",hash,null);//调用智能合约

你也许会考虑查询 aaa.bbb.test 的过程为什么不是这样

    //查询 http://aaa.bbb.test
    var hash = nameHashArray(["test","bb","aaa"]);//可以客户端计算
    NNS.Resolve("http",hash,null);//调用智能合约

我们要考虑aaa.bb.test 是否拥有一个独立的解析器，如果aaa.bb.test被卖给了别人，他指定了一个独立的解析器，这样是可以查询到的。
如果aaa.bb.test 并没有独立的解析器，他是有bb.test的解析器来解析。
那么这样就无法查询到