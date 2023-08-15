The following demonstrates a method for creating docker bridge netwroks and
and attaching containers to the network:

Step 1: Create a bridge network
<code>
docker network create my_network
</code>

Output:
<code>
C:\Users\M.Ali>docker network create my_network
2eafd996706f058b4af5fd5620e73cc914161f2e2c882d34ca677d752e970277
</code>

Step 2: create an nginx container mapped on desktops port 80 and connect it to "my_network"
<code>
docker run -d --name nginx_container --network my_network -p 8080:80 nginx
</code>

Output:
<code>
C:\Users\M.Ali>docker run -d --name nginx_container --network my_network -p 8080:80 nginx
5497e9a5f152eb30371901e9d2fe825be69a14da078b4f86cdb68265f1b1acb5
</code>
Logs:
<code>
2023-08-15 15:45:08 /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
2023-08-15 15:45:08 /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
2023-08-15 15:45:08 /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
2023-08-15 15:45:08 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
2023-08-15 15:45:08 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
2023-08-15 15:45:08 /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
2023-08-15 15:45:08 /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
2023-08-15 15:45:08 /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
2023-08-15 15:45:08 /docker-entrypoint.sh: Configuration complete; ready for start up
2023-08-15 15:48:49 172.18.0.1 - - [15/Aug/2023:19:48:49 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36" "-"
2023-08-15 15:48:50 172.18.0.1 - - [15/Aug/2023:19:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36" "-"
2023-08-15 16:03:05 172.18.0.1 - - [15/Aug/2023:20:03:05 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36" "-"
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: using the "epoll" event method
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: nginx/1.25.1
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: OS: Linux 5.4.72-microsoft-standard-WSL2
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker processes
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 29
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 30
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 31
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 32
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 33
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 34
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 35
2023-08-15 15:45:08 2023/08/15 19:45:08 [notice] 1#1: start worker process 36
2023-08-15 15:48:50 2023/08/15 19:48:50 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
</code>

Step 3: Open the link http://localhost:8080 to verify if the default page of nginx is accessible

Output:


Step 4: Create another container for httpd mapped on port 80 and add it to "my_network"
<code>
docker run -d --name httpd_container --network my_network -p 8081:80 httpd
</code>

Output:
<code>
C:\Users\M.Ali>docker run -d --name httpd_container --network my_network -p 8081:80 httpd
Unable to find image 'httpd:latest' locally
latest: Pulling from library/httpd
648e0aadf75a: Already exists
c76ba39af630: Already exists
b9819ffb14ec: Already exists
37baa60548e6: Already exists
6dbce5de7542: Already exists
Digest: sha256:d7262c0f29a26349d6af45199b2770d499c74d45cee5c47995a1ebb336093088
Status: Downloaded newer image for httpd:latest
f3e3a691bbb836cc0ddf5354c02b5afcf48c5a70efff494d38339e4fc2c59828
</code>

Step 5: Open the link http://localhost:8081 to verify if the default page of httpd is accessible

Output:

Logs:
<code>
2023-08-15 15:58:55 172.18.0.1 - - [15/Aug/2023:19:58:55 +0000] "GET / HTTP/1.1" 200 45
2023-08-15 15:58:55 172.18.0.1 - - [15/Aug/2023:19:58:55 +0000] "GET /favicon.ico HTTP/1.1" 404 196
2023-08-15 15:51:58 AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
2023-08-15 15:51:58 AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
2023-08-15 15:51:58 [Tue Aug 15 19:51:58.942571 2023] [mpm_event:notice] [pid 1:tid 140587431561088] AH00489: Apache/2.4.57 (Unix) configured -- resuming normal operations
2023-08-15 15:51:58 [Tue Aug 15 19:51:58.943950 2023] [core:notice] [pid 1:tid 140587431561088] AH00094: Command line: 'httpd -D FOREGROUND'
</code>

Step 6: Display information regarding "my_network"
<code>
docker network inspect my_network
</code>

Output:
<code>
[
    {
        "Name": "my_network",
        "Id": "2eafd996706f058b4af5fd5620e73cc914161f2e2c882d34ca677d752e970277",
        "Created": "2023-08-15T19:37:38.8766256Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "5497e9a5f152eb30371901e9d2fe825be69a14da078b4f86cdb68265f1b1acb5": {
                "Name": "nginx_container",
                "EndpointID": "c94d20035881d0decab0757920cf72a88a1a7bc53634f0383f3364f5e74d20fe",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "f3e3a691bbb836cc0ddf5354c02b5afcf48c5a70efff494d38339e4fc2c59828": {
                "Name": "httpd_container",
                "EndpointID": "75d40427560a688a2bdf7e714cea1a79ef7aabf5a17d7f7de79409d7da71dcf9",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
</code>

Step 7: Stop and remove the "nginx_container" container
<code>
docker stop nginx_container
docker rm nginx_container
</code>

Output:
<code>
C:\Users\M.Ali>docker stop nginx_container
nginx_container

C:\Users\M.Ali>docker rm nginx_container
nginx_container
</code>

Step 8: Create a new nginx container mapped on port 80 and connect it to "my_network"
<code>
docker run -d --name nginx_container_2 --network my_network -p 8082:80 nginx
</code>

Output:
<code>
C:\Users\M.Ali>docker run -d --name nginx_container_2 --network my_network -p 8082:80 nginx
d3f87601d956f757f054d1d6fa91f61490c175eeadff8a52940150ec906909ad
</code>

Step 9: Open the link http://localhost:8082 to verify if the default page of nginx is accessible
 
Output:

Logs:
2023-08-15 16:42:56 /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
2023-08-15 16:42:56 /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
2023-08-15 16:42:56 /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
2023-08-15 16:42:56 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
2023-08-15 16:42:56 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
2023-08-15 16:42:56 /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
2023-08-15 16:42:56 /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
2023-08-15 16:42:56 /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
2023-08-15 16:42:56 /docker-entrypoint.sh: Configuration complete; ready for start up
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: using the "epoll" event method
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: nginx/1.25.1
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: OS: Linux 5.4.72-microsoft-standard-WSL2
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker processes
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 29
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 30
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 31
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 32
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 33
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 34
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 35
2023-08-15 16:42:56 2023/08/15 20:42:56 [notice] 1#1: start worker process 36
2023-08-15 16:47:33 2023/08/15 20:47:33 [error] 29#29: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.18.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8082", referrer: "http://localhost:8082/"
2023-08-15 16:47:33 172.18.0.1 - - [15/Aug/2023:20:47:33 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36" "-"
2023-08-15 16:47:33 172.18.0.1 - - [15/Aug/2023:20:47:33 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8082/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36" "-"

Step 10: Diplay running containers
<code>
docker container ls
</code>

Output:
<code>
C:\Users\M.Ali>docker container ls
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS             PORTS                  NAMES
d3f87601d956   nginx     "/docker-entrypoint.…"   10 minutes ago      Up 10 minutes      0.0.0.0:8082->80/tcp   nginx_container_2
f3e3a691bbb8   httpd     "httpd-foreground"       About an hour ago   Up About an hour   0.0.0.0:8081->80/tcp   httpd_container
</code>

Step 11: Stop and remove all running containers
<code>
docker container stop nginx_container_2
docker container stop httpd_container
docker container rm nginx_container_2
docker container rm httpd_container
</code>

Output:
<code>
C:\Users\M.Ali>docker container stop nginx_container_2
nginx_container_2

C:\Users\M.Ali>docker container stop httpd_container
httpd_container

C:\Users\M.Ali>docker container rm nginx_container_2
nginx_container_2

C:\Users\M.Ali>docker container rm httpd_container
httpd_container
</code>

Step 12: Remove "my_network"
<code>
docker network rm my_network
</code>

Output:
<code>
C:\Users\M.Ali>docker network rm my_network
my_network
</code>