# ��������Լ

## ��������Լ������ʽ

2.�������Լ����������Ϣ

2.����������Լ����nep4�ķ�ʽ���ý������Ľ����ӿڻ�ȡ������Ϣ��

3.���������ý�������ʱ����Appcall����ʽ���ö���������Լ��getInfo�ӿ�����֤��������Ȩ

        [Appcall("dffbdd534a41dd4c56ba5ccba9dfaaf4f84e1362")]
        static extern object rootCall(string method, object[] arr);

�κκ�Լ������ͨ��  appcall ����������ԼgetInfo�ӿڵķ�ʽ����֤��������Ȩ

## �������ӿ�

ע����������ʽ����Ҳ��0710������05

        public static byte[] Main(string method, object[] args)
        {
            if (method == "resolve")
                return resolve((string)args[0], (byte[])args[1]);
            if (method == "setResolveData")
                return setResolveData((byte[])args[0], (byte[])args[1], (string)args[2], (string)args[3], (byte[])args[4]);
            ...

### resolve(string protocol,byte[] nnshash)

�˽ӿ�Ϊ�������淶Ҫ�󣬱���ʵ�֣�������������ʱ����ô˽ӿ����ս���

protocolΪЭ���ַ���

nnshashΪ����������

����byte[] ��������
 
### setResolveData(byte[] owner,byte[] nnshash,string or int[0] subdomain,string protocol,byte[] data)

�˽ӿ�Ϊ��ʾ�ı�׼���������У������ߣ�Ŀǰ��ֻ֧���˻���ַ�����ߣ����Ե��ô˽ӿ����ý�������

owner ������

nnshash �����ĸ�����

subdomain ���õ������������Դ�0��������õľ�����������������������0��  

protocol Э���ַ���

data ��������

����[1]��ʾ�ɹ� ����[0]��ʾʧ��