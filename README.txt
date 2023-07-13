Steps performed
===============
Step 1 - Create a new spring boot project using spring initializr(3 dependencies: web, websocket, lombok)
Step 2 - Open project in an IDE(Intellij idea in this case)
Step 3 - Create a new configuration class for websocket in a new directory('config.WebsocketConfig.java')
            add the following class level annotations:  @Configuration, @EnableWebSocketMessageBroker
            implement the following class: WebSocketMessageBrokerConfigurer,
            override the following methods:
                1- registerStombEndpoints(StombEndpointRegistry registry){}
                    @Override
                    public void registerStompEndpoints(StompEndpointRegistry registry) {
                        registry.addEndpoint("ws").withSockJS();
                    }

                2- configureMessageBrocker(MessageBrockerRegistry registry){}
                    @Override
                    public void configureMessageBroker(MessageBrokerRegistry registry) {
                        registry.setApplicationDestinationPrefixes("/app");
                        registry.enableSimpleBroker("/topic");
                    }

Step 4 - Create another configuration for listener(for session disconnect listener): config.WebSocketEventListener.java
            add class level annotations: @Component, @RequiredArgsConstructor, @Slf4j
            create a method handleWebSocketDisconnetListener(SessionDisconnectEvent event) {}
            to make this method an event listener, add @EventListener annotation for the above method
Step 5 - Create an Enum: 'MessageType' with 3 values: CHAT, JOIN, LEAVER
Step 6 - Create a class for message content: chat.ChatMessage.java.
            add class-level annotations: @Getter, @Setter, @NoArgsConstructor, @AllArgsConstructor, @Builder
            add properties such as content(string), sender(string) and messageType(MessageType)
Step 7 - Create the controller class: ChatController.java
            add class level annotations: @Controller
            create handler method: public ChatMessage sendMessage(@Payload ChatMessage chatMessage) {}
                add following annotation for the above method:
                    @MessageMapping("/chat.sendMessage") - For mapping
                    @SentTo("topic/public")  - To which queue/topic we need to send the message
            Create another handler method: public ChatMessage addUser(@Payload ChatMessage chatMessage, SimpMessageHeaderAccessor headerAccessor) {}
                In case a user joins, this endpoint allow us to establish a connection b/w the user and websocket
                add following annotation for the above method:
                    @MessageMapping("/chat.addUser") - For mapping
                    @SentTo("topic/public")  - To which queue/topic we need to send the message


                    --- Backend Logic is Done
                    --- Now implement the front end part
Step 8 - Create a file index.html inside the folder 'resources/static'
Step 9 - In order to use websocket inside javascript, we need to import following Javascript files:
            sockjs.min.js - <script src="https://cdnjs.cloudflare.com/ajax/libs/sockjs-client/1.6.1/sockjs.min.js" integrity="sha512-1QvjE7BtotQjkq8PxLeF6P46gEpBRXuskzIVgjFpekzFVF4yjRgrQvTG1MTOJ3yQgvTteKAcO7DSZI92+u/yZw==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
            stomp.min.js.js - <script src="https://cdnjs.cloudflare.com/ajax/libs/stomp.js/2.3.3/stomp.min.js" integrity="sha512-iKDtgDyTHjAitUDdLljGhenhPwrbBfqTKWO1mkhSFH3A7blITC9MhYon6SjnMhp4o0rADGw9yAC6EW4t5a4K3g==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
            main.js(internal) - <script src="/static/js/main.js"></script>
Step 10 - Create a css file (main.css) and import the same into index.html


