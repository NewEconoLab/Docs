# ע������Լ

## ע������Լ������ʽ

ע���Լ����Appcall����ʽ���ö���������Լ��register_SetSubdomainOwner�ӿ�

        [Appcall("dffbdd534a41dd4c56ba5ccba9dfaaf4f84e1362")]
        static extern object rootCall(string method, object[] arr);

����������Լ�������ջ��ȡ���������ĺ�Լ�Ͷ���������Լ�����ע�������бȶԡ�

����ֻ��ָ����ע������Լ����ʵ�ֹ���

## ע�����ӿ�

ע����������ʽ����Ҳ��0710������05

        public static object Main(string method, object[] args)
        {
            if (method == "getSubOwner")
                return getSubOwner((byte[])args[0], (string)args[1]);
            if (method == "requestSubDomain")
                return requestSubDomain((byte[])args[0], (byte[])args[1], (string)args[2]);
            ...

### getSubOwner(byte[] nnshash,string subdomain)

�˽ӿ�Ϊע�����淶Ҫ�󣬱���ʵ�֣�������������ʱ����ô˽ӿ���֤Ȩ��

nnshash Ϊ����hash

subdomain Ϊ������

���� byte[] �����ߵ�ַ�����߿�
 
### requestSubDomain(byte[] who,byte[] nnshash,string subdomain)

�˽ӿ�Ϊ��ʾ���ȵ��ȵ�ע����ʹ�ã��û�����ע����������ӿ���������

who˭������

nnshash �����ĸ�����

subdomain �����������  