# 使用阿里云进行内网数据转发
*author：LZN*  

### 工具：frp

frp下载地址：https://github.com/fatedier/frp/releases

### 使用：frp分为server端和client端。

   阿里云服务器运行server端：

            `./frps -c frps.ini`
      
   使用如下命令在后台运行：
        
            nohup ./frps -c frps.ini &

   实验室内网主机运行client端:
            
            ./frpc -c frpc.ini

注意frp的server端与client端的版本一致性。
        
### 运行前配置：

   #### 阿里云服务器frp配置：  
   配置文件为 frps.ini  
     默认文件内容为：  

            [common]
            bind_port = 9001
        
   当前端口号为9001，也可以修改bind_port为你想要的端口号。该端口号用于frp的server与client的连接，即：阿里云服务器与实验室内网主机的连接。frp的server端将一直监听9001端口。

   #### 内网主机（实验室主机）配置：  
   配置文件为 frpc.ini  
   文件内容：
            [common]
            server_addr = x.x.x.x
            server_port = 9001

            [carInfo]
            type=tcp
            local_ip=127.0.0.1
            local_port = 9999
            remote_port = 6000
            
   x.x.x.x为frp的server端IP地址，即阿里云服务器ip  
   server_port为frp的server（阿里云）的端口号，应与server端（阿里云）保持一致。  
   [carInfo]为新建的自定义转发目标。type为协议格式，当前为tcp。remote_port=6000 定义的端口号目的是：告诉frp server收到6000端口的服务请求要转发给我。  
   local_ip和local_port定义的是本机的用于处理接收请求的应用位置。具体设置要遵循具体应用的配置。
    
