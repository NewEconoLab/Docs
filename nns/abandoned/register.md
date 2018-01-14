# 域名结构

> 根域名
> > 顶级域名
> > > 主域名
> > > > 子域名

> .
> > neo
> > > qingmingzi
> > > > addr

nns:addr.qingmingzi.neo

# 顶级域名注册器
&emsp;&emsp;一个智能合约，可以管理顶级域名和顶级域名注册器存储合约、规则合约的对应关系
## 管理顶级域名的实现
&emsp;&emsp;使用合约私有存储区保存顶级域名namehash和顶级域名注册器的scripthash的对应关系，namehash为key，scripthash为value 

&emsp;&emsp;顶级域名注册器私有化存储区数据结构

key | value
---|---
namehash+0000 | 存储合约scripthash
namehash+0001 | 规则合约scripthash

&emsp;&emsp;namehash附加0001（2位16进制数）代表存储合约，附加0002（2位16进制数）代表规则合约，value存储合约的scripthash。

## 方法定义

方法定义 | 作用
---|---
byte[] Query(byte[] namehashplus) | 输入顶级域名namehash+类型定义,返回主域名注册器合约scripthash
bool SetRegister(string label,byte[] type,byte[] scripthash) | 输入顶级域名、合约类型、注册器合约hash，执行存储区写入
bool IsChecked(byte[][] scripthash) | 输入根登记员地址scripthash数组，验证当前操作是否由2/3以上签名。任何修改操作都在此函数返回为真后继续



## 顶级域名注册器的治理
&emsp;&emsp;根注册器的修改操作需要若干奇数根登记员（地址或合约）进行签名，签名超过三分之二方可执行。根注册器的操作应该是非常谨慎的，仅为按需增加新顶级域名与修复已有顶级域名注册器重大bug而操作。

# 主域名注册器
&emsp;&emsp;定义了一个顶级域名的注册规范，不同顶级域名可能有不同规范，目前我们已经设定如下顶级域名与规则：
## .test
   - 该顶级域名及其下域名树仅为测试目的设立
   - 域名树的根为.test
   - 任何人可以注册为未被注册或已过期的以.test结尾的主域名
   - 每个被注册的域名有效期为30天，有效期到期时原登记员不具有优先续期权
   - 主域名长度最小不得小于1个字符。为兼容传统DNS，最大长度不能超过63个字符
   - 主域名登记员拥有分配、管理子域名、移交子域名管理权的权限，子域名应该最多含有两级（以.分割），总长度不能超过253个字符
   - 主域名登记员可以主动移交所有权，但是有效期不重新计算
   - 主域名登记员可以主动撤销一个其所有主域名的所有权
## .neo
   - 该顶级域名及其下域名树为正式使用目的设立，可以用于生产
   - 域名树的根为.neo
   - 主域名的所有权通过竞标获得，任何人可以为未被注册或已过期的以.test结尾的主域名开标。
   - 每个被注册的域名有效期为365天，有效期到期前30天内原登记员可以优先获得所有权续期权，续期将需要支付租金（租金确定模型将稍后公布）
   - 主域名的长度在不同阶段设置有不同限制
     1. 第一阶段，.neo发布后，主域名长度最小不得小于7个字符，最大不得大于63个字符
     2. 第二阶段，上一阶段后180天，主域名长度最小不得小于3个字符，最大不得大于63个字符
     3. 第三阶段，上一阶段后180天，主域名长度最小不得小于2个字符，最大不得大于63个字符
     4. 第四阶段，上一阶段后180天，主域名长度最小不得小于1个字符，最大不得大于63个字符
   - 主域名登记员拥有分配、管理子域名、移交子域名管理权的权限，子域名应该最多含有两级（以.分割），总长度不能超过253个字符
   - 主域名登记员可以主动移交所有权，但是有效期不重新计算。登记员可以使用交易服务发送域名出售邀约。
   - 主域名登记员可以主动撤销一个其所有主域名的所有权

## 主域名的注册实现
&emsp;&emsp;注册器由2个互相协作的合约组成，分别为存储合约与规则合约，每个顶级域名的注册器的2个合约hash由根域名登记员管理；
### 主域名注册器私有化存储区数据结构

key | value
---|---
namehash+0000 | 登记员地址scripthash
namehash+0001 | 注册时间unix时间戳
namehash+0002 | 解析器合约scripthash


&emsp;&emsp;namehash附加0001（2位16进制数）代表登记员地址（钱包地址的等价物），附加0002（2位16进制数）代表解析器合约hash，附加0003（2位16进制数）代表注册时间unix时间戳（以此计算到期）。

### 存储合约（主合约）
  - 注册器的入口
  - 实现查询域名所有权方法，输入namehash，在私有化存储区查询，输出所有者scripthash（address的一种等价物）。如果发现主域名已过期，则强制执行注销并返回0地址。
  - 实现允许规则合约允许的注册行为，输入nanmehash、所有者scripthash，执行存储区写入，以当前时间转换为unix时间戳存入存储区作为注册时间
  - 实现允许规则合约允许的注册变更行为，输入nanmehash、所有者scripthash，执行存储区擦除后写入
  - 实现允许规则合约允许的注销行为，输入namehash，执行擦除指定key的value
  - 实现允许规则合约允许的子域名注册（管理）行为，输入子域名namehash、子域名管理者scripthash，执行存储区写入（或擦除后写入）
  - 实现直接返回解析结果方法，输入namehash，从存储区取出对应解析器合约hash，动态调用（NEP4）指定解析器合约，返回对应解析值。如果发现主域名已过期，则强制注销并返回零地址。
  - 实现允许规则合约允许的解析器器设置行为，输入namehash、解析器hash，执行存储区写入
  - 实现当规则合约需强制注销方法返回为真时，强制注销主域名。
  - 存储合约首先从根域名注册器私有化存储区去除当前合法的规则合约hash，并以此作为参数调用规则合约。（NEP4动态合约调用）
### 规则合约
  - 实现域名形式检查方法，返回布尔型
  - 实现域名注册权验证方法，返回布尔型
  - 实现域名变更权验证方法，返回布尔型
  - 实现域名注销权验证方法，返回布尔型
  - 实现子域名注册（管理）权的验证方法，返回布尔型
  - 实现解析器设置权的验证方法，返回布尔型
  - 实现需强制注销判断方法，返回布尔型
  - 根域名注册器中存储的规则合约hash地址决定注册器使用何种规则，规则应该在重大bug前提下被谨慎变更
 
## 方法定义
### 存储合约（主合约）

方法定义 | 作用
---|---
byte[] Query(string domain, string name, string subname) | 将输入参数导入规则合约进行验证,验证有效性、是否过期等,验证通过返回域名登记员地址scripthash
string QueryResolver(string domain, string name, string subname) | 将输入参数导入规则合约进行验证,验证有效性、是否过期等,验证通过动态调用对应解析器合约,返回域名解析值（每个解析器实例都应实现此接口）
bool SetOwner(string domain, string name, string subname,byte[] owner) | 将输入参数导入规则合约进行验证，验证注册权后允许将主域名所有权写入存储区,以执行时间为注册时间戳
bool SetSubname（string domain, string name, string subname,byte[] subnameManger） | 将参数导入规则合约验证，验证操作者对主域名的所有权后，允许设置子域名管理者，以主域名注册时间戳为子域名注册时间戳
bool SetResolver(string domain, string name, string subname，byte[] scripthash) | 将参数导入规则合约验证，验证操作者是主域名所有者或子域名管理者，允许设置域名解析器合约
bool TransferOwner（string domain, string name, string subname，byte[] newowner） | 将参数导入规则合约验证，验证操作者对主域名所有权后，允许更换登记员，更换后主域名注册时间戳不变。==【变更子域名管理者不应使用此方法而应使用setSubname()】==
bool CancelOwner(string domain, string name, string subname) | 注销一个登记员对一个namehash的所有权，namehash导入规则合约验证，验证为主域名所有者允许注销域名。当域名到期时将被强制注销。主域名注销后，子域名、解析器合约地址在被使用时会被强制注销，



### 规则合约

方法定义 | 作用
---|---
bool IsOwner（byte[] namehash） | 输入namehash，判断当前调用合约的交易是否是被namehash所有者签名的
bool IsExpire（byte[] namehash）| 输入namehash，判断域名是否已过期
bool IsValid（string domain, string name, string subname）| 输入域名，判断域名是否有效
bool IsLegal（string domain, string name, string subname）| 输入域名，判断顶级域名是否合法、域名长度是否合法等
bool AllowRegiste(string domain, string name, string subname,byte[] scripthash) | 输入域名,判断请求人是否有权注册主域名
bool AllowManage(string domain, string name, string subname,byte[] scripthash) | 输入域名,判断请求人是否有权管理子域名、解析器地址
bool TryCancelOwner（string domain, string name, string subname）| 判断一个域名是否需要被强制注销，如果需要则调用主合约 CancelOwner(）注销域名
bool TryCancelResolver（string domain, string name, string subname）| 判断一个域名的解析设置是否需要被强制注销，如果需要则调用主合约SetResolver()将解析合约地址设置为空。



## 域名的规范化

### Namehash算法
&emsp;&emsp;采用Namehash算法将所有域名处理成统一范式，方便处理同时具有一定安全和保密性
  - Namehash算法实现
    1. 从域名中按英文句号（.）为分隔符拆分出，顶级域名、主域名（二级域名）、子域名多个部分
    2. 将多个部分全部按照UTF8字符集转换为byte[]字节数组
    3. 将每部分字节数组按照统一的Namehash函数递归处理
    <pre>
	///参数namehash为上级域名的Namehash()结果
	///参数label为本级域名的字符串
	///返回值为32位byte数组

	byte[32] Namehash(byte[32] namehash,string label)
	</pre>
    4. Namehash()使用[Neo hash256算法](https://github.com/neo-project/neo/blob/master/neo/Cryptography/Crypto.cs)，对字节数组进行散列运算，此32位长度字节数组的运算结果即为namehash结果值
	<pre>
	return hash256(namehash + label)
	</pre>
  - Namehash应用场景
    - 合约中使用（如规则合约）
    - 客户端使用（如查询UI）
  - Namehash算法返回应符合以下预期
    - 输入域名：qingmingzi.neo
    - 返回namehash:9b87a694f0a282b2b5979e4138944b6805350c6fa3380132b21a2f12f9c2f4b6

### namecheck服务（API）
&emsp;&emsp;namecheck为NEL向公众提供一个API服务，其功能旨在为NNS用户提供统一的域名规范化判别标准。由于智能合约不适宜执行规范化验证这类高消耗计算，故而采用链外接口提供。namecheck实质上并非强制性规范，只是推荐性的，但是仍希望所有nns参与者遵守。
  - namecheck规范要点
    - 检查域名是否包含一般不应出现在域名中的特殊字符
    - 检查域名是否包含一般容易引起歧义的字符（比如0和O）
    - 检查每一级域名是否不超过63个字符
    - 检查域名全长是否不超过253个字符
    - 检查子域名是否最多只包含一个分割点（英文句号.）
  - namecheck的应用场景
    - 在任何域名注册前
    - 在任何域名解析前
  - namecheck返回值的处置策略
    - 当返回假（false），拒绝注册或解析nns
    - 当返回假（flase），提示用户域名不符合规范（不推荐）