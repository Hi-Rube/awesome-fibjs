##一个基于 fibjs 的RTMP服务端
其实是2个，一个基于fibjs，一个基于nodejs      

两个版本大部分是一样的，只是在数据处理方面有差异。

fibjs版本：https://github.com/illuspas/NodeMediaServer
fibjs没有回调一说，同步的流程写起来相当舒服。

nodejs版：https://github.com/illuspas/Node-Media-Server     
nodejs的数据是on(‘data’)回调回来的，解析rtmp包很费劲，需要根据包头一步一步分析出需要的包大小。为此写了个QueueBuffer类，请求的数据大小足够即返回，不够就回压再等待数据下次继续解析。

目前只支持了H264+AAC，支持多路发布和播放。     
fibjs版目前在大并发的时候会阻塞发布端，写得有点问题，空了再改改。      
没有缓冲关键帧，播放的启动时间可能会等下一个关键帧来了才开始

此项目仅为空闲时写着玩的，参照了很多别人的代码，并不会长期维护。如果你感兴趣，欢迎fork。

转自：http://bashell.sinaapp.com/archives/open-source-javascript-rtmp-server.html