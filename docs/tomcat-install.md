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
1. [/etc/firewalld/zones/public.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/etc/firewalld/zones/public.xml) 생성
2. [/usr/lib/firewalld/services/tomcat.xml](https://github.com/bitacademy-poscodx/rocky-practices/blob/main/lx/usr/lib/firewalld/services/tomcat.xml) 생성
3. 방화벽 서비스 재실행
```sh
# systemctl stop firewalld
# systemctl start firewalld
```

7. 브라우저로 접근 하기
   http://192.168.80.203:8080

8. 중지 시키기
   # /usr/local/poscodx/tomcat/bin/catalina.sh stop

9. 서비스 등록 하기
   /usr/lib/systemd/system/tomcat.service 파일 생성
   # systemctl enable tomcat

10. tomcat 서비스 실행/중지/재실행
   # systemctl start tomcat
   # systemctl stop tomcat
   # systemctl restart tomcat


11. tomcat manager 설정
   1) tomcat-users.xml 설정
      # vi /usr/local/poscodx/tomcat/conf/tomcat-users.xml

========================================================
<tomcat-users>
  . . .
  . . . 
  <role rolename="manager"/>
  <role rolename="manager-gui" />
  <role rolename="manager-script" />
  <role rolename="manager-jmx" />
  <role rolename="manager-status" />
  <role rolename="admin"/>
  <user username="admin" password="manager" roles="admin,manager,manager-gui, manager-script, manager-jmx, manager-status"/>

</tomcat-users>
========================================================
   2) /usr/local/poscodx/tomcat/webapps/manager/META-INF/context.xml
========================================================
 주석 처리
<Context>
 ....
</Context>

새로 다음내용 추가
<Context antiResourceLocking="false" privileged="true" docBase="${catalina.home}/webapps/manager">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="^.*$" />
</Context>

========================================================

12. tomcat 재시작
    # systemctl stop tomcat
    # ps -ef | grep tomcat
    # systemctl start tomcat

13. http://192.168.80.131/manager
