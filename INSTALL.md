#### Download and Build OpenSDS Source Code

* Configure local environment

```sh
export GOPATH=$HOME/gopath
export PATH=$HOME/gopath/bin:$PATH
mkdir -p $HOME/gopath/src/github.com/opensds/   
cd $HOME/gopath/src/github.com/opensds
```

* Download OpenSDS source code

```sh
git clone https://github.com/opensds/opensds.git $HOME/gopath/src/github.com/opensds/
```

* Import dependency packages

```sh
go get github.com/opensds/opensds/cmd/osdsctl
go get github.com/opensds/opensds/cmd/osdslet
go get github.com/opensds/opensds/cmd/osdsdock
```

* Build OpenSDS source code to generate executable files

```sh
cd cmd/sdslet
go build
   
cd cmd/sdsctl
go buld

cd cmd/sdsdock
go build
```

* Move these executable files to /usr/local/bin

```sh
cp cmd/sdslet/sdslet /usr/local/bin
cp cmd/sdsctl/sdsctl /usr/local/bin
cp cmd/sdsdock/sdsdock /usr/local/bin
```

### Dependencies Installation

* Install pip

```sh
curl https://bootstrap.pypa.io/get-pip.py | python
```

* Install python dependencies

```sh
curl https://bootstrap.pypa.io/get-pip.py | python
```

* Install os-brick

```sh
pip install git+https://github.com/leonwanghui/os-brick.git
```

* Create configuration files

```sh
cp /usr/local/etcc/os-brick/ /etc/ -r
```

* Modify config file(/etc/os-brick/os-brick.conf) and change one line below:

```sh
[DEFAULT]
my_ip=your_host_ip
```

### Configuration

* Configure backend storage credential information

```sh
sudo mkdir /etc/opensds
```

```sh
vim examples/config.json
```

```sh
sudo cp examples/config.json /etc/opensds/
```

* Configure dock route table in controller module

```sh
sudo touch /etc/opensds/dock_route.json 
```

* Create OpenSDS logging directory

```sh
sudo mkdir /var/log/opensds
```

#### Run OpenSDS Service

* Start **osdsdock** with root access (for logging purpose)

```sh
sudo cp examples/osdsdock /etc/init.d
```

```sh
sudo chmod +x /etc/init.d/osdsdock
```

```sh
service osdsdock start
```

* Start **osdslet** with root access (for logging purpose)

```sh
sudo osdslet //suppose the user has copied the compiled binary to /usr/local/bin
```

* Run **osdsctl** for operations you want to perform. 

```sh
sudo osdsctl --help //see what you can do with opensds
```

```sh
sudo osdsctl dock register osdsdock_node_ip //register your osdsdock node ip into controller.
```

Currently osdsctl supports all the basic Cinder/Manila operations, for example if you want to 
create a 1GB volume from a Dell-EMC VMAX, which is connected to the OpenSDS underlay infra - 
OpenStack Cinder via its in-tree vmax cinder driver, using OpenSDS for an easy access:

```sh
sudo sdsctl volume create 1 -n cinder-vmax-volume -b cinder
```
Viola !
