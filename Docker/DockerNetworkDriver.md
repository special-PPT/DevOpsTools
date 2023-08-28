# Docker Network Driver
## Bridge Network Driver
A Container will be attached to the default bridge network if no network type is specified. 

You can also create a separate bridge network for better isolation. 

    docker network create bridge_network

    docker run --name nginx_server -itd -p 8080:80 nginx

Access the nginx welcome page at `host_ip:8080`

## Host Network Driver
The Container will use the host's IP and port directly.

    docker run --name nginx_server -itd --network host nginx

Access the nginx welcome page at `host_ip`.
## IPvlan Network Driver
Using the ipvlan network driver, the container will have its own IP address.

First, determine the subnet and gateway of your host network, as well as the physical parent network interface connected to the internet.

If using a virtual machine, choose the Bridge Adapter before creating the network.

![virtual box network setting](resources/images/virtual%20box%20network%20setting.png)

Below is an example from my computer:

    docker network create -d ipvlan --subnet 192.168.1.0/24 --gateway 192.168.1.1 -o parent=enp0s3 ipvlan_network

    docker run --name nginx_server -itd --network ipvlan_network --ip 192.168.1.93 nginx

Access the nginx welcome page at `192.168.1.93`

## Macvlan Network Driver
By using the macvlan network driver, the container will have its own MAC address. 

Here is an example from my computer:

    docker network create -d macvlan --subnet 192.168.1.0/24 --gateway 192.168.1.1 -o parent=enp0s3 macvlan_network

    docker run --name nginx_server  -itd --network macvlan_network --ip 192.168.1.93 nginx


Note: Delete ipvlan network before creating a macvlan network.

References: [Docker networking is CRAZY!! (you NEED to learn it)](https://www.youtube.com/watch?v=bKFMS5C4CG0&t=1685s)