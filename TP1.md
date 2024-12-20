# TP1 : Premiers pas Docker

## I. Init


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


## II. Images

## 1. Images publiques





🌞 Récupérez des images

```
[hugo@TP1Linux ~]$ docker image ls
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
linuxserver/wikijs   latest    863e49d2e56c   6 days ago      465MB
python               3.13      3ca4060004b1   8 days ago      1.02GB
python               latest    3ca4060004b1   8 days ago      1.02GB
nginx                latest    66f8bdd3810c   2 weeks ago     192MB
wordpress            latest    c89b40a25cd1   2 weeks ago     700MB
mysql                5.7       5107333e08a8   12 months ago   501MB
```

🌞 Lancez un conteneur à partir de l'image Python

```
[hugo@TP1Linux ~]$ docker run -it python bash
root@72c26159c072:/#
```

## 2. Construire une image
🌞 Ecrire un Dockerfile pour une image qui héberge une application Python

```
[hugo@TP1Linux python_app_build]$ cat Dockerfile
FROM debian:latest

RUN apt update && apt install -y python3-pip python3

RUN apt install python3-emoji

COPY app.py /app.py

ENTRYPOINT ["python3", "/app.py"]
```

🌞 Build l'image

```
[hugo@TP1Linux python_app_build]$ docker build . -t python_app:version_de_ouf
[+] Building 9.5s (9/9) FINISHED                                                                         docker:default
 => [internal] load build definition from Dockerfile                                                               0.0s
 => => transferring dockerfile: 199B                                                                               0.0s
 => [internal] load metadata for docker.io/library/debian:latest                                                   0.7s
 => [internal] load .dockerignore                                                                                  0.0s
 => => transferring context: 2B                                                                                    0.0s
 => [1/4] FROM docker.io/library/debian:latest@sha256:17122fe3d66916e55c0cbd5bbf54bb3f87b3582f4d86a755a0fd3498d36  0.0s
 => [internal] load build context                                                                                  0.0s
 => => transferring context: 27B                                                                                   0.0s
 => CACHED [2/4] RUN apt update && apt install -y python3-pip python3                                              0.0s
 => [3/4] RUN apt install python3-emoji                                                                            2.2s
 => [4/4] COPY app.py /app.py                                                                                      0.3s
 => exporting to image                                                                                             5.8s
 => => exporting layers                                                                                            5.7s
 => => writing image sha256:5505477bfca888f63df2d97f1b3838506f8449a8fcbeb2177a92e5a0656d7f81                       0.0s
 => => naming to docker.io/library/python_app:version_de_ouf                                                       0.0s
```

🌞 Lancer l'image

```
[hugo@TP1Linux python_app_build]$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```
## III. Docker compose

🌞 Créez un fichier docker-compose.yml

```
[hugo@TP1Linux compose_test]$ cat docker-compose.yml
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

🌞 Lancez les deux conteneurs avec docker compose

```
[hugo@TP1Linux compose_test]$ docker compose up -d
WARN[0000] /home/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 3/3
 ✔ conteneur_nul Pulled                                                                                            2.8s
   ✔ fdf894e782a2 Already exists                                                                                   0.0s
 ✔ conteneur_flopesque Pulled                                                                                      3.1s
[+] Running 3/3
 ✔ Network compose_test_default                  Created                                                           0.3s
 ✔ Container compose_test-conteneur_nul-1        Started                                                           0.9s
 ✔ Container compose_test-conteneur_flopesque-1  Started                                                           0.9s
 ```





🌞 Vérifier que les deux conteneurs tournent

```
[hugo@TP1Linux compose_test]$ docker compose ps
WARN[0000] /home/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME                                 IMAGE     COMMAND        SERVICE               CREATED         STATUS         PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   4 minutes ago   Up 4 minutes
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         4 minutes ago   Up 4 minutes
```



 🌞 Pop un shell dans le conteneur conteneur_nul

```
[hugo@TP1Linux compose_test]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
62e3cf52967e   debian    "sleep 9999"   25 minutes ago   Up 25 minutes             compose_test-conteneur_nul-1
605b4298cc75   debian    "sleep 9999"   25 minutes ago   Up 25 minutes             compose_test-conteneur_flopesque-1
```

```
[hugo@TP1Linux compose_test]$ docker exec -it 62 bash
root@62e3cf52967e:/# apt-get update -y
Get:1 http://deb.debian.org/debian bookworm InRelease [151 kB]
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm/main amd64 Packages [8789 kB]
Get:5 http://deb.debian.org/debian bookworm-updates/main amd64 Packages [8856 B]
Get:6 http://deb.debian.org/debian-security bookworm-security/main amd64 Packages [216 kB]
Fetched 9268 kB in 2s (4908 kB/s)
Reading package lists... Done
root@62e3cf52967e:/# apt-get install -y iputils-ping
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcap2-bin libpam-cap
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 96.3 kB of archives.
After this operation, 312 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main amd64 libcap2-bin amd64 1:2.66-4 [34.7 kB]
Get:2 http://deb.debian.org/debian bookworm/main amd64 iputils-ping amd64 3:20221126-1+deb12u1 [47.2 kB]
Get:3 http://deb.debian.org/debian bookworm/main amd64 libpam-cap amd64 1:2.66-4 [14.5 kB]
Fetched 96.3 kB in 0s (1095 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 6089 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.66-4_amd64.deb ...
Unpacking libcap2-bin (1:2.66-4) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20221126-1+deb12u1_amd64.deb ...
Unpacking iputils-ping (3:20221126-1+deb12u1) ...
Selecting previously unselected package libpam-cap:amd64.
Preparing to unpack .../libpam-cap_1%3a2.66-4_amd64.deb ...
Unpacking libpam-cap:amd64 (1:2.66-4) ...
Setting up libcap2-bin (1:2.66-4) ...
Setting up libpam-cap:amd64 (1:2.66-4) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.36.0 /usr/local/share/perl/5.36.0 /usr/lib/x86_64-linux-gnu/perl5/5.36 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.36 /usr/share/perl/5.36 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20221126-1+deb12u1) ...
```



```
root@62e3cf52967e:/# ping compose_test-conteneur_flopesque-1
PING compose_test-conteneur_flopesque-1 (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.112 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=2 ttl=64 time=0.067 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=3 ttl=64 time=0.069 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=4 ttl=64 time=0.069 ms
^C
--- compose_test-conteneur_flopesque-1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3099ms
rtt min/avg/max/mdev = 0.067/0.079/0.112/0.018 ms
```