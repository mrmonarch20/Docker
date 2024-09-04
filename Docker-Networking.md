# Docker Commands and Network Example

This repository demonstrates some basic Docker commands and network management. The commands used in this example include running containers, inspecting container details, and managing Docker networks.

## Commands and Explanations

### 1. Run Containers

- `docker run -d --name login nginx:latest`
  - Runs a new container named `login` using the `nginx:latest` image in detached mode.

- `docker run -d --name logout nginx:latest`
  - Runs a new container named `logout` using the `nginx:latest` image in detached mode.

- `docker run -d --name finance --network=secure-network nginx:latest`
  - Runs a new container named `finance` using the `nginx:latest` image and attaches it to the `secure-network` network.

- `docker run -d --name host-demo --network=host nginx:latest`
  - Runs a new container named `host-demo` using the `nginx:latest` image and attaches it to the `host` network.

### 2. Execute Commands in Containers

- `docker exec -it login /bin/bash`
  - Opens a Bash shell inside the `login` container.

### 3. Inspect Containers and Networks

- `docker inspect login`
  - Shows detailed information about the `login` container.

- `docker inspect logout`
  - Shows detailed information about the `logout` container.

- `docker inspect finance`
  - Shows detailed information about the `finance` container.

- `docker inspect host-demo`
  - Shows detailed information about the `host-demo` container.

- `docker network ls`
  - Lists all Docker networks.

- `docker network create secure-network`
  - Creates a new Docker network named `secure-network`.

## Example Output

### Container IP Addresses

After creating and running containers, you can inspect their IP addresses using:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' login
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' logout
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' finance
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' host-demo

## To test connection between login and logout containers , we can use this command

docker exec -it login /bin/bash
ping 172.17.0.3

##output:
-PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
64 bytes from 172.17.0.3: icmp_seq=1 ttl=64 time=0.100 ms
64 bytes from 172.17.0.3: icmp_seq=2 ttl=64 time=0.090 ms
64 bytes from 172.17.0.3: icmp_seq=3 ttl=64 time=0.061 ms
64 bytes from 172.17.0.3: icmp_seq=4 ttl=64 time=0.059 ms
64 bytes from 172.17.0.3: icmp_seq=5 ttl=64 time=0.060 ms
64 bytes from 172.17.0.3: icmp_seq=6 ttl=64 time=0.059 ms
64 bytes from 172.17.0.3: icmp_seq=7 ttl=64 time=0.065 ms
64 bytes from 172.17.0.3: icmp_seq=8 ttl=64 time=0.080 ms
64 bytes from 172.17.0.3: icmp_seq=9 ttl=64 time=0.063 ms
64 bytes from 172.17.0.3: icmp_seq=10 ttl=64 time=0.060 ms
64 bytes from 172.17.0.3: icmp_seq=11 ttl=64 time=0.061 ms
64 bytes from 172.17.0.3: icmp_seq=12 ttl=64 time=0.094 ms

## For demo container created with host shows

- ubuntu@ip-172-31-93-67:~$ docker exec -it login /bin/bash
- root@71f04a4e2876:/# ping 172.18.0.2
- PING 172.18.0.2 (172.18.0.2) 56(84) bytes of data.
- exit
 - --- 172.18.0.2 ping statistics ---
- 35 packets transmitted, 0 received, 100% packet loss, time 34850ms

