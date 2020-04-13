# ssh

TODO��sshЭ��ԭ�����������ӹ���

## ssh����

ssh��һ������Э�飬���ڼ����֮��ļ��ܵ�¼��ssh���ù�Կ���ܣ����Ա�֤�����������֮�佻������Ϣ��ȫ

sshֻ��һ��Э�飬���ڶ���ʵ�֣�������Ե�ʵ����openssh����������������Ӧ�÷ǳ��㷺

### ssh����--�м��˹���

ssh���ӵ����������������ģ�

- 1.Զ�������յ��û��ĵ�¼���󣬰��Լ��Ĺ�Կ���͸��û�
- 2.�û�ʹ�������Կ������¼������ܺ󣬷��͵�Զ������
- 3.Զ���������Լ���˽Կ�����ܵ�¼���룬���������ȷ����ͬ���û���¼

������̱����ǰ�ȫ�ģ�����ʵʩ�Ĺ����д���һ�����գ�������˽ػ��˵�¼����Ȼ��ð��Զ����������α��Ĺ�Կ���͸��û�����ô�û����ѱ����α��

���ԣ���������߲����û���Զ������֮��(���繫����wifi����)����α��Ĺ�Կ����ȡ�û��ĵ�¼���롣������������¼Զ����������ôssh�İ�ȫ���ƾ͵�Ȼ�޴��ˣ����ַ��վ���������`�м��˹���(Man-in-the-middle-attack)`

## ssh��װ������

ssh�ֿͻ���openssh-client�ͷ����openssh-server

�����ֻ�����½��Ļ�����ssh��ֻ��Ҫ��װopenssh-client�����Ҫʹ��������ssh������Ҫ��װopenssh-server

### 1. �鿴�������Ƿ��Ѱ�װ�˿ͻ��˺ͷ����
```
dpkg -l | grep ssh
```

### 2. ��װ
```
apt install openssh-client
apt install openssh-server
```

### 3. ȷ��ssh-server�Ƿ��Ѿ�����
```
ps -e | grep ssh
```
����`ssh-agent`��ʾssh-client������`sshd`��ʾssh-server����

ssh����������ֹͣ��������
```
sudo /etc/init.d/ssh start
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh restart
```

## ssh�����ֵ�¼��ʽ

ssh�ṩ���ַ�ʽ����֤��ʽ��

- ���ڿ���İ�ȫ��֤��ֻҪ��֪���˺źͿ���Ϳ��Ե�¼��Զ�����������д�������ݶ��ᱻ���ܣ����ǲ��ܱ�֤���������ӵķ����������������ӵķ����������ܻ��б�ķ���������ð�������ķ�������Ҳ���ǻ��ܵ�"�м��˹���"
- ������Կ�İ�ȫ��֤������ҪΪ�Լ�����һ����Կ�����ѹ�Կ�ŵ���Ҫ���ʵķ������������Ҫ���ӵ�ssh��������ssh�ͻ��˾ͻ���������������������������Կ���а�ȫ��֤���������յ�����֮�����ڸ÷������������Ŀ¼��Ѱ����Ĺ�Կ��Ȼ��������㷢�͹����Ĺ�Կ���бȽϣ����������Կһ�£����������ù�Կ����"��ѯ"���������͸��ͻ����������ͻ��������յ�"��ѯ"֮��Ϳ��������˽Կ�ڱ��ؽ����ٰ������͸���������ɵ�¼

���һ�ּ�����ȣ���Կ��¼�����������д�������ݣ�Ҳ����Ҫ�������ϴ��Ϳ����˰�ȫ�Ը��ߣ�������Ч��ֹ"�м��˹���"

### 1. �����¼

ssh �û���@������ip��ַ��
```
ssh user@192.168.0.1
```

�����Ҫ����ͼ�ν������ʹ��`-X`ѡ��
```
ssh -X user@192.168.0.1
```

����ͻ����ͷ������û�����ͬ����¼ʱ����ʡ���û���

ssh�����Ĭ�϶˿���22����������������������Ķ˿ڣ�ͨ��`-p`ѡ���޸ĵ�¼�˿ڣ�
```
ssh -p 1234 192.168.0.1
```

### 2. ��Կ��¼

- 1.�ڱ���������Կ��

  ��Կ��¼֮ǰ��Ҫ��ʹ��`ssh-keygen`�ڱ���������Կ�ԣ�
  ```
  ssh-keygen -t rsa   # -t��ʾ�������ͣ�����ʹ��rsa�����㷨
  ```

  Ȼ�������ʾһ������enter���ɣ�ִ�н���֮����� **��ǰ�û���Ŀ¼** ������һ�� **.ssh�ļ���** �����а��� **˽Կid_rsa** �� **��Կid_rsa.pub** 

- 2.����Կ���Ƶ�Զ������

  ʹ��`ssh-copy-id`�����Կ���Ƶ�Զ����������Կ�ᱻд��Զ��������`~/.ssh/authorized_keys`�ļ���
  ```
  ssh-copy-id user@192.168.0.1
  ```

���������������裬�Ժ��¼���Զ�������Ͳ�����Ҫ�������룬Ҳ���Ӱ�ȫ��

## ��������

### 1. ssh��¼��ʹ���������ն�����

�����ssh��¼��ֱ�����ն���һЩ���򣬹رձ����ն˴��ں󣬲��ܺ�̨������ǰ̨���򶼻����ն˹رն�����

����ʹ��`nohup`�����ó��������նˣ����ն˹ر�ʱ���ܼ�������

```
nohup python3 a.py &
```

### 2. ����sshһֱ����

���ʹ��iTerm2��Ҫ��ssh�����ߣ�preferences -> profiles -> sessions -> when idel, send ASCII code

���������пͻ��ˣ�����ͨ������`ServerAliveInterval`��ʵ�֣���`~/.ssh/config`��д�룺
```
Host *
  ServerAliveInterval 60
```
��ʾssh�ͻ���ÿ��60���Զ����������һ��no-op����no-op�����κβ�������˼������Զ�������Ͳ���ر����ssh�Ự

`Host *`��ʾ���ӵ����е�Զ������ʱ������һֱ���ӣ�Ҳ�������ĳ����������Ҫ����Ϊ�û�����hostname

### 3. sshԶ�̲���

ssh������������Զ��������¼��������ֱ����Զ��������ִ�в���������
```
ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```
�������еĲ��֣���ʾ��Զ��������ִ�еĲ���������������ض��򣬱�ʾ����ͨ��ssh����Զ������

�����˵��ssh�������û���Զ������֮�䣬������������ݵĴ���ͨ������˺ܶ����鶼����ͨ��ssh�����

���濴�������ӣ�

- 1. ��`$HOME/src/`Ŀ¼�µ������ļ����Ƶ�Զ��������`$HOME/src/`Ŀ¼
  ```
  cd && tar czv src | ssh user@host 'tar xz'
  ```

- 2. ��Զ������`$HOME/src/`Ŀ¼�µ������ļ������Ƶ��û��ĵ�ǰĿ¼
  ```
  ssh user@host 'tar cz src' | tar zxv
  ```
- 3. �鿴Զ�������Ƿ����н���httpd
  ```
  ssh user@host 'ps ax | grep httpd'
  ```
���Զ�̿����Ƽ�ʹ��`scp`

### 4. scp���Զ�̿���

scp�����÷��鿴[��ƪ����](command/scp.md)

### 5. ssh�˿ڲ���

- **1. �󶨱��ض˿�**

  ��Ȼssh���Դ������ݣ���ô���ǿ�������Щ�����ܵ��������ӣ�ȫ������ssh���ӣ��Ӷ���߰�ȫ��

  ���磬����Ҫ��8080�˿ڵ����ݣ���ͨ��ssh����Զ�����������������д��
  ```
  ssh -D 8080 user@host
  ```
  ssh�ͻὨ��һ��socket��ȥ�������ص�8080�˿ڡ�һ�������ݴ����ö˿ڣ����Զ�����ת�ƶ�ssh�������棬����Զ�������������������8080�˿�ԭ����һ�������ܶ˿ڣ����ڽ����һ�����ܶ˿�
  
  ��������`-D`ʵ�ֿ�ѧ����������ο� [�ý̳�](https://www.huiyingwu.com/353/) | [��ѧ������ԭ��](https://segmentfault.com/a/1190000011485579)

  **ע��** windows�¿���ʹ��bitvise ssh client���ߴﵽ��ǽ��Ч��������ο�[�ý̳�](https://www.cnblogs.com/plokmju/p/SSH_Chrome_SwitchySharp_BitviseTunnelier.html)

- **2. ���ض˿�ת��**

  ����host1�Ǳ���������host2��Զ����������������ԭ������̨����֮���޷���ͨ�����ǻ�������һ̨host3������ͬʱ��ͨǰ����̨����������ssh���ض˿�ת�����Խ���host3��ͨhost1��host2������ͨ����host1�������������ʵ�֣�
  ```
  ssh -L 2121:host2:21 host3
  ```

  L������������ֵ���ֱ���"���ض˿�:Ŀ������:Ŀ�������˿�"������֮����ð�Ÿ����������������˼�����ǽ����ض˿�2121������ͨ��host3ת����Ŀ������host2��21�˿���

  ����������ֻҪ����host1��2121�˿ڣ��Ϳ�������host2��ftp����(�ٶ�host2����ftp������������Ĭ�϶˿�21��)
  ```
  ftp localhost:2121
  ```

  "���ض˿�ת��"ʹ��host1��host2֮���γ���һ�����ݴ����������������˱���Ϊ"ssh����"

  ��������Ȥ�����ӣ�

  - ���ض˿ں�Զ�̶˿ڰ�

    ```
    ssh -L 5900:localhost:5900 host3
    ```

    ���ʾ��������5900�˿ں�host3��5900�˿ڰ�(�����localhostָhost3����ΪĿ�������������host3���Ե�)

  - host1����ssh��¼host2

    ```
    ssh -L 9001:host2:22 host3
    ```

    ������ֻҪssh��¼������9001�˿ڣ����൱�ڵ�¼host2��

    ```
    ssh -p 9001 localhost
    ```


- **3.  Զ�̶˿�ת��**

  ���ſ������Ǹ����ӣ�host1��host2֮���޷���ͨ���������host3ת�������ǣ�������������ˣ�host3��һ̨��������������������������host1�����Ƿ������Ͳ��У�������host1������������host3����ʱ��"���ض˿�ת��"�Ͳ������ˣ���ô�죿

  ����취�ǣ���Ȼhost3������host1����ô�ʹ�host3�Ͻ�����host1��ssh���ӣ�Ȼ����host1��ʹ���������ӾͿ�����

  ��host3��ִ����������
  ```
  ssh -R 2121:host2:21 host1
  ```

  R����Ҳ��������ֵ���ֱ���"Զ�������˿�:Ŀ������:Ŀ�������˿�"�������������˼��������host1�����Լ���2121�˿ڣ�Ȼ���������ݾ���host3��ת����host2��21�˿ڡ����ڶ���host3��˵��host1��Զ��������������������ͱ���Ϊ"Զ�̶˿ڰ�"

  ��֮�����ǾͿ�����host1������host2�ˣ�
  ```
  ftp localhost:2121
  ```
  
### 6. ssh��������

sshһ������������˿�ת��ʱ��һ�㻹������������ϡ�

- -f ssh�ں�̨���У�����֤֮��ssh�˾Ӻ�̨
- -T ��Ҫ����tty�ն�
- -N ��Ҫ�ڷ�����ִ������
- -C ѹ�����ݰ�
- -i ָ����֤��Կ�ļ�
- -n �� stdio �ض��� /dev/null����-f���ʹ��
- -p ָ�����Ӷ˿�
- -X Enables X11 forwarding.
- -q ����ģʽ

һ���������Ͷ˿�ת��ʱ��ʹ��`-f`��`-T`��`-N`��`-n`��`-C`ѡ�
```
ssh -fTNnC -D user@host
```


[�ο��̳�](https://blog.csdn.net/pipisorry/article/details/52269785)

[sshת��������ssh-agent�÷����](https://www.cnblogs.com/f-ck-need-u/p/10484531.html)

O��RELLY�ġ�SSH: The Secure Shell - The Definitive Guide��