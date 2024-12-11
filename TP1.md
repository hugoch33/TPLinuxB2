# TP1 : Premiers pas Docker



🌞 Ajouter votre utilisateur au groupe docker

```
[hugo@TP1Linux ~]$ sudo usermod -aG docker hugo
[hugo@TP1Linux ~]$ exit
logout
Connection to 10.1.1.25 closed.
PS C:\Users\chama> ssh hugo@10.1.1.25
[hugo@TP1Linux ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[hugo@TP1Linux ~]$
```


🌞 Lancer un conteneur NGINX

```
[hugo@TP1Linux ~]$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bc0965b23a04: Pull complete
650ee30bbe5e: Pull complete
8cc1569e58f5: Pull complete
362f35df001b: Pull complete
13e320bf29cd: Pull complete
7b50399908e1: Pull complete
57b64962dd94: Pull complete
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
06f6fc676e59e0f2c91aaf2187eb7c49801fd99230c35bad28ab6b1673cb7d4c
```

🌞 Visitons

```
[hugo@TP1Linux ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS
             NAMES
06f6fc676e59   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   dazzling_keller

[hugo@TP1Linux ~]$ sudo docker logs -f 06
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/11 16:16:18 [notice] 1#1: using the "epoll" event method
2024/12/11 16:16:18 [notice] 1#1: nginx/1.27.3
2024/12/11 16:16:18 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2024/12/11 16:16:18 [notice] 1#1: OS: Linux 5.14.0-427.13.1.el9_4.x86_64
2024/12/11 16:16:18 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2024/12/11 16:16:18 [notice] 1#1: start worker processes
2024/12/11 16:16:18 [notice] 1#1: start worker process 28

[hugo@TP1Linux ~]$ sudo ss -lnpt
State         Recv-Q        Send-Q                Local Address:Port                 Peer Address:Port        Process
LISTEN        0             511                         0.0.0.0:80                        0.0.0.0:*
 users:(("nginx",pid=14360,fd=6),("nginx",pid=14359,fd=6))
LISTEN        0             128                         0.0.0.0:22                        0.0.0.0:*
 users:(("sshd",pid=1733,fd=3))
LISTEN        0             4096                        0.0.0.0:9999                      0.0.0.0:*
 users:(("docker-proxy",pid=14404,fd=4))
LISTEN        0             511                            [::]:80                           [::]:*
 users:(("nginx",pid=14360,fd=7),("nginx",pid=14359,fd=7))
LISTEN        0             128                            [::]:22                           [::]:*
 users:(("sshd",pid=1733,fd=4))
LISTEN        0             4096                           [::]:9999                         [::]:*
 users:(("docker-proxy",pid=14409,fd=4))
``` 

🌞 On va ajouter un site Web au conteneur NGINX





🌞 Visitons

```
[hugo@TP1Linux nginx]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS
                 NAMES
55629afa119b   nginx     "/docker-entrypoint.…"   53 seconds ago   Up 53 seconds   80/tcp, 0.0.0.0:9999->8080/tcp, [::]:9999->8080/tcp   naughty_borg
```



🌞 Lance un conteneur Python, avec un shell

```
[hugo@TP1Linux nginx]$ docker run -it python bash
root@e387e5d51a26:/# pip install aiohttp
Collecting aiohttp
  Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (7.7 kB)
Collecting aiohappyeyeballs>=2.3.0 (from aiohttp)
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl.metadata (6.1 kB)
Collecting aiosignal>=1.1.2 (from aiohttp)
  Downloading aiosignal-1.3.1-py3-none-any.whl.metadata (4.0 kB)
Collecting attrs>=17.3.0 (from aiohttp)
  Downloading attrs-24.2.0-py3-none-any.whl.metadata (11 kB)
Collecting frozenlist>=1.1.1 (from aiohttp)
  Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (13 kB)
Collecting multidict<7.0,>=4.5 (from aiohttp)
  Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.0 kB)
Collecting propcache>=0.2.0 (from aiohttp)
  Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (9.2 kB)
Collecting yarl<2.0,>=1.17.0 (from aiohttp)
  Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (69 kB)
Collecting idna>=2.0 (from yarl<2.0,>=1.17.0->aiohttp)
  Downloading idna-3.10-py3-none-any.whl.metadata (10 kB)
Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 2.1 MB/s eta 0:00:00
Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl (14 kB)
Downloading aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Downloading attrs-24.2.0-py3-none-any.whl (63 kB)
Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (267 kB)
Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (131 kB)
Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (227 kB)
Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (339 kB)
Downloading idna-3.10-py3-none-any.whl (70 kB)
Installing collected packages: propcache, multidict, idna, frozenlist, attrs, aiohappyeyeballs, yarl, aiosignal, aiohttp
Successfully installed aiohappyeyeballs-2.4.4 aiohttp-3.11.10 aiosignal-1.3.1 attrs-24.2.0 frozenlist-1.5.0 idna-3.10 multidict-6.1.0 propcache-0.2.1 yarl-1.18.3
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
root@e387e5d51a26:/# pip install aioconsole
Collecting aioconsole
  Downloading aioconsole-0.8.1-py3-none-any.whl.metadata (46 kB)
Downloading aioconsole-0.8.1-py3-none-any.whl (43 kB)
Installing collected packages: aioconsole
Successfully installed aioconsole-0.8.1
```

🌞 Récupérez des images

