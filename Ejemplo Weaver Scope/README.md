Before You Begin
If you are using OS X or Windows, you can install Docker Toolbox, which provides all of the tools you need to complete this guide.

For other operating systems, please install and configure the following separately before proceeding:

docker-machine binary (>= 0.2.0)
docker binary, at least the client (>= v1.6.x)
VirtualBox (>= 4.3.x)
curl (any version)
Setting up the VM
First, install Linux onto a VM using virtual toolbox by typing:

docker-machine create -d virtualbox weave-scope-demo
SSH onto the newly created VM and then proceed to the next section:

docker-machine ssh weave-scope-demo
Deploying the Sample Application
To demonstrate Weave Scope in stand-alone mode, you will deploy an example application using Docker Compose. This example uses a single host, but keep in mind that Weave Scope works across on multiple hosts, or even across data centers and cloud providers.

Install Docker and Docker Compose onto the VM by running:

$ wget -qO- https://get.docker.com/ | sh
$ sudo curl -L https://github.com/docker/compose/releases/download/1.5.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
And next, use Docker Compose to launch all of the components of the sample application:

$ wget -O docker-compose.yml http://git.io/scope-compose
$ docker-compose up -d
Check that all seven application containers are running by typing docker ps:

CONTAINER ID  IMAGE            PORTS                 NAMES
fe41c10a63ca  tns_lb1:latest   0.0.0.0:8001->80/tcp  tns_lb1_1
c94005d87115  tns_lb2:latest   0.0.0.0:8002->80/tcp  tns_lb2_1
8c15a1325094  tns_app1:latest  8080/tcp              tns_app1_1
645386356a2e  tns_app2:latest  8080/tcp              tns_app2_1
e34ccea042fd  tns_db3:latest   9000/tcp              tns_db3_1
c0d53d1327b4  tns_db2:latest   9000/tcp              tns_db2_1
0a920e17818a  tns_db1:latest   9000/tcp              tns_db1_1
Verify that the containers are reachable by curling one of the tns_lb instances.

By default, the containers listen on ports 8001 and 8002:

$ curl localhost:8001  # on a Mac, try: curl `boot2docker ip`:8001
lb-6d5b2352f76d4a807423ce847b80f060 via http://app1:8080
app-60fbe0a31aee9526385d8e5b44d46afb via http://db2:9000
db-e68d33ceeddbb77f4e36a447513367e8 OK
Launching Weave Scope
With the sample app up and running, you are ready to install and launch Weave Scope:

sudo wget -O /usr/local/bin/scope \
  https://github.com/weaveworks/scope/releases/download/latest_release/scope
sudo chmod a+x /usr/local/bin/scope
sudo scope launch
