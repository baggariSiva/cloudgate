# ReactJs example demo CI/CD pipeline


<a href="https://dash.elest.io/deploy?source=cicd&social=Github&url=https://github.com/elestio-examples/cloudgate"><img src="public\images\deploy-on-elestio.png" alt="Deploy on Elest.io" width="180px" /></a>

Example application and CI/CD pipeline showing how to deploy a Cloudgate app on elestio.


## CI/CD on Elestio

Fork this repository to create your own copy that you can modify and use in a CI/CD pipeline


# Steps to create CI/CD pipeline on elestio

### Step 1: Select CI/CD from left sidebar in app.

Click [here](https://dash.elest.io/deploy?source=cicd) to directly go to the CI/CD

### Step 2: Select Deployment method.

We have three different types of deployment method

- Github
- Gitlab
- Docker compose

But for this ReactJs website example, you can choose GitHub as your deployment method.

### Step 3: Authentication

If you forked the repo then you can click on the sign-in with GitHub button and authorize elestio to access the git repo then you can select the reactjs-example repo otherwise you can directly insert a git repo URL to deploy the ReactJs website.

### Step 4: Configuration

After selecting a repo or inserting a URL it will auto-filled all the desired configurations using the elestio.yml/elestio.json file.

You can also manually customize the Configure your application. 

Select your runtime and its version, run & build commands.

Reverse proxy configuration, Volume Configuration, Exposed Ports Configuration and Environment variables.

### Step 5: Choose Deployment Targets

Elestio provides two different types of deployment targets.

- New Infrastructure
- Existing Infrastructures

On elestio single CI/CD target you can deploy multiple CI/CD pipelines so, If you already have CI/CD target on elestio then you can deploy a new pipeline on the same existing CI/CD target by choosing **Existing Infrastructures** and then select the CI/CD target otherwise if you don't have anything or want to deploy on new target then you can choose **New Infrastructure**

If you choose **New Infrastructure** then you have to select the deployment mode we have two different types of deployment modes.

- Single-mode.
- Cluster mode.

  **NOTE:-** Steps 6,7,8 and 9 are only for New Infrastructure targets for Existing Infrastructures targets directly following the final step.

### Step 6: Select Service Cloud Provider

Elestio supports five different types of cloud service providers you can choose anyone to deploy your service.

- Hetzner Cloud.
- Digital Ocean.
- Amazon Lightsail.
- Linode.
- Vultr.

We also provide a BYOVM service option so if you already have your VM on any third-party provider (Azure, GCP, Alibaba, ...) then you can choose BYOVM to deploy CI/CD pipeline on your VM.

Elestio provides one BYOVM service for free. To be eligible the VM you connect must have no more than 2 vCPU, max 4 GB of ram, and max 80 GB of storage

### Step 7: Select Service Plan

This step is only for other than BYOVM service providers.

We're providing multiple different types of service plans as per proposing by your selected providers.

### Step 8: Provide Service Name

By default, we create a unique target name for you but you can customize it.

### Step Final: Create Ci/CD pipeline

Now after following all the above steps you can click on the button **Create Ci/CD pipeline**.

It will take a few seconds to deploy your pipeline on elestio.

For each pipeline deployed on elestio will create a cname for it. but if you want your custom domain then you can configure it inside the target details.


&nbsp;

![Cloudgate logo](public/cloudgate128.png)

Cloudgate is useful if you need a Node.js Application server able to:
- Serving static files, REST APIs, Websockets ... all in one
- Full control over the whole pipeline with no middle man
- Multi-threaded with inter-thread communication & shared memory
- Crazy fast (check benchmarks below)

&nbsp;

# Cloudgate Quickstart

## Install Node.js 16 + GIT

    sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
    curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
    sudo apt -y install nodejs git

## Create a cloudgate app with a simple command:

    npx create-cloudgate-app my-app-1

This will create a full stack Node.js application supporting static files, REST API & Websockets endpoints.

OR

## Clone this repo & install dependencies

    git clone https://github.com/elestio/cloudgate-app.git
    cd cloudgate-app;
    npm install;

&nbsp;
# Getting started 

- Add your static files in `/public/` folder
- Add or duplicate an api function in `/api/` folder or subfolders
- Reference your new functions in `/apiconfig.json`

Sample basic API function (can be found in `/api/tests/simple.js`):

    module.exports = async (event) => {
        var myContent = "Hello World! " + (new Date().getTime())
        return {  
            httpStatus: "200", 
            headers: {'content-type': 'text/html'}, 
            content: myContent 
        };
    };

REST API & Websocket functions receive an event object, here is an example:

    {
        "method": "GET",
        "path": "/api/tests/simple",
        "ip": "XXX.XXX.XXX.XXX",
        "query": "myParam1=123",
        "queryStringParameters": {
            "myParam1": "123"
        },
        "headers": {
            "host": "localhost:3000",
            "connection": "keep-alive",
            "cache-control": "max-age=0",
            "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36",
            "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
            "accept-encoding": "gzip, deflate",
            "accept-language": "fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7",
            "cookie": "myCookie1=XXXXXXXXXXXXX;"
        },
        "GET": {
            "myParam1": "123"
        },
        "body": null
        "FILES": []
    }

&nbsp;

# Run

## Run directly on port 3000

    PORT=3000 node index.js


## Run on port 443 with SSL
    
    SSL=1 SSL_PORT=443 SSL_CERT=/path/to/cert.cer SSL_KEY=/path/to/cert.key node index.js

&nbsp;

## Run with docker
Run just once

    docker build -t elestio/cloudgate-app .
    docker run --rm -p 80:3000 -it elestio/cloudgate-app

Run as a docker service

    systemctl enable docker; #enable docker at boot
    docker run -p 80:3000 -it -d --name cloudgate-app --restart always elestio/cloudgate-app;


&nbsp;
# Benchmarks

I used WRK for benchmarks, anything else will be too slow and will hit the limits of the benchmarking tool rather than cloudgate limits.
More details about the great WRK: https://github.com/wg/wrk

You can run the whole benchmark suit on your computer like this:

    ./benchmarks.sh


# Results
My test configuration for the tests results below: AMD Ryzen 5 3600, 6 cores

## Serving Static HTML file

    > wrk -t6 -c256 http://127.0.0.1:3000/ --latency
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:3000/
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.06ms    1.89ms  27.98ms   89.41%
        Req/Sec    88.16k    19.46k  126.65k    55.00%
    Latency Distribution
        50%  255.00us
        75%    1.02ms
        90%    3.09ms
        99%    9.23ms
    5273106 requests in 10.05s, 3.18GB read
    Requests/sec: 524742.35
    Transfer/sec:    324.28MB


I runned the exact same test (same static html file) on nginx to get a point of comparison
nginx is running on port 8080 on my server

    > wrk -t6 -c256 http://127.0.0.1:8080/icg.html
    --------------------------------------------        
    Running 10s test @ http://127.0.0.1:8080/icg.html
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.74ms    3.07ms 208.07ms   89.85%
        Req/Sec    47.97k     9.38k   91.66k    69.83%
    2867104 requests in 10.07s, 2.67GB read
    Requests/sec: 284788.93
    Transfer/sec:    271.31MB


## Serving Static PNG file

    > wrk -t6 -c256 http://127.0.0.1:3000/cloudgate128.png --latency
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:3000/cloudgate128.png
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.49ms    2.58ms  32.23ms   87.76%
        Req/Sec    65.91k    15.63k   95.35k    62.10%
    Latency Distribution
        50%  341.00us
        75%    1.37ms
        90%    4.83ms
        99%   11.95ms
    3939181 requests in 10.08s, 56.74GB read
    Requests/sec: 390743.74
    Transfer/sec:      5.63GB

I runned the exact same test (same static png file) on nginx to get a point of comparison
nginx is running on port 8080 on my server

    > wrk -t6 -c256 http://127.0.0.1:8080/cloudgate128.png
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:8080/cloudgate128.png
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.80ms    3.11ms 207.78ms   89.77%
        Req/Sec    44.71k     9.14k   73.43k    69.00%
    2675720 requests in 10.05s, 38.65GB read
    Requests/sec: 266202.28
    Transfer/sec:      3.85GB

&nbsp;
## REST API: Hello World

    > wrk -t6 -c256 http://127.0.0.1:3000/api/tests/simple --latency
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:3000/api/tests/simple
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.45ms    2.57ms  44.15ms   88.06%
        Req/Sec    74.82k    19.55k  120.61k    65.17%
    Latency Distribution
        50%  292.00us
        75%    1.43ms
        90%    4.63ms
        99%   11.90ms
    4489094 requests in 10.09s, 3.16GB read
    Requests/sec: 444811.04
    Transfer/sec:    320.27MB      


&nbsp;
## REST API: read headers, method, querystring, posted data, source ip, increment shared memory, ...

    > wrk -t6 -c256 http://127.0.0.1:3000/api/tests/full --latency
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:3000/api/tests/full
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.02ms  571.05us  19.19ms   82.27%
        Req/Sec    41.70k     2.75k   68.99k    76.17%
    Latency Distribution
        50%    0.92ms
        75%    1.24ms
        90%    1.63ms
        99%    2.69ms
    2489466 requests in 10.04s, 2.45GB read
    Requests/sec: 248071.03
    Transfer/sec:    249.54MB

&nbsp;
## REST API: read file on disk, replace variables, return generated page

    > wrk -t6 -c256 http://127.0.0.1:3000/api/tests/template --latency
    --------------------------------------------
    Running 10s test @ http://127.0.0.1:3000/api/tests/template
    6 threads and 256 connections
    Thread Stats   Avg      Stdev     Max   +/- Stdev
        Latency     1.52ms    2.00ms  28.25ms   89.05%
        Req/Sec    38.81k     5.42k   63.84k    73.12%
    Latency Distribution
        50%  778.00us
        75%    1.70ms
        90%    3.73ms
        99%   10.01ms
    2326046 requests in 10.09s, 3.18GB read
    Requests/sec: 230525.88
    Transfer/sec:    322.94MB    