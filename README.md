# Transmission<p>

### Goal<p>
Using Restful APIs to control transmission downloading files.<p>

### Prerequisite<p>
+ Ubuntu 16.04 64bit<p>
+ Windows Ultimate 64bit<p>
+ Docker Engine<p>
+ Postman<p>

### Related Reference<p>
[Official Website](https://www.transmissionbt.com/)<p>
[Docker Hub Image](https://hub.docker.com/r/dperson/transmission/)<p>
[Docker API](https://docs.docker.com/engine/reference/api/docker_remote_api_v1.23)<p>

### Procedure<p>
+ **Deploy Docker Daemon**<P>
  ```
  service docker stop
  docker daemon -H tcp://127.0.0.1:2375
  ```

+ **Deploy Docker Image**<P>
  - Use Postman to pull transmission image from docker hub<p>
  `POST http://127.0.0.1:2375/images/create?fromImage=dperson/transmission`<p>
  - or Use Curl<p>
  `curl -d "fromImage=dperson/transmission" http://192.168.5.105:2375/images/create`<p>

+ **Use Transimission with docker api**<P>
  - Create a container<p>
  `POST http://127.0.0.1:2375/containers/create`<p>
  `Postman -> Body -> raw`<p>
    ```
    {"Image":"7c5fe3eafa3f","ExposedPorts":{"9091/tcp":{}},"HostConfig":{"Binds":["/your/path/Downloads/:/var/lib/transmission-daemon/downloads"]}}
    ```
    
    PS: **7c5fe3eafa3f** is the ID of transmission's docker image. It will return a new ID for container's ID.<p>
    ```
    {
      "Id": "05fc8e5fc6f00622fd723322ec3bf0494fe6bb55aa6c602933d1e64d56e0b150",
      "Warnings": null
    }
    ```
  - or Use Curl to create a container<p>
    ```
    nano hehe.txt 
    {"Image":"7c5fe3eafa3f","ExposedPorts":{"9091/tcp":{}},"HostConfig":{"Binds":["/your/path/Downloads/:/var/lib/transmission-daemon/downloads"]}}
    ```
    `curl -X POST --header "Content-Type:application/json" http://192.168.5.105:2375/containers/create --data @hehe.txt`<p>
  
  - Start a container<p>
  `POST http://127.0.0.1:2375/containers/05fc8e5fc6f00622fd723322ec3bf0494fe6bb55aa6c602933d1e64d56e0b150/start`<p>
  `Postman -> Body -> raw`<p>
    ```
    {"PortBindings":{"9091/tcp":[{"HostIp":"127.0.0.1","HostPort":"9091"}]}}
    ```
    
  - or Use Curl
    ```
    nano haha.txt
    {"PortBindings":{"9091/tcp":[{"HostIp":"127.0.0.1","HostPort":"9091"}]}}
    ```
    `curl -X POST --header "Content-Type:application/json" http://192.168.5.105:2375/containers/05fc8e5fc6f00622fd723322ec3bf0494fe6bb55aa6c602933d1e64d56e0b150/start --data @haha.txt`<p>
    
  - Use Transmission with browser<p>
  `http://192.168.5.105:9091/`<p>

  `PS: The default username/password are admin/admin`<p>

### Done
