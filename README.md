<h1 align="center">ShortURL: A URL Shortening service 🔗</h1>
<p align = center>
    <img alt="Project Logo" src="https://raw.githubusercontent.com/Abhinav-CHD/URL-Shortener/master/client/src/assets/images/shorurl.jpg" target="_blank" />
</p>
<h2 align='center'>A distributed and highly available URL Shortener.</h2><br/>

## 📚 | Introduction

- ShortURL is a distributed and highly available URL Shortening service, built on the MERN stack.
- It uses Redis as a cache, and MongoDB as a NoSQL database.
- It uses Nginx that acts as a load balancer, and a reverse proxy for the backend server.
- It uses Apache ZooKeeper to provide tokens for hash generation, and eliminates race conditions between nodes.
- The entire application is containerized by Docker, and orchestrated by k8s.

<br/>

## 🚀 | Usage

- Install Docker Desktop and enable Kubernetes for a quick setup.
- Clone this repository:<br>

```sh
git clone https://github.com/Abhinav-CHD/URL-Shortener.git
```

- Create a .env file with the following variables:

```js
REACT_APP_MONGODB_URI="<enter your MongoDB URI>"
REACT_APP_REDIS_PORT=1111 (defaults to 6379)
REACT_APP_REDIS_HOST='redis-server' (name it as you like, but change the same in the docker-compose.yml file)
```

- Open the project folder and start the container with docker compose:<br>

```yml
docker compose up --build

# We can also scale instances of an image for higher scalability or distribution.
Example: docker compose up --build --scale node-server=3
```

- Enjoy the project! 😉

<br/>

## 🌐 | API Endpoints

```yml
GET:
    /url/:identifier : Get the shortened URL from DB
    /del : Delete the Zookeeper token
POST:
    /url [body : {"OriginalUrl" : "url"}] : Shorten the URL and store in DB


Access the API using the following URLs:
    Client: http://localhost:3000/
    Load Balanced Server: http://localhost:4000/
```

## 📺 | Demonstration

<p align = center>
    
![trim](https://user-images.githubusercontent.com/50882624/154680953-a41c84e3-6512-4fdf-8b3c-b8856f3c5842.gif)

</p>

<br/>

## 📘 | System Design Schematic

<p align = center>
    <img alt="getURL" src="https://raw.githubusercontent.com/Abhinav-CHD/URL-Shortener/master/client/src/assets/images/getURLs.png" target="_blank" />
    <img alt="redirect" src="https://raw.githubusercontent.com/Abhinav-CHD/URL-Shortener/master/client/src/assets/images/redirect.png" target="_blank" />
</p>

<br/>

## ⌛ | Architectural Discussion

- The _**availability**_ of the application can be improved by using multiple Zookeeper instances, replicas of the DB, and a distributed cache, thus increasing the fault tolerance of the architecture.
- Adding load balancers in between the following improves the performance of the application, and reduces the load on any particular instance:
  - client and server
  - server and DB
  - server and cache
- _**CAP Theorem**_:
  - We opt for an _**eventually consistent approach**_, as in case of a network partition, a URL Shortener should have low latency and high throughput at all times. <br/>
  - Redirection of the user to the original URL should always have low latency as it directly impacts the business aspect of the application.
  - We don't opt for a _**strongly consistent approach**_, as we would have to wait for the data to be replicated across the cluster, which decreases the availability and increases the latency of the application, thus impacting the user experience negatively.

<br/>
