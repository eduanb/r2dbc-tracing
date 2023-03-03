This is an example repository for https://github.com/spring-projects/spring-boot/issues/34466
1. Run postgres:
`docker run --name postgres -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=mydb -d -p 5432:5432 postgres`
2. Run app:
`./gradlew bootRun`
3. See this exception on startup:

```
java.lang.UnsupportedOperationException: Although QueueSubscription extends Queue it is purely internal and only guarantees support for poll/clear/size/isEmpty. Instances shouldn't be used/exposed as Queue outside of Reactor operators.
    at reactor.core.publisher.ContextPropagation$ContextQueue.peek(ContextPropagation.java:376) ~[reactor-core-3.5.3.jar:3.5.3]
    at io.r2dbc.postgresql.client.ReactorNettyClient$BackendMessageSubscriber.onNext(ReactorNettyClient.java:811) ~[r2dbc-postgresql-1.0.1.RELEASE.jar:1.0.1.RELEASE]
    at io.r2dbc.postgresql.client.ReactorNettyClient$BackendMessageSubscriber.onNext(ReactorNettyClient.java:719) ~[r2dbc-postgresql-1.0.1.RELEASE.jar:1.0.1.RELEASE]
    at reactor.core.publisher.FluxHandle$HandleSubscriber.onNext(FluxHandle.java:128) ~[reactor-core-3.5.3.jar:3.5.3]
    at reactor.core.publisher.FluxPeekFuseable$PeekConditionalSubscriber.onNext(FluxPeekFuseable.java:854) ~[reactor-core-3.5.3.jar:3.5.3]
    at reactor.core.publisher.FluxMap$MapConditionalSubscriber.onNext(FluxMap.java:224) ~[reactor-core-3.5.3.jar:3.5.3]
    at reactor.core.publisher.FluxMap$MapConditionalSubscriber.onNext(FluxMap.java:224) ~[reactor-core-3.5.3.jar:3.5.3]
    at reactor.netty.channel.FluxReceive.drainReceiver(FluxReceive.java:294) ~[reactor-netty-core-1.1.3.jar:1.1.3]
    at reactor.netty.channel.FluxReceive.onInboundNext(FluxReceive.java:403) ~[reactor-netty-core-1.1.3.jar:1.1.3]
    at reactor.netty.channel.ChannelOperations.onInboundNext(ChannelOperations.java:411) ~[reactor-netty-core-1.1.3.jar:1.1.3]
    at reactor.netty.channel.ChannelOperationsHandler.channelRead(ChannelOperationsHandler.java:113) ~[reactor-netty-core-1.1.3.jar:1.1.3]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:444) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:412) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.handler.codec.ByteToMessageDecoder.fireChannelRead(ByteToMessageDecoder.java:346) ~[netty-codec-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.handler.codec.ByteToMessageDecoder.channelRead(ByteToMessageDecoder.java:318) ~[netty-codec-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:444) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:412) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1410) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:440) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:420) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:919) ~[netty-transport-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.epoll.AbstractEpollStreamChannel$EpollStreamUnsafe.epollInReady(AbstractEpollStreamChannel.java:800) ~[netty-transport-classes-epoll-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.epoll.EpollEventLoop.processReady(EpollEventLoop.java:499) ~[netty-transport-classes-epoll-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.channel.epoll.EpollEventLoop.run(EpollEventLoop.java:397) ~[netty-transport-classes-epoll-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:997) ~[netty-common-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74) ~[netty-common-4.1.89.Final.jar:4.1.89.Final]
    at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30) ~[netty-common-4.1.89.Final.jar:4.1.89.Final]
    at java.base/java.lang.Thread.run(Thread.java:833) ~[na:na]
```

4. Remove `Hooks.enableAutomaticContextPropagation()` in main and the exception goes away
