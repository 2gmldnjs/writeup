[hacklab: vulnix](https://www.vulnhub.com/entry/hacklab-vulnix,48/)

# targetdiscovery
***
### nmap -sn 192.168.25.29-255 --open
![1](https://user-images.githubusercontent.com/69203345/112837596-0d462000-90d7-11eb-8a28-aaaed4ddc884.PNG)

# enumerate + exploitation
***
## target: 192.168.25.31
### nmap -O -sV -sS -p- 192.168.25.31
![2](https://user-images.githubusercontent.com/69203345/112838010-8c3b5880-90d7-11eb-90df-801ff4dd957f.PNG)     
#### smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.25.31
![3](https://user-images.githubusercontent.com/69203345/112838855-7a0dea00-90d8-11eb-8968-458d5dfdf6ce.PNG)        
사용자목록을 하나 만들어주자       
![4](https://user-images.githubusercontent.com/69203345/112839158-d3761900-90d8-11eb-87a4-c38989fa4f35.PNG)      
finger를 이용해서 유저 하나하나 해서 2명을 알아냈다 (user, vulnix)        
![4-2](https://user-images.githubusercontent.com/69203345/112843821-f6ef9280-90dd-11eb-92c8-99128d7e1f10.PNG)      
nfs 가 2049포트에 활성화되있다    
showmount 명령어를 활용해 공유되고 있는 디렉터리를 확인후 공격서버에 마운트 해보자      
![5](https://user-images.githubusercontent.com/69203345/112840646-901caa00-90da-11eb-9815-f3ef394016e3.PNG)           
근데 권한이 없어서 못들어갔다     
#### showmount 192.168.25.31
shomount -e 192.168.25.31      
![6](https://user-images.githubusercontent.com/69203345/112840932-dbcf5380-90da-11eb-91fa-da7b16ae60ea.PNG)
#### hydra -l user -P rockyou.txt 192.168.25.31 ssh -t 4 
ssh무차별 대입      
![7](https://user-images.githubusercontent.com/69203345/112842729-d3781800-90dc-11eb-8b8b-2337fd5d68bf.PNG)       

# privilege escalation 
***   
user letmein      
![8](https://user-images.githubusercontent.com/69203345/112843206-4ed9c980-90dd-11eb-9f8b-2dae3ff7f5e0.PNG)        
당연히 바로 루트 전환은 안된다 ㅋㅋㅋㅋ     
이 유저는 sudo를 사용할수 없다고 한다        
user로는 할일이 별로 없어 보이니까 vulnix에 대한 UID를 가져 와서 시스템에 임시 사용자를 만들어 액세스해보자       
![9](https://user-images.githubusercontent.com/69203345/112844404-a9bff080-90de-11eb-8a14-0a089a5cdbaa.PNG)        
![10](https://user-images.githubusercontent.com/69203345/112844785-163aef80-90df-11eb-80bc-3bad67ea7497.PNG)      
vulnix 유저로 접속을 하기 위해 ssh key를 생성하자     
순서
ssh-keygen으로 ssh key 만들어서 vulnix 사용자로 /home/vulnix 아래 .ssh 디렉터리를 어서 
Public key 를 .ssh 에 복붙      
![11](https://user-images.githubusercontent.com/69203345/112846179-ba716600-90e0-11eb-9f25-27486b1af26e.PNG)      
![12](https://user-images.githubusercontent.com/69203345/112846413-fdcbd480-90e0-11eb-8094-2cfb3c5edde2.PNG)        
ssh vulnix 접속 후 sudo -l      
![13](https://user-images.githubusercontent.com/69203345/112846903-7fbbfd80-90e1-11eb-93ad-fc500891731f.PNG)      
vulnix 사용자가 root 권한으로sudoedit /etc/exports를 실행시킬 수 있다      
![14](https://user-images.githubusercontent.com/69203345/112847287-e3dec180-90e1-11eb-9e08-273888e941b3.PNG)       
/root 추가      
![15](https://user-images.githubusercontent.com/69203345/112847751-5b145580-90e2-11eb-9211-f5275eb2c485.PNG)       
VM을 다시 시작하고 공유 디렉터리를 다시 마운트하고 /bin/bash복사해서 setuid 권한을 부여     
![16](https://user-images.githubusercontent.com/69203345/112850416-eee72100-90e4-11eb-90df-479336da25c6.PNG)       
이제 상대 서버에 침투해 쉘을 실행시킬 수 있다.        
![17](https://user-images.githubusercontent.com/69203345/112850903-6b79ff80-90e5-11eb-9c20-aff8cbe4ca27.PNG)        
안된다... 왜..?     
마운트할때 /home/vulnix가 아닌 /root로 들어가 보자 (sudoedit에서 추가한 그거 맞다)        
\+ 새 디렉토리를 만든후에 들어가보도록 하자     
![18](https://user-images.githubusercontent.com/69203345/112852186-a597d100-90e6-11eb-86a8-5ae75bdf820a.PNG)      

잘 모르겠다 다시 한번 해봐야 겠다..

ㄲ-ㅌ

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
root_squash : Root squash is a technique to void privilege escalation on the client machine via suid executables Setuid.      
Without root squash, an attacker can generate suid binaries on the server that are executed as root on other client,        
even if the client user does not have superuser privileges.        
root_squash는 suid 실행 파일인 Setuid를 통해 클라이언트 시스템에서 권한 상승을 무효화하는 기술이다.      
루트스쿼시 기능을 꺼 놓으면(no_root_squash) 상대편 서버에서 루트 권한으로 실행 가능한 SUID 바이너리를 생성할 수 있다.        
