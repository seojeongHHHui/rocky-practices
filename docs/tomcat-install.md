## Tomcat 설치

1. tomcat9 다운로드
```sh
   # wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.89/bin/apache-tomcat-9.0.89.tar.gz
```

2. 압축 풀기
```sh
   # tar xvfz apache-tomcat-9.0.89.tar.gz
```

3. 설치
```sh
   # mv apache-tomcat-9.0.89 /usr/local/poscodx
   # cd /usr/local/poscodx
   # ln -s apache-tomcat-9.0.89 tomcat
```

4. 포트 확인(/usr/local/poscodx/tomcat/conf/server.xml)
```xml

...

 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
...

```

5. 실행
```sh
   # /usr/local/poscodx/tomcat/bin/catalina.sh start
   # ps -ef | grep tomcat
```

6. 방화벽(firewalld, 8080 포트) 열기
   - [/etc/firewalld/zones/public.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/etc/firewalld/zones/public.xml) 생성
   - [/usr/lib/firewalld/services/tomcat.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/usr/lib/firewalld/services/tomcat.xml) 생성
   - 방화벽 서비스 재실행
   ```sh
   # systemctl stop firewalld
   # systemctl start firewalld
   ```

7. 브라우저로 접근 하기
   http://192.168.80.203:8080

8. 중지 시키기
```sh
# /usr/local/poscodx/tomcat/bin/catalina.sh stop
```

9. 서비스 등록 하기([/usr/lib/systemd/system/tomcat.service](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/usr/lib/systemd/system/tomcat.service))
```sh
# systemctl enable tomcat
```

10. tomcat 서비스 실행/중지/재실행
```sh
# systemctl start tomcat
# systemctl stop tomcat
# systemctl restart tomcat
```

11. tomcat manager 설정
   - [/usr/local/poscodx/tomcat/conf/tomcat-users.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/usr/local/poscodx/tomcat/conf/tomcat-users.xml) 수정
   - [/usr/local/poscodx/tomcat/webapps/manager/META-INF/context.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/usr/local/poscodx/tomcat/webapps/manager/META-INF/context.xml) 수정
      
12. tomcat 재시작
```sh
# systemctl stop tomcat
# ps -ef | grep tomcat
# systemctl start tomcat
```

13. Manager 앱 접근
http://192.168.80.131/manager
