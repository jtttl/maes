<h1 align="center">
  <font>MAES</font>
</h1>

<p align="center">
  <a><img src="https://img.shields.io/badge/based on-c++-green"></a>
  <a><img src=" https://img.shields.io/badge/based on-zmq-green"></a>
  <a><img src=" https://img.shields.io/badge/based on-zlog-green"></a>
  <a><img src=" https://img.shields.io/badge/based on-jsoncpp-green"></a>
</p>

<p align="center">
  <a href="#System Overview">System Overview</a> •
  <a href="#How To Use">How To Use</a> •
  <a href="#Install">Install</a>  •
  <a href="#Why microservice">Why microservice</a>
</p>


maes = **Microservice Architecture on Embedded System**.

maes gives your system the abilities of :

1. build your service fast in C/C++;
2. mutity-service or mulity-process communication; (PUB/SUB and REQ/RESP)
3. write and save logs;
4. load and configuration file and use configurations;



# System Overview
<img width="590" alt="image" src="https://github.com/jtttl/maes/assets/8311087/d562ad71-d0a9-4b2f-8f69-7215b66aedf9">



# How To Use

```cpp
#include <iostream>
#include <memory>

// maes include
#include "MaesService.h" 

// Create a derived class from maes::ServiceHandler
class MqttService : public maes::ServiceHandler {
public:
		MqttService() {}
  	virtual ~MqttService() {}
  
public:

  	// path of storing all the log files 
    std::string getLogPath() override {
        return "/tmp";
    }
  
  	// path of configuration file
    std::string getConfigFile() override {
        return "/home/config.json";
    }

  	// set your service name
    std::string getServiceName() override {
        return "mqtt-service";
    }
  
  	// set your service version
	  std::string getVersion()
    {
      	return "1.2.3";
    }
  
  	// startup sequence 1 inside maes
    void onInit() override {
				// logs
				maeslog.debug("%s\n", __FUNCTION__); 
    }

  	// startup sequence 2 inside maes
    int initHal() override {
				maeslog.debug("%s\n", __FUNCTION__); 
      	// init your hardware
      	// beep_init();
        // uart_init();
        // nfc_init();
        return 0;
    }

  	// startup sequence 3 inside maes
    int initFml() override {
				maeslog.debug("%s\n", __FUNCTION__); 
        // init your functions
      	// db_init();
      	// algorithm_init();
      	// encrypt_init();
        return 0;
    }

  	// startup sequence 4 inside maes
    int initAppl() override {
      	// get configurations
				const int val = framework().getConfig()["mqtt-service"]["val"].asInt();
     		
      	// do something
      	// ...
      	
      	// main loop
      	while(1) {
          // ...
        }
      
        return 0;
    }

  	// on maes exit
    void onExit() override {
				maeslog.debug("%s\n", __FUNCTION__); 
    }
};

int main(int argc, char const *argv[]) {
  	// put your service into the maes
    maes::Framework f(std::make_shared<TpuService>());
  	// let's go
    return f.run();
}
```



# Install

```bash
# Clone this repository
$ git clone https://github.com/jtttl/maes

# Go into the repository
cd maes

# Create build directory for cmake
mkdir my_build

# Go into my_build
cd my_build 

# Set zmq lib and inc directories
cmake .. -DZMQ_LIB="..." -DZMQ_INC="..."

# Make
make
```

# Why microservice

1. Breaking your monolithic application into services, which are then loosely connected via APIs. This allows for improved scalability, better fault isolation, and faster time to market.
2. Easy for Two-Pizza Development Teams.

