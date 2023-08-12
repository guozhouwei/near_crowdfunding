# 众筹平台
## 背景
针对社会上需要需要帮助的事件，如：涿州水灾，流浪动物救助，留守儿童失学 等问题，现搭建此平台，发布募捐活动，募集NEAR，为救助对象提供帮助；
## 平台主要能力
1、发布募捐活动，募集near币
2、用户参与募捐活动，向募捐指定账户转入NEAR
2、对捐赠的用户赠送纪念NFT

## 代码仓库
https://github.com/guozhouwei/near_crowdfunding


## 业务逻辑图
![avatar](http://baidu.com/pic/doge.png)

## 部署和交互
假设你注册了两个测试网账户 `alice111.testnet` 和 `code111.testnet`, 一个用于作为主账户, 另一个用于作为合约账户, 私钥保存在 legacy keychain 中

1. 部署并初始化合约
    ```shell
    near contract deploy code.testnet use-file ./res/hello_near.wasm with-init-call init json-args '{"owner_id":"alice111.testnet"}' prepaid-gas '100.000 TeraGas' attached-deposit '0 NEAR' network-config testnet sign-with-keychain send
    ```
2. 调用 Change 方法
    ```shell
    near contract call-function as-transaction code.testnet set_account_description json-args '{"account_id":"bob.testnet","description":"Nice Bob"}' prepaid-gas '100.000 TeraGas' attached-deposit '0 NEAR' sign-as alice.testnet network-config testnet sign-with-keychain send
    ```
3. 调用 View 方法
    ```shell
    near contract call-function as-read-only code111.testnet get_account_description json-args '{"account_id":"bob.testnet"}' network-config testnet now
    ```


