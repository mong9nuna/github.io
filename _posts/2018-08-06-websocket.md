---
layout: post
title: 'SpringBoot에서 웹소켓(Websocket) 구현하기'
author : 효준
date: 2018-08-06 11:30
tags: [spring]
image: /files/covers/blog.jpg
---

# 웹소켓이란?

    웹소켓은 서버와 클라이언트 간의 효율적인 양방향 통신을 실현하기 위한 구조이다.
    최근에는 Gmail처럼 데이터의 실시간 특성을 중시한 웹 어플리케이션이 많이 등장하여 주목받고있다.
    자바스크립트의 처리성능이 크게 개선된 현재, 웹 어플리케이션의 성능면에서 병목현상이 나타나는것은
    네트워크 통신부분으로 웹 소켓은 실시간 웹을 구현하기 위한 핵심 기술로 기대받고 있다.

    웹소켓은 매우 단순한 API로 구성되어있다. 웹소켓을 이용하면 하나의 HTTP접속으로 양방향 메시지를 자유롭게
    주고 받을수 있다.
    ajax나 Server-Sent Event를 조합해서 양방향 통신을 구현하는 경우와 비교해 통신 효율이 좋고, 설계나 구현도
    간단해지는 장점이 있다.

      - Http 프로토콜을 기반으로 웹 브라우저와 웹 서버간의 양방향 통신을 지원하기 위한 표준 API 이다.

      - 클라이언트와 서버가 서로 실시간으로 메세지를 자유롭게 주고 받을 수 있다.

      - 주로 채팅 서비스를 개발한다.

      - 실시간 통신을 위한 기술을 Web Socket, COMET and AJAX가 있다.

# 웹소켓이 필요한 다섯가지 경우
    1. 실시간 양방향 데이터 통신이 필요한 경우.

    2. 많은 수의 동시 접속자를 수용해야 하는 경우.

    3. 브라우저에서 TCP 기반의 통신으로 확장해야 하는 경우.

    4. 개발자에게 사용하기 쉬운 API가 필요할 경우.

    5. 클라우드 환경이나 웹을 넘어 SOA(Service Oriented Architecture) 로 확장해야 하는 경우


# 웹소켓 구현 (서버)
    compile('org.springframework.boot:spring-boot-starter-websocket')

    websocket에 대한 디펜던시 추가
========================================================================

    @Configuration
    @EnableWebSocketMessageBroker
    public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
        @Override
        public void registerStompEndpoints(StompEndpointRegistry registry) {
            registry.addEndpoint("/stomp").withSockJS();
        }

        @Override
        public void configureMessageBroker(MessageBrokerRegistry registry) {
            registry.enableSimpleBroker("/topic");
            registry.setApplicationDestinationPrefixes("/app");
        }
    }
    웹소켓에 대한 설정이다.

========================================================================

    @Component
    public class Producer {
        private static final SimpleDateFormat dateFormatter = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");

        @Autowired
        private SimpMessagingTemplate template;

        public void sendMessage(String topic, String message) {
            StringBuilder builder = new StringBuilder();
            builder.append("[");
            builder.append(dateFormatter.format(new Date()));
            builder.append("]");
            builder.append(message);

            this.template.convertAndSend("/topic/" + topic, builder.toString());

        }

    }

    /topic/으로 날짜와 메시지를 보내줄것.

========================================================================

    @RestController
    public class WebSocketController {
        @Autowired
        Producer producer;

        @RequestMapping(value = "/send/{topic}")
        public String sender(@PathVariable String topic, @RequestParam String message) {
            producer.sendMessage(topic, message);
            return "OK";
        }
    }



========================================================================
# 웹소켓 구현 (클라이언트)

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>스프링부트 웹소켓 메시징</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/sockjs-client/1.1.5/sockjs.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/stomp.js/2.3.3/stomp.min.js"></script>
    </head>
    <body>
        <div>
            <form id="frm" method="get" onsubmit="return false;">
                <h3>메시지 : <input id="message" type="text" name="message"></h3>
                <ol id="messages"></ol>
                <input type="button" id="submitBtn" value="메시지보내기">
            </form>
        </div>

    <script>
        $(document).ready(function () {
            var messageList = $("#messages");
            var socket = new SockJS('/stomp');
            var stompClient = Stomp.over(socket);
            stompClient.connect({}, function (frame) {
                stompClient.subscribe("/topic/message", function (data) {
                    var message = data.body;
                    messageList.append("<li>" + message +"</li>");
                    });
                });
            });
           $("#submitBtn").off().on('click', function () {
                $.ajax({
                    url : '/send/message?message='+$('#message').val(),
                    type : 'get'
                });
        });
    </script>
    </body>
    </html>

    메시지보내기 버튼을 클릭하면 ajax를 통해 /send/message로 보냄 (맞는지는 모르겠음)

