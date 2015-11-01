##Git��װ����֮�����ȵ����ø�����Ϣ����Ϊ�Ƿֲ�ʽ��ϵͳ�����Խ�����ʱ��Ҫ�Ա�����
```bash
$ git config --global user.name "Your Name"  
$ git config --global user.email "email@example.com"  
```  

##1	�½�һ���ֿ⣬���Ҹòֿ�Ӧ����git����  

	1.1 git init�½�git���͵Ĳֿ�

##2	�½��ļ�������git�ֿ�    

	
	2.1	touch a.txt   touch a.txt����vi  
	2.2	git add ���½���a.txt����git����  
	2.3	git status,�鿴�ļ���git�ֿ��е�״̬
	2.4	git commit -m "�ύ�޸ĵ���Ϣ˵��",������״��ύ
	2.5	�����ļ����ݣ��ٴγ����ύ��```

##3	git����־�͸��ٹ��� 
 
	
```bash
    3.1	git log,�鿴ÿ�β�������־�����
		git log --pretty=oneline����һ����ʾ���鿴�ؼ���Ϣ
	3.2	git diff,�鿴���ݲ�ͬ��
	3.3 git checkout -- file �����������޸ģ���ʵ���ð汾����İ汾�滻                         
	    �������İ汾�� 
	3.4 git reset HEAD file  �����ݴ����޸ģ��ٽ���3.3
	3.5 rm file,git rm file;   ɾ���ļ�������checkout�ָ�
```

##4	git�汾�Ļ���  

	4.1	��һ����git reset --hard HEAD^��ָ�����һ����
	4.2	�˶ಽV1��git reset --hard HEAD^^^^^^^^^^,�������
	4.3	�˶ಽV2��git reset --hard HEAD~���ֲ���
	4.4	����Խ��git reflog���ͷ7λ�汾�ţ�Ȼ��
			git reset --hard 7λ�汾��

##5	git����  
    
```bash
5.1 Working directory        ������
5.2 Repository               �汾��
5.3 Stage��index��           �ݴ���
```

##5  gitԶ�ֿ̲�  

###��1��������SSH Key�����û���Ŀ¼�£�������û��.sshĿ¼������У��ٿ������Ŀ¼����û��id_rsa��id_rsa.pub�������ļ�������Ѿ����ˣ���ֱ��������һ�������û�У���Shell��Windows�´�Git Bash��������SSH Key��
```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```  
�û���Ŀ¼���ҵ�.sshĿ¼��������id_rsa��id_rsa.pub�����ļ�������������SSH Key����Կ�ԣ�id_rsa��˽Կ������й¶��ȥ��id_rsa.pub�ǹ�Կ�����Է��ĵظ����κ��ˡ�  
###��2������½GitHub���򿪡�Account settings������SSH Keys��ҳ�棺

Ȼ�󣬵㡰Add SSH Key������������Title����Key�ı�����ճ��id_rsa.pub�ļ������ݡ�  
###��3���������⣬�뱾�ع���������GitHub����ʾ���ڱ��ص�learngit�ֿ����������
```bash
$ git remote add origin git@github.com:michaelliao/learngit.git
```
###��4�����Ϳ��԰ѱ��ؿ�������������͵�Զ�̿��ϣ�
```bash
$ git push -u origin master
```
�ѱ��ؿ���������͵�Զ�̣���git push���ʵ�����ǰѵ�ǰ��֧master���͵�Զ��.����Զ�̿��ǿյģ����ǵ�һ������master��֧ʱ��������-u������Git������ѱ��ص�master��֧�������͵�Զ���µ�master��֧������ѱ��ص�master��֧��Զ�̵�master��֧�������������Ժ�����ͻ�����ȡʱ�Ϳ��Լ����
```bash
$ git push origin master
```
```bash
Ҫ����һ��Զ�̿⣬ʹ������
git remote add origin git@server-name:path/repo-name.git��

������ʹ������git push -u origin master��һ������master��֧���������ݣ�

�˺�ÿ�α����ύ��ֻҪ�б�Ҫ���Ϳ���ʹ������git push origin master���������޸ģ�
```

###��5��  
Ҫ��¡һ���ֿ⣬���ȱ���֪���ֿ�ĵ�ַ��Ȼ��ʹ��git clone�����¡��
```bash
git clone
```	
Git֧�ֶ���Э�飬����https����ͨ��ssh֧�ֵ�ԭ��gitЭ���ٶ���졣
###��6��  
git remote�鿴Զ�̿���Ϣ
###��7��
```bash
�������push���󣬿���push -fǿ��
Ҳ����pull������Ҫ�ȣ�
    $ git config branch.master.remote origin 
    $ git config branch.master.merge refs/heads/master 
```


##6	git��֧  

	6.1	git branch �鿴��֧
	6.2	git branch ��֧����  �½���֧
	6.3	git checkout ��֧��  �л���֧
	6.4	git merge dev		��master��֧�Ϻϲ�dev��֮����
	6.5	git branch -d ��֧��	ɾ����֧
	6.6	git checkout -b ��֧��	�½�+�л�һ���㶨
	6.7 git merge --no-ff -m "merge with no-ff" dev
	6.8 $ git stash  һ����git stash apply�ָ������ǻָ���stash���ݲ���ɾ��������Ҫ��git stash drop��ɾ������һ�ַ�ʽ����git stash pop���ָ���ͬʱ��stash����Ҳɾ�ˣ�$ git stash list�鿴
	6.8 git branch -D feature-vulcanɾ��û��merge�ķ���
	6.8 git remote�鿴Զ�̿���Ϣ
 
##7	��һ�ֳ�ͻ  

	��֧�ϲ���ĳ�ͻ����ν����VCR��
	
	�ڶ��ֳ�ͻ
	push�ύ������ݳ�ͻ������pull�������˹���Ԥ�չ��ϲ�����push

	�����ֳ�ͻTortoiseGit��TortoiseSVN��ͬ���Ĳ������
	TortoiseGit--��ɫ���Ǹ�̾��---edit conflict---merge---resolve--commit--->OK

	�����ֳ�ͻ��Egit����
	�г�ͻ����pull�������Vcr


##8	TortoiseGit  

		

##9	EGit   

```bash
[branch "master"]
    remote = origin
    merge = refs/heads/master

[remote "origin"]
    url = your httpsaddress
    fetch = +refs/heads/*:refs/remotes/origin/*
    push = refs/heads/master:refs/heads/master
```
����Է���ʱ��,����һ�·��������صĶԱȡ��ܷ�ʱ�䣬���Ǻ���Ч����ҪäĿ��pull