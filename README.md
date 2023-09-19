<h1 align="center">
<br>
  <a href="https://github.com/jtttl/maes"><img src="https://github.com/jtttl/maes/assets/8311087/b97fc5da-9944-4eb3-9bbe-8daa7c6e54d2" alt="maes" width="200"></a>
  <br>
  <font>MAES</font>
</h1>

<p align="center">
  <a><img src="https://img.shields.io/badge/based on-c++-green.svg?maxAge=2592000&amp;style=flat"></a>
  <a><img src="https://img.shields.io/badge/based on-zmq-green.svg?maxAge=2592000&amp;style=flat"></a>
  <a><img src="https://img.shields.io/badge/based on-zlog-green.svg?maxAge=2592000&amp;style=flat"></a>
  <a><img src="https://img.shields.io/badge/based on-jsoncpp-green.svg?maxAge=2592000&amp;style=flat"></a>
</p>

<p align="center">
  <a href="#Why Microservice">Why Microservice</a> •
  <a href="#System Overview">System Overview</a> •
  <a href="#How To Use">How To Use</a> •
  <a href="#Install">Install</a>  •
  <a href="#Contact me">Contact me</a>
</p>


MAES = **Microservice Architecture on Embedded System**.

MAES gives your system the abilities of :

1. Build your service fast in C/C++;
2. Multi-service or multi-process communication; (PUB/SUB and REQ/RESP)
3. Write and save logs;
4. Load configuration file and use configurations;

# Why Microservice

Why use this backend architecture in embedded system?

1. Breaking your monolithic application into services, which are then loosely connected via APIs. This allows for improved scalability, better fault isolation, and faster time to market;
2. Easy for two-pizza development teams;
3. For embedded system or embedded devices, the communication is easily between your services in your device or amoung your devices;
4. Hope you will love [MAES](https://www.github.com/jtttl/maes);


# System Overview
<img width="590" alt="image" src="https://github.com/jtttl/maes/assets/8311087/e15301f5-47c8-4cf8-b1de-6278faa70df4">


# How To Use
## Create a Service
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
    std::string getVersion() {
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
    maes::Framework f(std::make_shared<MqttService>());
    // let's go
    return f.run();
}
```
## Communication Between Microservices
### PUB-SUB
```cpp
// TODO
```

### REQ-RESP
```cpp
// TODO
```

# Install

```bash
# Clone this repository
git clone https://github.com/jtttl/maes

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

# Contact me

- Email : jtttl1221@gmail.com
- WeChat : jtl1221
