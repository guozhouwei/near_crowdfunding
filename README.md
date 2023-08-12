# 众筹平台
## 背景
针对社会上需要需要帮助的事件，如：涿州水灾，流浪动物救助，留守儿童失学 等问题，现搭建此平台，发布募捐活动，募集NEAR，为救助对象提供帮助；
## 平台主要能力
```shell
1、发布募捐活动，募集near币
2、用户参与募捐活动，向募捐指定账户转入NEAR
2、对捐赠的用户赠送纪念NFT
```

## 代码仓库
https://github.com/guozhouwei/near_crowdfunding


## 业务逻辑图
![avatar](https://github.com/guozhouwei/near_crowdfunding/blob/master/images/%E5%86%B3%E7%AD%96%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

## 部署和交互
### 账户
假设你注册了4个测试网账户:
```shell
1. 主账户（同合约部署签名账户）owner123.near
2. 合约账户 contract.near
3. 募捐人账户1 a.near
4. 募捐人账户2 b.near
```

#### 以上账户私钥保存在 legacy keychain 中
```shell
near account import-account using-seed-phrase "${YOUR_SEED_PHRASE}" --seed-phrase-hd-path 'm/44'\''/397'\''/0'\''' network-config testnet

```

### 编译募捐合约
1. 进入项目目录
    ```shell
   cd .
    ```
2. 安装 WASM 工具链
    ```shell
   rustup target add wasm32-unknown-unknown
    ```
3. 编译合约
    ```shell
   RUSTFLAGS="-C link-arg=-s" cargo build --target wasm32-unknown-unknown --release
    ```
4. 将合约 WASM 文件移动到项目根目录下方便后续操作
    ```shell
   mkdir -p ./res && cp ./target/wasm32-unknown-unknown/release/contract.wasm ./res/
    ```
以上操作已经封装在 makefile 文件中
    ```shell
   make all
    ```



### 合约交互
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


