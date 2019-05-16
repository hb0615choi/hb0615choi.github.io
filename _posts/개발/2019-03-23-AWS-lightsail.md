# AWS Lightsail 서비스
 - node 와 mysql 서버를 위해 선택
 - 저렴하고 예측 가능한 비용 (최저 3.5$)


# Ubuntu 14 설치 완료
## Brew 를 통해 다른 컴포넌트 설치하려고 하였으나 mysql 은 결국 설치 실패.
 -  apt-get 통해서 설치 완료

## 한글 설치가 까다로움

### ubuntu 한글 설정 변경

- 아래 링크 통해서 변경 완료
https://seulkom.tistory.com/100


### mysql 한글 설정 변경

 - 아래 링크 통해서 하되, utf8 을 utf8mb 로 설치해야 되었다.
https://iyc1030.tistory.com/entry/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%9A%B0%EB%B6%84%ED%88%AC-MYSQL-%ED%95%9C%EA%B8%80-%EC%84%A4%EC%A0%95

> 한글의 경우 4byte utf8 을 사용하여 3byte utf8 의 경우 한글이 깨짐.


## MySQL 외부 접속 허용
> https://zetawiki.com/wiki/MySQL_%EC%9B%90%EA%B2%A9_%EC%A0%91%EC%86%8D_%ED%97%88%EC%9A%A9

````
> SELECT Host,User,authentication_string FROM mysql.user;
````

````
INSERT INTO mysql.user (host,user,authentication_string,ssl_cipher, x509_issuer, x509_subject) VALUES ('%','weprops',password('1qaz2wsx##'),'','','');
GRANT ALL PRIVILEGES ON *.* TO 'weprops'@'%';
FLUSH PRIVILEGES;
````

### mysql csv import
```
mysql> SELECT @@GLOBAL.secure_file_priv;

+---------------------------+
| @@GLOBAL.secure_file_priv |
+---------------------------+
| /var/lib/mysql-files/     |
+---------------------------+
1 row in set (0.00 sec)
```

```
$ vim /etc/mysql/my.cnf

[mysqld]
secure-file-priv=""
```

> https://sssunho.tistory.com/56


### forever
```
$ forever start -l server.log --minUptime 5000 --spinSleepTime 2000 -a server.js

$ forever restart server.js

$ forever start $(which http-server) ~/workspace/weprops/react_build/ -p 8080 -d false
```

>http://todactodac.blogspot.com/2016/06/nodejs-forever.html


### forward port 80 to 8080
```
$ sudo iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j $ REDIRECT --to-port 1337
$ sudo apt-get install iptables-persistent
$ sudo /etc/init.d/iptables-persistent save  => not working
```
> https://eladnava.com/binding-nodejs-port-80-using-nginx/
> nginx not working