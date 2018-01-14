# NNS 协议

## 协议
通常我们在互联网上使用的url如下 

    http://aaa.bbb.test/xxx 
其中

1.  http是协议(protocol)，NNS服务请求时会把域名和协议分开传递.
2.  aaa.bbb.test是域名，NNS服务请求时使用域名的hash
3.  xxx是路径，路径不是在dns的层次处理，对于nns也一样，如果有路径，交由其他的方式处理

## NNS的协议使用字符串定义
以下均为暂定

### http协议
    http协议指向一个string,表示一个互联网地址

### addr协议
    addr协议指向一个string，表示一个NEO address，形如 AdzQq1DmnHq86yyDUkU3jKdHwLUe2MLAVv

### script协议
    script协议指向一个byte[],表示一个NEO ScriptHash，形如0xf3b1c99910babe5c23d0b4fd0104ee84ffeec2a5

同一域名的不同协议做不同处理

    http://abc.test 可以指向 http://www.163.com
    addr://abc.test 可以指向 AdzQq1DmnHq86yyDUkU3jKdHwLUe2MLAVv
    script://abc.test 可以指向 0xf3b1c99910babe5c23d0b4fd0104ee84ffeec2a5
