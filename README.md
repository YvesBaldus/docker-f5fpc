
# F5 VPN client

This Docker image provides the F5 VPN Client, which can be used without local installation.
The docker image allows two operating modes:

* Using F5 VPN client with local installation like a local installed VPN client.
* Use the running Docker container as router/gateway to avoid VPN split tunneling.
  In this mode, the only modification to your local system is a route for your VPN subnets to the running Docker container.


## Setup

* Install Docker. **Important:** Do **not** use the packages provided by your
  distribution packages sources.
  Use the officical resources from docker: https://docs.docker.com/engine/installation/.
* If you want to use the gateway mode: Install required python packages:
  ```
  pip install -r requirements.txt
  ```

### Mac

* If you want to use the gateway mode:
  For automatic route setup on Mac you need to install ```iproute2mac``` via homebrew.


## Start F5 VPN client

### VPN client mode

You don't need to clone this repository.
Just start the Docker container with the following command:
```
docker run --name f5fpc --net host -it --rm --privileged matthiaslohr/f5fpc /bin/bash
```

Start the VPN client and connect to your VPN server with
```
f5fpc -s
```

Status check:
```
f5fpc -i
```

Disconnect:
```
f5fpc -o
```


### Gateway mode

There's a wrapper script (```f5fpy-client.py```) which helps to set up
the docker container, the VPN connection and the desired network routes.
Therefore for this mode you need to clone this repository and ```cd``` to it.

Simply run:
```
./f5fpc-client.py <VPN_HOST> <USER>
```

Auto route setup for connecting to a VPN network which uses the 10.0.0.0/8 IP range (needs root add/remove routes):
```
sudo ./f5fpc-client.py <VPN_HOST> <USER> -n 10.0.0.0/8
```

For more information and options see
```
./f5fpc-client.py -h
```

