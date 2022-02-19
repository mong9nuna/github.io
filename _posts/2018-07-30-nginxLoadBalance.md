---
layout: post
title: 'nginx에서 로드밸런싱 하기'
author : 효준
date: 2018-07-30 17:30
tags: [linux]
image: /files/covers/blog.jpg
---


# 로드밸런싱

    로드밸런싱이랑 쉽게말해서 서버에 큰 부담이 없도록
    클라이언트 요청을 여러개의 서버로 분산해주는 것이다.
    우선 여러대의 서버로 분산하기 위해선 똑같은 서비스를하는 서버를 여러대 준비해야한다.
    나의 경우는 EC2 무료서버가 버텨주지못해 2개까지밖에 못켰다.


# nginx 설정

    우선 /etc/nginx 폴더에서 nginx.conf 파일을 vi로 실행한다.
    그 후에
    upstream hjApp {
        least_conn; # 현재 가장 부하가 적은 서버로 보낸다.
                    # 만약 ip_hash; 를 적용하면 sticky session이 적용된다고 함. 한 ip는 그서버만이용

        server localhost:8099;
        server localhost:8090;
    }

    참고로 8099,8090 포트는 완전히 똑같은 수행을하는 서버이다.

    그다음 /etc/nginx/site-available 폴더에 들어가서

    server{
        ....
        location / { # /로 들어오는 요청을 hjApp으로 보낼것
            proxy_pass http://hyojunApp
        }
    }

    를 추가해준다.
    추가하게 되면 클라이언트의 요청은 두개의 서버로 분산이 되므로
    서버 입장에서는 현재 2개밖에 추가안했지만 많을 수록 더 서버의 부하가 줄어들게 된다.

