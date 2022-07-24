# GO-SDK使用说明

## 通过GO-SDK实现合约部署调用
### 1. 子目录说明
demo目录  
- commont_test.go：包含一些常用的方法（区块链地址和私钥的生成; 获取最大区块高度; 根据区块高度获取区块详情信息）  
- main.go: 通过go-sdk部署，运行和查询evm合约（ERC1155）的方法  

### 2. 运行Demo程序
2.1 生成地址和公私钥  
生成ETH类型地址：go test -v ./ -test.run TestCreateAccountEth  

2.2 运行部署，发行，调用合约程序  
- 修改main.go，将上一步生成的内容，分别填充到以下几个参数中，注意私钥即资产，要隐私存放，而地址是可以公开的  
```  
// 合约部署人的地址和私钥
deployAddress    = "0x4cb94044427edb06ae7aeb8e8dd6eba078c8bc0a"
deployPrivateKey = "7dfe80684f7007b2829a28c85be681304f7f4cf6081303dbace925826e2891d1"
// 代扣手续费的地址和私钥
withholdAddress    = "0xfd89c32962f19bcea69b76093a64a03618cb33be"
withholdPrivateKey = "56d1272fcf806c3c5105f3536e39c8b33f88cb8971011dfe5886159201884763"
// 用户地址
useraAddress    = "0x6856f610b40e7321cace9e1f8752315110862573"
useraPrivateKey = "0x3967abcafaea83fee72766ca6dae578f4f156b5d1dae1ddf119e4564d5e2658c"
```

- 给上述合约部署人和代扣地址下充燃料（公链需要燃料，联盟链不需要）, 所有的用户都走代扣地址来扣除燃料  

- 运行测试程序  
```
# 在demo目录下  
windows下： go run ./
linux下： go run *.go
```