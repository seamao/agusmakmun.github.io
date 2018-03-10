---
layout: post
title:  "基于ipfs、智能合约的图片存储DAPP"
date:   2018-03-10
categories: [ipfs, dapp]
---
## 开局一条“狗”,后面全靠打!
----

<center> **《Dapp》**</center>




** 项目目标: **


* 结合ipfs,智能合约创建一个去中心化的图片储存查看平台.

** 项目需求: **

* 将图片存储到ipfs,通过哈希值读取图片.


** 环境依赖 **

  * Linux 系统 或者 Mac 系统

  - NodeJS 安装
```
安装地址 https://nodejs.org/en/ ,具体安装不做解释了,百度有很多教程,跟着教程去做就行了
```
  - truffle 安装

truffle介绍:    
以太坊是区块链开发领域最好的编程平台，而truffle是以太坊
（Ethereum）最受欢迎的一个开发框架.

全局安装truffle
```
npm install -g truffle
```
  - MetaMask 安装

MetaMask是以太坊网络非常好用的钱包插件,前提是要先安装谷歌浏览器.
提醒:MetaMask是谷歌浏览器拓展程序,所以需要翻墙才能查看,不能翻墙的朋友可以直接在这个链接下载,直接拉到拓展程序,同样可以用!
![](http://p3qaz7oyz.bkt.clouddn.com/%E8%B0%B7%E6%AD%8C%E6%8B%93%E5%B1%95%E7%A8%8B%E5%BA%8F.gif)

```
安装包：

链接: https://pan.baidu.com/s/12rumbY5xvpbr-gYtu3OoMg 密码: v958

```

  - ipfs 环境配置

```

区块链职业教育培训之父黎跃春已经有非常详细的安装步骤，详情可看下方链接

链接: https://pan.baidu.com/s/1LDyx9TVFPV8V8byYWFDlag 密码: 3yxb

```

_以上就是一些依赖环境了_


------

>这里要恭喜你完成了上面最艰难的环境搭建，下面就来做我们的项目吧！


*  **创建项目**

创建一个名为`Project`的文件夹，在文件夹里面输入`truffle unbox rect`命令创建项目（此处新建的项目是框架完整的项目）。

因为项目需要下载一些依赖包，所以时间会相对长点，具体看网速，等待就行了，我这渣渣网速需要10分钟时间。

```
maohaibo@maohaibo:~/桌面$ mkdir Project
maohaibo@maohaibo:~/桌面$ cd Project/
maohaibo@maohaibo:~/桌面/Project$ truffle unbox react
Downloading...
Unpacking...
Setting up...
```
下载完之后整个项目包里面的内容如下：
最主要的有几个

_contracts_  ： 我们在这里面编译智能合约

_node__modules_  ： 我们所需要的依赖包都在这里面存着

_src_  ： 我们需要在这里输入的是最多的

![](http://p3qaz7oyz.bkt.clouddn.com/truffle%E5%8C%85.png)


* **编写程序**


  一、在依赖包中导入ipfs包

在项目目录下运行下面的命令安装  ipfs接口
```
npm install --save-dev ipfs-api

```

安装完之后在`package.json`项目下会出现如下代码，表示安装成功

```
"dependencies": {
  "dotenv": "^2.0.0",
  "ipfs-api": "^18.1.1",
  "react": "^15.4.2",
  "react-dom": "^15.4.2"
},
```
安装成功后在`Project/src/utils/APP.js`加入如下代码


```
import ipfsAPI from 'ipfs-api'
const ipfs = ipfsAPI({host: 'localhost', port: '5001', protocal: 'http'})

const contractAddress = "你的合约地址"


```
如何获取你自己的合约地址呢？

1.在项目路径下进入`truffle`控制台，可以得到10个公钥对应10个秘钥，还有12个单词的住一次。

```
truffle develop
```
2.编译合约
```
compile
```
3.部署合约，`SimpleStorage:×××××××××××××××` 后面的字符串就是你的合约地址了
```
migrate
```
  二、 编译合约

用下面的合约把`Project/contracts/SimpleStorage` 路径下的代码取代掉

```
pragma solidity ^0.4.19;

contract SimpleStorage {

    string[] public photoArr;

    function storePhoto(string hash) public {
        photoArr.push(hash);
    }

    function getPhoto(uint index) public view returns (uint, string){
        return (photoArr.length, photoArr[index]);
    }

}
```      

  三、编译js文件

打开`Project/src/utils/APP.js`文件
用下面的代码替换点文件里面的所有代码

其中需要值得注意的是`你的合约地址`还是要填写你自己合约地址的

```
import React, {Component} from 'react'
import SimpleStorageContract from '../build/contracts/SimpleStorage.json'
import getWeb3 from './utils/getWeb3'

import './css/oswald.css'
import './css/open-sans.css'
import './css/pure-min.css'
import './App.css'

import ipfsAPI from 'ipfs-api'
const ipfs = ipfsAPI({host: 'localhost', port: '5001', protocal: 'http'})

const contractAddress = "你的合约地址"
let simpleStorageInstance

let saveImageOnIpfs = (reader) => {
	return new Promise(function (resolve, reject) {
		const buffer = Buffer.from(reader.result);
		ipfs.add(buffer).then((response) => {
			console.log(response)
			resolve(response[0].hash);
		})
		.catch((err) => {
			console.error(err)
			reject(err);
		})
	})
}

class App extends Component {
	constructor(props) {
		super(props)

		this.state = {
			photos: [],
			count: 0,
			web3: null
		}
  	}

  	componentWillMount() {
    // Get network provider and web3 instance. See utils/getWeb3 for more info.

    getWeb3.then(results => {
    	this.setState({web3: results.web3})
			this.instantiateContract()
		}).catch(() => {
			console.log('Error finding web3.')
		})
	}

  	instantiateContract() {
		const that = this
		const contract = require('truffle-contract')
		const simpleStorage = contract(SimpleStorageContract)
		simpleStorage.setProvider(this.state.web3.currentProvider)

    	this.state.web3.eth.getAccounts((error, accounts) => {
        	simpleStorage.at(contractAddress).then((instance) => {
				simpleStorageInstance = instance
			})
			.then(result => {
				return simpleStorageInstance.getPhoto(0)
			})
			.then(result => {
				console.log(result)
				let imgNum = result[0].c[0]
				if(imgNum===0){
					return
				}
				if(imgNum===1){
					this.setState({
						count: imgNum,
						photos: this.state.photos.concat([result[1]])
					})
				}
				if(imgNum>1){
					for(let i=0;i<imgNum;i++){
						(function(i){
							simpleStorageInstance.getPhoto(i)
							.then(result => {
								that.setState({
									photos: that.state.photos.concat([result[1]])
								})
							})
						})(i)
					}
				}
			})
		})
	}

  	render() {
		let doms = [],
			photos = this.state.photos
		for(let i=0; i<photos.length;i++){
			doms.push(<div key={i}><img src={"http://localhost:8080/ipfs/" + photos[i]}/></div>)
		}

		return (
			<div className="App">
				<header>上传图片至ipfs，并保存信息至以太坊区块</header>
				<div className="upload-container">
					<label id="file">选择图片</label>
					<input type="file" ref="file" id="file" name="file" multiple="multip
					le"/>
					<button onClick={() => this.upload()}>上传</button>
				</div>
				<div className="img-container">
					{doms}
				</div>
			</div>
		);
	}

 	upload() {
		var file = this.refs.file.files[0];
		console.log(file)
    	var reader = new FileReader();
		// reader.readAsDataURL(file);
		reader.readAsArrayBuffer(file)
		reader.onloadend = (e) => {
			//console.log(reader);
			saveImageOnIpfs(reader).then((hash) => {
				console.log(hash);

					simpleStorageInstance.storePhoto(hash, {from: this.state.web3.eth.accounts[0]})
					.then(() => {
						console.log("写入区块成功")
						this.setState({
							photos: this.state.photos.concat([hash])
						})
					})
			});
		}
	}

}

export default App
```

* **部署项目**

1、在项目路径运行 ipfs节点

```
输入：  ipfs daemon
```

2、因为我们之前部署了合约地址，所以我们先删除`build`文件

```
输入： rm -rf build
```
然后再重新进入`truffle`控制台
```
1、truffle develop  //进入控制台
2、compile     //编译合约
3、migrate     //部署合约
```

3、再开启一个终端，进入当前项目路径

```
输入：  npm run start

```
_需要注意的是：以上3个终端需要按照顺序同时进行_
等一会会自动跳转如下，传入图片，需要使用MetaMask

等一会图片就上传上去了
![](http://p3qaz7oyz.bkt.clouddn.com/ipfs%E4%BB%A5%E5%A4%AA%E5%9D%8A%E7%BB%93%E5%90%88.png)


---

恭喜你，完成了一个基于智能合约,ipfs的微型DAPP,蚂蚁虽小，五脏俱全。
文章若出现跑不通的情况，请到下方留言哦！
![](http://p3qaz7oyz.bkt.clouddn.com/%E5%85%AC%E4%BC%97%E5%8F%B7.jpeg)

