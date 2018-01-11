# Namehash �㷨

## Namehash
NNS�д洢������Ϊ32�ֽ�ɢ��ֵ������������ԭ�ĵ��ı������м������ԭ��
1.	�������ͳһ���������ⳤ�ȵ�������
2.	һ���̶ȱ�������������˽
3.	������ת��Ϊɢ�е��㷨��ΪNameHash

## ����domain����������domainarray ��Э��
ͨ�������ڻ�������ʹ�õ�url���� 

    http://aaa.bbb.test/xxx 
����

1.  http��Э��(protocol)��NNS��������ʱ���������Э��ֿ�����.
2.  aaa.bbb.test��������NNS��������ʱʹ��������hash
3.  xxx��·����·��������dns�Ĳ�δ�������nnsҲһ���������·�������������ķ�ʽ����

NNS����ʹ�õĲ��������������������������飬����������������ֱ��

���� aaa.bb.test ת���ֽ��������["test","bb","aa"]

������������ý���
>NNS.ResolveFull("http",["test","bb","aa"]);

���ɺ�Լȥ�����namehash
## NameHash�㷨
NameHash�㷨�ǽ�����ת��DomainArray�Ժ������Ӽ���hash�ķ�������������

        //����תhash�㷨
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

## ���ٽ���
�����Ľ�����������DomainArray,�����ܺ�Լȥ������һ��������
����NameHash�Ĺ���Ҳ����Ų���ͻ�����ã��ٴ���ֻ�ܺ�Լ��
���÷�ʽ����

    //��ѯ http://aaa.bbb.test
    var hash = nameHashArray(["test","bb"]);//���Կͻ��˼���
    NNS.Resolve("http",hash,"aaa");//�������ܺ�Լ

����

    //��ѯ http://bbb.test
    var hash = nameHashArray(["test","bb"]);//���Կͻ��˼���
    NNS.Resolve("http",hash,null);//�������ܺ�Լ

��Ҳ��ῼ�ǲ�ѯ aaa.bbb.test �Ĺ���Ϊʲô��������

    //��ѯ http://aaa.bbb.test
    var hash = nameHashArray(["test","bb","aaa"]);//���Կͻ��˼���
    NNS.Resolve("http",hash,null);//�������ܺ�Լ

����Ҫ����aaa.bb.test �Ƿ�ӵ��һ�������Ľ����������aaa.bb.test�������˱��ˣ���ָ����һ�������Ľ������������ǿ��Բ�ѯ���ġ�
���aaa.bb.test ��û�ж����Ľ�������������bb.test�Ľ�������������
��ô�������޷���ѯ��