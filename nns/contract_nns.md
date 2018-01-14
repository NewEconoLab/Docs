# ����������Լ

## ����������Լ�ĺ���ǩ��

����ǩ������

    public static object Main(string method, object[] args)
����ʱ���� ���� 0710������ֵ 05 ������

## ����������Լ�Ľӿ�
���������Ľӿڷ�Ϊ������

һ��Ϊ ͨ�ýӿڣ�����ҪȨ����֤�������˶����Ե���

һ��Ϊ �����߽ӿڣ�������������ǩ���������������߽ű�������Ч

һ��Ϊ ע�����ӿڣ���������ע�����ű�������Ч

## ͨ�ýӿ�
ͨ�ýӿڣ�����ҪȨ����֤��
��������

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
���ص�ǰ����������Լ��Ӧ�ĸ�����
����ֵΪstring

#### rootNameHash
���ص�ǰ����������Լ��Ӧ��NameHash
����ֵΪbyte[]

#### getInfo(byte[] namehash)
����һ����������Ϣ
����ֵΪһ����������

    [
        byte[] owner//������
        byte[] register//ע����
        byte[] resolver//������
        BigInteger ttl//����ʱ��
    ]

#### nameHash(string domain)
��������һ��ת��ΪNameHash
  ���� 

    nameHash("test") 
    nameHash("abc")

����ֵΪbyte[]

#### nameHashSub(byte[] domainhash,string subdomain)
������������NameHash,
 ����

    var hash = nameHash("test");
    var hashsub = nameHashSub(hash,"abc")//����abc.test ��namehash

����ֵΪbyte[]

#### nameHashArray(string[] nameArray)
����NameArray��NameHash��aa.bb.cc.test ��Ӧ��nameArray��["test","cc","bb","aa"]

    var hash = nameHashArray(["test","cc","bb","aa"]);

#### resolve(string protocol,byte[] hash,string or int(0) subdomain)
��������
    
    ��һ������ΪЭ��
        ����http�ǽ�����ӳ��Ϊһ�������ַ
        ����addr�ǽ�����ӳ��Ϊһ��NEO��ַ�����������õ�ӳ�䣩
    �ڶ�������ΪҪ����������Hash
    ����������ΪҪ������������Name

Ӧ�ô�������
    
    var hash = nameHashArray(["test","cc","bb","aa"]);//�ͻ��˼����
    resolve("http",hash,0)//��Լ���� http://aa.bb.cc.test
    
    or
    
    var hash = nameHashArray(["test","cc","bb");//�ͻ��˼����
    resolve("http",hash,��aa")//��Լ���� http://aa.bb.cc.test

��������Ϊbyte[]������byte[]��ν�����ɲ�ͬ��Э�鶨�壬addrЭ�飬byte[]�ʹ���ַ�����
Э��Լ������׫�ġ�

���˶��������Ľ���������ʹ��resolve("http",hash,0)�ķ�ʽ������Ľ������鶼ʹ��resolve("http",hash,��aa")�ķ�ʽ��
#### resolveFull(string protocol,string[] nameArray)
��������������ģʽ
    
    ��һ������ΪЭ��
    �ڶ�������ΪNameArray

���ֽ�����ʽΨһ�Ĳ�ͬ���ǻ�����֤һ������Ȩ�Ƿ�͵Ǽǵ�һ�£�һ����resolve����

��������ͬresolve

## �����߽ӿ�
�����߽ӿ�ȫ��Ϊ owner_SetXXX(byte[] srcowner,byte[] nnshash,byte[] xxx)����ʽ��
xxx ����scripthash��

����ֵ��Ϊ һ��byte array [0] ��ʾʧ�� [1] ��ʾ�ɹ�

�����߽ӿھ������˻���ֱַ��ǩ�����ú����ܺ�Լ�����ߵ��á�

��������������ܺ�Լ����ô������Ӧ���Լ��ж�Ȩ�ޣ���������������Ҫ����Զ���������Լ��appcall

#### owner_SetOwner(byte[] srcowner,byte[] nnshash,byte[] newowner)

ת����������Ȩ�������������߿�����һ���˻���ַ��Ҳ������һ�����ܺ�Լ��

srcowner ���� ���������˻���ַʱ������֤ǩ�������ǵ�ַ��scripthash

nnshash ��Ҫ����������namehash

newowner ���µ������ߵĵ�ַ��scripthash


#### owner_SetRegister(byte[] srcowner,byte[] nnshash,byte[] newregister)

��������ע������Լ������ע����Ϊһ�����ܺ�Լ��

����ע����������ʽ����Ҳ��0710������05

����ʵ�����½ӿ�

        public static object Main(string method, object[] args)
        {
            if (method == "getSubOwner")
                return getSubOwner((byte[])args[0], (string)args[1]);
            ...

        getSubOwner(byte[] nnshash,string subdomain)

�κ��˿ɵ���ע�����Ľӿڼ�������������

��������ע�����������ӿ���ʽ�����淶���ٷ��ṩ��ע����������׫��˵����

�û��Լ�ʵ�ֵ�����ע����������ʵ��getSubOwner�ӿ�

#### owner_SetResolve(byte[] srcowner,byte[] nnshash,byte[] newresolver)

����������������Լ������������Ϊһ�����ܺ�Լ��

����������������ʽ����Ҳ��0710������05

����ʵ�����½ӿ�

        public static byte[] Main(string method, object[] args)
        {
            if (method == "resolve")
                return resolve((string)args[0], (byte[])args[1]);
            ...
        
        resolve(string protocol,byte[] nnshash)

�κ��˿ɵ��ý������ӿڽ��н���

���������������������ӿ���ʽ�����淶���ٷ��ṩ�Ľ�����������׫��˵����

�û��Լ�ʵ�ֵ�����������������ʵ��resolve �ӿ�

## ע�����ӿ� 

ע�����ӿ���ע�������ܺ�Լ���е��ã�ֻ��һ��

### register_SetSubdomainOwner(byte[] nnshash,string subdomain,byte[] newowner,BigInteger ttl)

ע��һ��������

nnshash ��Ҫ����������namehash

subdomain ��Ҫ������������

newowner ���µ������ߵĵ�ַ��scripthash

ttl ����������ʱ�䣨����߶ȣ�

�ɹ����� [1] ,ʧ�ܷ��� [0]