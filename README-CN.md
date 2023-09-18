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
  <a href="#系统总览">系统总览</a> •
  <a href="#使用示例">使用示例</a> •
  <a href="#安装">安装</a>  •
  <a href="#为什么使用微服务框架">为什么使用微服务框架</a> •
  <a href="#联系我">联系我</a>
</p>


MAES = **Microservice Architecture on Embedded System**.

MAES 提供给你的软件或系统如下能力：

1. 使用C/C++快速生成可运行的一个服务；
2. 支持服务之前（或称之为进程直接）通信；（支持发布订阅、请求应答方式）
3. 通过框架可创建并记录日志；
4. 通过框架加载配置文件以及使用参数；



# 系统总览
<img width="590" alt="image" src="https://github.com/jtttl/maes/assets/8311087/e15301f5-47c8-4cf8-b1de-6278faa70df4">



# 为什么使用微服务框架

将后端的微服务架构思想引入嵌入式领域：

1. 将你的单体软件拆分成若个服务程序（进程），并通过API进行关联，这样将会是你的软件可扩展性更强、更加轻松应对外界的需求、同时让微服务可以做到异常隔离；
2. 更加轻松的将任务分配给多个人开发，且开发的大部分过程开发人员之间相互没有依赖；
3. 使用微服务框架，其自带的通信机制，让服务直接通信更加轻松自然，甚至是跨设备之间的服务；
4. 希望你会喜欢 [MAES](https://www.github.com/jtttl/maes);



# 使用示例

```cpp
#include <iostream>
#include <memory>

// maes头文件包含
#include "MaesService.h"

// 创建一个class并继承maes::ServiceHandler
class MqttService : public maes::ServiceHandler {
public:
    MqttService() {}
    virtual ~MqttService() {}

public:

    // 设置日志存储位置
    std::string getLogPath() override {
        return "/tmp";
    }

    // 设置配置文件所在位置
    std::string getConfigFile() override {
        return "/home/config.json";
    }

    // 设置你的服务名
    std::string getServiceName() override {
        return "mqtt-service";
    }

    // 设置服务版本
    std::string getVersion() {
      	return "1.2.3";
    }

    // maes框架内启动顺序1
    void onInit() override {
        // 使用日志
	maeslog.debug("%s\n", __FUNCTION__);
    }

    // maes框架内启动顺序2
    int initHal() override {
        maeslog.debug("%s\n", __FUNCTION__);
      	// 初始化你的硬件外设
      	// beep_init();
        // uart_init();
        // nfc_init();
        return 0;
    }

    // maes框架内启动顺序3
    int initFml() override {
        maeslog.debug("%s\n", __FUNCTION__);
        // 初始化你的软件功能模块
      	// db_init();
      	// algorithm_init();
      	// encrypt_init();
        return 0;
    }

    // maes框架内启动顺序4
    int initAppl() override {
      	// 获取参数文件中的参数
        const int val = framework().getConfig()["mqtt-service"]["val"].asInt();

      	// do something
      	// ...

      	// 主循环
      	while(1) {
          // ...
        }

        return 0;
    }

    // maes退出时执行次函数
    void onExit() override {
        maeslog.debug("%s\n", __FUNCTION__);
    }
};

int main(int argc, char const *argv[]) {
    // 将上面创建的MqttService放入框架
    maes::Framework f(std::make_shared<MqttService>());
    // let's go
    return f.run();
}
```



# 安装

```bash
# 克隆仓库
$ git clone https://github.com/jtttl/maes

# 进入maes目录
cd maes

# 创建一个build目录
mkdir my_build

# 进入build目录
cd my_build

# 设置zmq库和头文件路径
cmake .. -DZMQ_LIB="..." -DZMQ_INC="..."

# 开始make
make
```



# 联系我

如果有想法或者合作意向等，可以联系我：

- 邮箱 : jtttl1221@gmail.com
- 微信 : jtl1221
