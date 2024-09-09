## Docker Commands and Network Example (Simple Explanation)

This repository explains basic Docker commands and how to use Docker networks. Docker helps us run small programs (called "containers") that can talk to each other or stay isolated, just like different classrooms in a school where students work on different tasks.

---

## Simple Examples of Docker Commands

### 1. Running Containers (Starting Programs)

Think of containers as rooms where different activities happen. We can create these "rooms" (containers) and put "people" (programs) in them.

- `docker run -d --name login nginx:latest`
  - Imagine you’re starting a classroom called `login`, and inside this room, a web server is running. This command creates the room (container) and starts the web server using the `nginx` program.

- `docker run -d --name logout nginx:latest`
  - Now, we create another room called `logout` where another web server runs the same `nginx` program. This way, we have two rooms doing similar tasks but separately.

### 2. Connecting to a Room and Running Commands

You can go inside a room and look around, just like visiting a classroom.

- `docker exec -it login /bin/bash`
  - Imagine you are stepping into the `login` room and opening a terminal to talk to the program running inside. In this case, it's like opening a window and asking the web server what's happening.

### 3. Inspecting Containers (Looking Inside the Room)

Want to know more details about what’s happening in each room? We can "inspect" or check the container.

- `docker inspect login`
  - This command shows you detailed information about the `login` room: what it's running, what its IP address is (like its phone number), and more.

- `docker inspect logout`
  - Same thing for the `logout` room—inspect it to see what’s happening inside.

### 4. Networks (How Rooms Communicate)

In Docker, we can create "networks" to connect containers so they can talk to each other, or we can keep them separate. It’s like deciding whether classrooms in a school can send messages to each other or not.

- `docker network create secure-network`
  - Imagine we create a new "school hallway" called `secure-network`. This hallway allows containers (rooms) to communicate with each other.

- `docker run -d --name finance --network=secure-network nginx:latest`
  - Now we create another room called `finance` and place it in the `secure-network` hallway so it can talk to other rooms in that hallway.

- `docker run -d --name demo --network=demo-bridge nginx:latest`
  - Another room named `demo` is created, but it’s in a different hallway called `demo-bridge`. This network helps keep the `demo` container separate from other containers unless we want them to talk.

---

## Testing Communication Between Rooms (Containers)

Let’s see if the `login` room can talk to the `logout` room.

- `docker exec -it login /bin/bash`
  - Go inside the `login` room.

- `ping 172.17.0.3`
  - Imagine you send a "ping" (like knocking on the door) to the `logout` room’s IP address (its phone number). If the `login` room gets a response, we know that the two rooms can communicate.

**Output Example:**

```bash
PING 172.17.0.3 (172.17.0.3) 56(84) bytes of data.
64 bytes from 172.17.0.3: icmp_seq=1 ttl=64 time=0.100 ms


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

#Custom Networks (Creating Our Own Hallways)
--We can create special networks (hallways) where only specific containers (rooms) can communicate.

-docker network create --driver bridge demo-bridge

##Imagine we build a new hallway called demo-bridge. This hallway is only for certain rooms to use.
--docker run -d --name demo --network=demo-bridge nginx:latest

-We put the demo room in this hallway so it can only talk to other rooms in the demo-bridge hallway.
-Host Network Example (Using the Main School Hallway)
-Sometimes, we want to give a container direct access to the main school hallway (the host network), meaning it can talk to everyone in the school directly.

--docker run -d --name host-demo --network=host nginx:latest
-This command runs the host-demo container and attaches it to the main hallway (the host network). It can now communicate with any room in the school without restrictions.
#Testing if Rooms Can’t Communicate
#Let’s test a situation where two rooms are isolated and can't communicate.

--docker exec -it host-demo /bin/bash

--Step into the host-demo room.
--ping 172.18.0.2

##Try knocking on the door of the demo room that’s in the demo-bridge hallway. You might not get a response because the host-demo room is in a different hallway (network).
--Output Example:

--- 172.18.0.2 ping statistics ---
35 packets transmitted, 0 received, 100% packet loss
This shows that the host-demo room couldn’t talk to the demo room, meaning they are in separate networks and can't communicate.

-Summary
-Containers are like rooms in a school.
-Networks are like hallways where rooms can communicate.
-We can control which rooms (containers) can talk to each other by putting them in the same hallway (network).
-Commands like docker run, docker exec, and docker network let us create rooms, connect to them, and manage their communication.
