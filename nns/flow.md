# 流程
## 注册器接口
注册器合约简称nns

以下数据存在于智能合约存储区

node = namehash("xxx.xxx...")
namehash计算之后的域名称为node

    ```
    //域名->解析器映射数据
    //存在于Storage中
    //此为伪代码
    struct nodeinfo
    {
        address resolver;//解析器地址
        address owner;//拥有者地址
        int ttl;
    }
    dictionary<node,nodeinfo>
    ```
以下接口存在于智能合约

    ```
    //均为伪代码
    //查询类
    address getowner(byte[] node);
    address getresolver(byte[] node);
    uint64  getttl(bytes[] node);

    //设置类    
    void setOwner(bytes[] node, address owner);//设置域名拥有者
    //设置子域名拥有者
    void setSubnodeOwner(bytes[] node, string label, address owner){
        //检验操作者是否有设置Node的下级域名的权限
        //然后
        var subnode = hash256(node + label);
        setOwner(subnode,owner);
    }

    void setResolver(byte[] node, address resolver);

    void setTTL(bytes32 node, uint64 ttl) only_owner(node);
    ```
## 解析器接口
    ```
    //一个解析器拥有多种片段类型，类型定义其他文章描述
    bool supportElement(byte[] type);
    void SetValue(byte[] type,byte[] value);
    byte[] GetValue(byte[] type);
    ```     
## 登记员接口
    ```
    void register(string label,address owner);
    ```
## 注册域名流程
    
    1.nns管理员调用 nns.setOwner(namehash(".neo"),先到先得登记员);
        先到先得登记员拥有了所有的.neo域名
    2.用户A调用 先到先得登记员.register("hihi",userA);
        用户A得到了自己的域名 hihi.neo
    ** 先到先得登记员.register 会调用 nns.setSubnodeOwner(namehash(".neo"),"hihi",userA);
    2.用户A调用  nns.setSubnodeOwner(namehash("hihi.neo"),"b",userB);
        将自己的二级域名 b.hihi.neo 分配给用户B
    3.用户B调用  nns.setSubnodeOwner(namehash("b.hihi.neo"),"mrx",userX);
        将三级域名 mrx.b.hihi.neo 分配给用户X

## 设置解析流程
     1.用户X 调用 nns.setResolver(namehash("mrx.b.hihi.neo"),pubResolver);
            用户X设置了一个通用解析器
            该解析器，放进什么，就出什么。
    2.用户X 调用  nns.getresolver(namehash("mrx.b.hihi.neo")).SetValue(Value.Text,"ni hao ma.");
            将解析器的 “text" 字段写进了 ni hao ma.
    
## 使用解析流程
    
    1.用户X 调用  nns.getresolver(namehash("mrx.b.hihi.neo")).GetValue(Value.Text);
            得到解析数据
    