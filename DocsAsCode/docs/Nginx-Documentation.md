# Setting up Nginx Load Balancer

## Set up my react project running in an docker File 

### Docker file 

```DockerFile
FROM ubuntu:20.04
#FROM python_docker
ENV TZ=Australia/Sydney
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y curl

RUN curl -sL https://deb.nodesource.com/setup_16.x -o /tmp/nodesource_setup.sh

RUN bash /tmp/nodesource_setup.sh

RUN apt-get install -y nodejs
RUN node -v
RUN npm -v

WORKDIR /usr/src/app/

COPY package.json /usr/src/app/

RUN npm install --force

COPY . /usr/src/app/

EXPOSE 3000

CMD ["npm", "start"]

```

Building Docker image fomr the dockerfile
```bash
sudo docker build --no-cache -f /home/kumaraupadhayayula/Desktop/final-portfolio-2-master/DockerFile .
```

### Running Docker form docker file 


```bash
sudo docker run -p 30001:3000 node_ubuntu_docker
```

```bash
sudo docker run -p 3002:3000 node_ubuntu_docker
```

Both the docker are running in the root bridgew network:-

They can communictae with each other and the host
No DNS in the containers

In this case the the docker images are running in this network 


https://172.17.0.2/

https://172.17.0.3/

### Create nginx Instance and replace the conf files

- Install nginx
    ```bash
    sudo apt install nginx
    ```

- The location of the Nginx Config is 

    ```bash
    /etc/nginx/nginx.conf
    ```
- The Nginx config file is 
    ```nginx
    http {

    upstream backendnnserver {
        server 127.0.0.1:30001;
        server 127.0.0.1:3002;
    }

    server {
        listen 8080;

        location / {
            proxy_pass https://backendnnserver;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
    }

    events{}
    ```


    - [ X ] Getting started wityh Docker 

        - [ X ] Pull Image from docker
        - [ X ]
