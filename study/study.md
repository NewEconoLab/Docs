# NEO开发技术学习手册
## NEO技术简述
NEO 技术指的是NEO区块链技术

NEO 官网(http://neo.org)

NEO GITHUB(https://github.com/neo-project)

NEO是一项区块链基础技术，你可以使用NEO提供的技术快速搭建一条区块链。

NEO的区块链定位是智能经济，为使用智能合约创建各种DAPP提供底层支撑。

## NEO官方主要项目介绍

### 核心项目

Neo(https://github.com/neo-project/neo)

NeoVM(https://github.com/neo-project/neo-vm)

其中NeoVM是智能合约运行的虚拟机，Neo是Neo所有的算法、网络、节点的关键代码

### 节点项目

NeoGUI(https://github.com/neo-project/neo-gui)

NeoCLI(https://github.com/neo-project/neo-cli)

NeoGUI是一个图形化的节点，主要用于客户端使用，他是一个演示性质的客户端，供客户端开发者研究学习。

NeoCLI是官方提供的命令行节点，具有主要的功能，可以在生产环境作为服务器节点使用。

### 智能合约编译器相关

NeoCompiler(https://github.com/neo-project/neo-compiler)

智能合约编译器，用来将c#代码 和 java代码编译为智能合约指令

NeoDevPackDotnet(https://github.com/neo-project/neo-devpack-dotnet)

c#智能合约开发包

NeoDevpackJava(https://github.com/neo-project/neo-devpack-java)

java智能合约开发包

## NEL学习项目介绍
NEL 有众多项目，此文档定位在于NEO开发学习的指导手册，故而只介绍一部分和开发有直接关系的项目

NEONDEBUG(https://github.com/NewEconoLab/neondebug)

智能合约调试工具

NeoThinSDK_CS(https://github.com/NewEconoLab/neo-thinsdk-cs)

NEO轻钱包开发SDK，c#版

NeoThinSDK_TS(https://github.com/NewEconoLab/neo-thinsdk-ts)

NEO轻钱包开发SDK,Typescript 版


## 通用学习资料

这部分资料为开发者初步了解NEO系统准备。

看这部分内容前建议先看官网技术文档，这里针对性更强，在解释上面更注重对程序部分的说明。官网的内容更加普适，建议先阅读。

[Video 如何学习NEO开发](https://v.qq.com/x/page/m05494b9plq.html)

[00了解NEO的基本使用](startlesson/start.md)

[01了解UTXO](startlesson/utxo.md)

[Video UTXO小实验](https://v.qq.com/x/page/l0544i8lnwm.html)

[02了解NEO的基本架构](startlesson/neobase.md)

[03了解NEO智能合约](startlesson/smartcontract.md)

## 定向学习资料

以下资料为不同方向的开发者准备，排名不分先后，都很重要。

[开始之前请先阅读这里](study_more.md)

### 1.Neo 节点开发与修改

### 2.pc版轻钱包开发

### 3.web版轻钱包开发

### 4.智能合约开发
[NEO智能合约开发（一）不可能完成的任务](smartcontract/lesson01-imf.md)

[Video 如何调试智能合约](https://v.qq.com/x/page/r0537xvmwm1.html)

### 5.交易所开发

### 6.浏览器开发

### 7.运维技能

[Docker技术快速构建Neo私链](DevOps/DockerNeoPrivatenet.md)


