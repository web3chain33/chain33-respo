# 本文介绍如何在Chain33链上按标准发行NFT并在小狐狸钱包中显示NFT的属性值以及图片

## 1. 连接到chain33测试链
以下内容基于remix和metamask钱包插件来完成合约的部署，调用和交易签名。 参考文档 [[环境配置]](../web3-sdk.md)   

## 2. 编译并发布合约到Chain33测试链
 - 准备一个ERC721合约

```
 pragma solidity ^0.8.6;

   import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
   import "@openzeppelin/contracts/utils/Counters.sol";
   import "@openzeppelin/contracts/access/Ownable.sol";
   import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

   contract MyNFT is ERC721URIStorage, Ownable {
       using Counters for Counters.Counter;
       Counters.Counter private _tokenIds;

       constructor() ERC721("Goddess", "GODDESS") {}

       function mintNFT(address recipient, string memory tokenURI)
           public onlyOwner
           returns (uint256)
       {
           _tokenIds.increment();

           uint256 newItemId = _tokenIds.current();
           _mint(recipient, newItemId);
           _setTokenURI(newItemId, tokenURI);

           return newItemId;
       }
   }
```
 - 通过remix和metamask将该合约部署到Chain33测试链上

## 3. 使用IPF存储图片并为NFT配置元数据
把图片和元数据存储到IPFS平台上，采用Pinata,如果没有注册，可以 [免费注册](https://app.pinata.cloud/) 一个。
创建好账户后：
- 导航到“文件”页面，然后单击页面左上角的蓝色“Upload”按钮。
- 将图像上传到 Pinata — 这将是您的 NFT 的图像资产。随意命名资产
- 上传后，在“File”页面的表格中看到文件信息。还将看到 CID 列。可以通过单击旁边的复制按钮来复制CID（例子里的CID值是：QmWgVLxwFbdcKX43DA71uBv7LdQjosEGstvUdU4EknRnjZ，在下文的json文件中会用到）。也可以通过“Name”列边上的预览查看上传的图片。
- 上传完图片后，接下来上传图片的描述信息（图片的属性，图片的存储路径等），内容如下所示：
```
{
  "attributes": [
    {
      "trait_type": "background",
      "value": "purple"
    }
  ],
  "description": "Goddess of twelve constellations",
  "image": "ipfs://QmWgVLxwFbdcKX43DA71uBv7LdQjosEGstvUdU4EknRnjZ",
  "name": "twelve constellations"
}
```
- 编辑好内容后，存成自定义.json文件，然后上传Pinata, 并获得json文件的路径：https://gateway.pinata.cloud/ipfs/QmZJPqNe2aukXrr2wErXaQhvRdiQcxWQpBW7xpMroLpXeg 
![上传文件](/resources/NFT_METAMASK.png) 

## 4. 通过Remix发行NFT资产
![发行NFT](/resources/Create_NFT.png) 
调用函数：mintNFT  
参数recipient：区块链地址，指定NFT拥有者  
参数tokenURI: 上文获得的json文件路径  
点击[transact]后，再通过小狐狸签名，就完成了NFT的发行。

## 5. 在MetaMask中导入上述NFT资产
- 打开MetaMask手机APP， 进入到[收藏品]标签页
![收藏品检查](/resources/MetaMast_NFT_Check.jpg) 
- 点击[添加收藏品]
填入NFT合约地址（在上述remix部署合约后可以获得）  
填入NFT ID值 
![收藏品导入](/resources/MetaMast_NFT_Import.jpg) 
- 点击[添加]按钮,再刷新页面，就可在钱包中看到NFT资产的属性值和图片
![收藏品查看](/resources/MetaMast_NFT_Show.jpg) 