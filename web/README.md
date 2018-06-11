# Vistio Web [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[nmnellis/vistio/web](https://github.com/nmnellis/vistio/web) web front-end application  

This fork of Netflix's [vizceral-example](https://github.com/Netflix/vizceral-example) contains these new features:
* Replaying
* Connection Chart
* Node Coloring
* Class Filtering
* Notice Filtering

...

# Install
This application is one of promviz's components.  
To install, please refer to [nmnellis/vistio/web#install](https://github.com/nmnellis/vistio/web#install).  

# Install & Run Independently
```
npm install
npm run dev
```
or, you can use Docker to run:
```
docker build -t <name>/vistio .
docker run -p 8080:8080 -d <name>/vistio
```
then, you can view the top page at http://localhost:8080/

### Public Docker Repository
[nmnellis/vistio-web](https://hub.docker.com/r/nmnellis/vistio-web/)

# Configuration
There are 2 ways to configure this application:  
1. edit .env file
1. set environment variables

You can customize this application's behavior with these variables:  
```
UPDATE_URL: endpoint of promviz server  
INTERVAL: interval between fetches (ms)  
MAX_REPLAY_OFFSET: limit of replaying offset (s)  
```

# Contributing
Welcome PRs!
