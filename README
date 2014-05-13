=nginx_upstream_jvm_route=
-------

This module achieves session stickiness with the session cookie. If the session is not in the cookie
or URL, the module will be a normal Round-Robin upstream module.

=INSTALLATION=
    
    cd nginx-0.7.59 # or whatever
    patch -p0 < /path/to/this/directory/jvm_route.patch
  
compile nginx with the following addition option:

  --add-module=/path/to/this/directory

=DIRECTIVES=

    ==jvm_route==

    syntax: jvm_route $cookie_SESSION_COOKIE[|session_url] [reverse]
    default: none
    context: upstream
    description: 
    '$cookie_SESSION_COOKIE' specifies the session cookie name(0.7.24+). 'session_url' specifies a
    different session name in the URL when the client does not accept a cookie. The session name is
    case-insensitive. In this module, if it does not find the session_url, it will use the session
    cookie name instead. So if the session name in cookie is the name with its in URL, you don't
    need give the session_url name.  
    With scanning this cookie, the module will send the request to right backend server. As far as I
    know, the resin's srun_id name is in the head of cookie. For example, requests with cookie value
    'a***' are always sent to the server with the srun_id of 'a'. But tomcat's JSESSIONID is
    opposite, which is like '***.a'. The parameter of 'reverse' specifies the cookie scanned from
    tail to head.
    If the request fails to be sent to the chosen backend server, It will try another server with
    the Round-Robin mode until all the upstream servers tried. The directive proxy_next_upstream can
    specify in what cases the request will be transmitted to the next server. If you want to force
    the session sticky, you can set 'proxy_next_upstream off'.


    ==jvm_route_status==

    syntax: jvm_route_status upstream_name
    default: none
    context: location
    example:
        location status {
            jvm_route_status backend;
        }
    description: 
    return the status of the jvm_route peers, like this: 

        upstream backend: total_busy = 10, total_requests = 311, current_peer 15/18

        peer 0: 172.19.0.126:80(g) down: 0, fails: 0/1, busy: 0/0, weight: 4/4, total_req: 60, last_req: 298, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 1: 172.19.0.120:80(a) down: 0, fails: 0/1, busy: 1/1, weight: 1/1, total_req: 15, last_req: 295, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 2: 172.19.0.121:80(b) down: 0, fails: 0/1, busy: 0/1, weight: 0/1, total_req: 16, last_req: 299, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 3: 172.19.0.122:80(c) down: 0, fails: 0/1, busy: 0/1, weight: 0/1, total_req: 16, last_req: 300, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 4: 172.19.0.123:80(d) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 301, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 5: 172.19.0.124:80(e) down: 0, fails: 1/1, busy: 0/1, weight: 1/1, total_req: 2, last_req: 216, total_fails: 2, fail_acc_time: Wed Nov 18 11:19:37 2009
        peer 6: 172.19.0.125:80(f) down: 0, fails: 0/1, busy: 0/0, weight: 0/1, total_req: 16, last_req: 302, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 7: 172.19.0.127:80(h) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 303, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 8: 172.19.0.128:80(i) down: 0, fails: 0/1, busy: 0/1, weight: 0/1, total_req: 16, last_req: 304, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 9: 172.19.0.129:80(j) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 305, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 10: 172.19.0.130:80(k) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 306, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 11: 172.19.0.131:80(l) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 307, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 12: 172.19.0.132:80(m) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 308, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 13: 172.19.0.235:80(n) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 309, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 14: 172.19.0.236:80(o) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 14, last_req: 310, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 15: 172.19.0.237:80(p) down: 0, fails: 0/1, busy: 1/1, weight: 0/1, total_req: 16, last_req: 311, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 16: 172.19.0.238:80(q) down: 0, fails: 0/1, busy: 0/1, weight: 1/1, total_req: 15, last_req: 292, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970
        peer 17: 172.19.0.239:80(r) down: 0, fails: 0/1, busy: 0/1, weight: 1/1, total_req: 15, last_req: 293, total_fails: 0, fail_acc_time: Thu Jan  1 08:00:00 1970

    total_busy is the sum of all the backend servers' active connections.
    total_requests is all the count of requests which had proxied to backend.
    current_peer is meaningful with the Round Robin mode when the session cookie is absent.

    down is the state of backend server whether is configured with 'down'.
    fails is the failure requests count in the interval of 'fail_timeout'.
    busy is the current active connections of the backend server.
    weight is the current weight of server and just meaningful with the Round Robin module.
    total_req is the count of requests which had proxied to this backend server.
    last_req is the last request's id proxied by this server.
    total_fails is the count of failure requests which had proxied to the this backend server.
    fail_acc_time stands for the last failure access time.

    ==server==

    Main syntax is the same as the official directive. 
    This module add these parameters:
    'srun_id': identifies the backend JVM's name by cookie. The default srun_id's value is 'a'. The
    name can be more than one letter.
    'max_busy': the maximum of active connections with the backend server. The default value is 0
    which means unlimited. If the server's active connections is higher than this parameter, it will
    not be chosen until the server is less busier. If all the servers are busy, Nginx will return
    502.
     
    NOTE: This module does not support the parameter of 'backup' yet.
 
=EXAMPLE=

1.For resin with nginx

upstream backend {
    server 192.168.0.100 srun_id=a;
    server 192.168.0.101 srun_id=b;
    server 192.168.0.102 srun_id=c;
    server 192.168.0.103 srun_id=d;

    jvm_route $cookie_JSESSIONID;
}

For all resin servers' configure:

    <server id="a" address="192.168.0.100" port="8080">
    <http id="" port="80"/>
    </server>
    <server id="b" address="192.168.0.101" port="8080">
    <http id="" port="80"/>
    </server>
    <server id="c" address="192.168.0.102" port="8080">
    <http id="" port="80"/>
    </server>
    <server id="d" address="192.168.0.103" port="8080">
    <http id="" port="80"/>
    </server>

And start each resin instances like this:
    server a
    shell $> /usr/local/resin/bin/httpd.sh -server a start

    server b
    shell $> /usr/local/resin/bin/httpd.sh -server b start

    server c
    shell $> /usr/local/resin/bin/httpd.sh -server c start

    server d
    shell $> /usr/local/resin/bin/httpd.sh -server d start

2.For tomcat with nginx
upstream backend {
    server 192.168.0.100 srun_id=a;
    server 192.168.0.101 srun_id=b;
    server 192.168.0.102 srun_id=c;
    server 192.168.0.103 srun_id=d;

    jvm_route $cookie_JSESSIONID reverse;
}

Each tomcats' configure:
    Tomcat a:
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="a">
    Tomcat b:
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="b">
    Tomcat c:
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="c">
    Tomcat d:
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="d">

3. A simple java test page
index.jsp

<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>

<html>
<head>
</head>
<body>
$your_jvm_name
<br />
<%out.print(request.getSession()) ;%> <br />
<%out.println(request.getHeader("Cookie"))&nbsp;%>
</body>
</html>

Note: This is a third-party module. And you need careful test before using this module in your
product environmenta.

Questions/patches may be directed to Weibin Yao, yaoweibin@gmail.com.
