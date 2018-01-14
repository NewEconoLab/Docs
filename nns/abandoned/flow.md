# ����
## ע�����ӿ�
ע������Լ���nns

�������ݴ��������ܺ�Լ�洢��

node = namehash("xxx.xxx...")
namehash����֮���������Ϊnode

    ```
    //����->������ӳ������
    //������Storage��
    //��Ϊα����
    struct nodeinfo
    {
        address resolver;//��������ַ
        address owner;//ӵ���ߵ�ַ
        int ttl;
    }
    dictionary<node,nodeinfo>
    ```
���½ӿڴ��������ܺ�Լ

    ```
    //��Ϊα����
    //��ѯ��
    address getowner(byte[] node);
    address getresolver(byte[] node);
    uint64  getttl(bytes[] node);

    //������    
    void setOwner(bytes[] node, address owner);//��������ӵ����
    //����������ӵ����
    void setSubnodeOwner(bytes[] node, string label, address owner){
        //����������Ƿ�������Node���¼�������Ȩ��
        //Ȼ��
        var subnode = hash256(node + label);
        setOwner(subnode,owner);
    }

    void setResolver(byte[] node, address resolver);

    void setTTL(bytes32 node, uint64 ttl) only_owner(node);
    ```
## �������ӿ�
    ```
    //һ��������ӵ�ж���Ƭ�����ͣ����Ͷ���������������
    bool supportElement(byte[] type);
    void SetValue(byte[] type,byte[] value);
    byte[] GetValue(byte[] type);
    ```     
## �Ǽ�Ա�ӿ�
    ```
    void register(string label,address owner);
    ```
## ע����������
    
    1.nns����Ա���� nns.setOwner(namehash(".neo"),�ȵ��ȵõǼ�Ա);
        �ȵ��ȵõǼ�Աӵ�������е�.neo����
    2.�û�A���� �ȵ��ȵõǼ�Ա.register("hihi",userA);
        �û�A�õ����Լ������� hihi.neo
    ** �ȵ��ȵõǼ�Ա.register ����� nns.setSubnodeOwner(namehash(".neo"),"hihi",userA);
    2.�û�A����  nns.setSubnodeOwner(namehash("hihi.neo"),"b",userB);
        ���Լ��Ķ������� b.hihi.neo ������û�B
    3.�û�B����  nns.setSubnodeOwner(namehash("b.hihi.neo"),"mrx",userX);
        ���������� mrx.b.hihi.neo ������û�X

## ���ý�������
     1.�û�X ���� nns.setResolver(namehash("mrx.b.hihi.neo"),pubResolver);
            �û�X������һ��ͨ�ý�����
            �ý��������Ž�ʲô���ͳ�ʲô��
    2.�û�X ����  nns.getresolver(namehash("mrx.b.hihi.neo")).SetValue(Value.Text,"ni hao ma.");
            ���������� ��text" �ֶ�д���� ni hao ma.
    
## ʹ�ý�������
    
    1.�û�X ����  nns.getresolver(namehash("mrx.b.hihi.neo")).GetValue(Value.Text);
            �õ���������
    