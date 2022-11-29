**Introduction**

Welcome to Vault (link: https://eos.defibox.io/vault/) documentation!
This document is divided into the following sections:
- About a quick overview of the Vault program.
- A user guide explaining to DeFi projects how to use the Vault system.
- Contract Audit Report


**About Vault**

The Vault protocol is the first single-token lossless yield protocol launched by Defibox. Users can earn corresponding token income by depositing tokens. Assets can be accessed flexibly and the chain is open and transparent. Vault income mainly comes from Defibox protocol income, Yield+ rewards, node income, REX income, etc. At the same time, in order to improve the utilization rate of funds, the protocol will issue sToken representing the deposit certificate, which is a standard EOS token, and supports and other protocol use in combination to get more benefits, for example: the deposit of sEOS is supported by lending, and the generating USN as collateral is supported by USN.
This documentation focuses on providing relevant, accessible guidance for users and developers. For an in-depth exploration and discussion of the vault system, see the requirements design document:


**User Guide:**

Deposit EOS for yield: transfer method


**Deposit EOS in vault.defi:**

```bash
cleos push action   eosio.token transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 EOS","memo":""}' -p testtesttest
```
The transfer method, which requires three parameters:
From: The name of the deposited user
To: Deposited contract address, fixed vault.defi
Quantity: The amount of EOS deposited, which needs to be formatted according to the token precision of EOS
Memo: It is the remark of the transfer, which can be empty and has no logic
After the deposit is successful, vault.defi will issue a corresponding amount of sEOS to the user based on the current exchange rate between sEOS and EOS. The user can query the token.defi(sEOS) on their account, and the deposited EOS will automatically buy REX, start enjoying the benefits of REX.


**Withdraw EOS:**

The first step is to transfer sEOS to vault.defi and send the command for withdraw

```bash
cleos push action token.defi transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 SEOS","memo":""}' -p testtesttest
```
After the transfer is successful, a withdrawal record will be generated in the release table of the vault.defi contract. 

There are three fields in this table record:
Quantity: The amount of sEOS withdrawn
Rate: Record the exchange rate between sEOS and EOS at the time of withdrawal. After 5 days, the amount of EOS that needs to be transferred (deducting the withdrawal fee) by the recorded exchange rate 
Time: After the withdrawal, there is a 5-day unlock period, so the withdrawal will be completed after 5 days, and the sEOS will be exchanged back to EOS and transferred back to the user

The second step is to use the release method to withdraw EOS after the 5-day unlock expires
The release method has only one parameter owner, which is for the user who needs to withdraw when it expires:

```bash
cleos push action vault.defi release '["testtesttest"]' -p testtesttest
```
The EOS in the withdrawal meeting will be deducted from the corresponding withdrawal fee and returned to the user. The user can withdraw it by himself when the unlock expires. At present, the system background triggers the automatic withdrawal and returns it to the user


**Vault pool reward injection: income method**
The award.defi account preserves Defibox’s protocol income, BP income, and Y+ rewards. Every 10 minutes, the system automatically calls the income method to inject the reward token from award.defi into vault.defi. Any user can call this method income, as follows:

```bash
cleos push action vault.defi income '[]' -p testtesttest
```
The Income method does not have any parameters, the logic of the contract is fixed, and the calculation logic of the amount transferred at intervals of 10 minutes is: income_ratio/10000 of the EOS token configured according to the collaterals table of the vault.defi contract * the current EOS balance of award.defi = 10 minutes to inject EOS amount to vault.defi, if it is not executed in the interval of 10 minutes, it will be re-rolled in next execution


**Contract audit report**

https://www.certik.com/projects/defibox



**介绍**

欢迎使用Vault(链接：https://eos.defibox.io/vault/)文档！
本文档分为以下部分：
- 关于 Vault提供了 Vault 程序的快速概览。
- 用户指南向 DeFi 项目解释了如何使用 Vault系统。
- 合约审计报告

**关于Vault**
Vault協議是Defibox推出的首個單幣無損收益協議，用戶存入代幣可賺取對應的代幣收益，資產可靈活存取且鏈上公開透明。 Valut的收益主要來源於Defibox協議收入、Yield+獎勵、節點收入、REX收入等，同時為了提高資金的使用率，協議將發放代表存幣憑證sToken，該憑證是標準的EOS代幣，支持和其他協議進行組合使用獲得更多收益,例：目前借贷已支持的sEOS的存款功能，USN已支持的做为抵押物生成USN功能。
本文档侧重于为用户和开发人员提供相关的、可访问的指南。有关 vault系统的深入探索和讨论，请参阅需求设计文档：


**用户指南：**
存入EOS获取收益：transfer方法
将EOS存入vault.defi合约，如下：

```bash
cleos push action   eosio.token transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 EOS","memo":""}' -p testtesttest
```
转账的方法，它需要三个参数：
From：存入的用户名称
To:存入的合约地址,固定vault.defi
Quantity:存入的EOS数量，需要按EOS的币种精度进行格式化
Memo：是转账的备注，可为空，没有逻辑
存入成功后，vault.defi会根据当前的sEOS与EOS的兑换率，发行对应数量的sEOS给用户，用户可在自己账号上查询stoken.defi(sEOS)，同时存入的EOS会自动买入REX，开始享受REX的收益


**取回EOS：**
第一步，先将sEOS转vault.defi，发送需要提取的指令

```bash
cleos push action   stoken.defi transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 SEOS","memo":""}' -p testtesttest
```
转账成功后，会在vault.defi合约的release表生成一笔提取记录，该表记录的三个字段：
Quantity:提取的sEOS数量
Rate:记录提取时sEOS与EOS的兑换率，5天到期后按该汇率结算需要转账的EOS按数量(扣除提取手续费)
Time:因提取后，需要有5天的解锁期，故在5天以后才是真正的提取完成，将sEOS换回EOS转回用户

第二步，5天解锁到期后调用release方法提取EOS
release方法只有一个参数owner，即到期需要提取的用户，如:

```bash
cleos  push action  vault.defi release '["testtesttest"]' -p testtesttest
```

提取会的EOS会扣除相应的提取手续费返还用户，解锁到期提取可用户自己提取，目前系统后台统一触发自动提取返回用户


**Vault池子注入奖励：income方法**
award.defi账号囤积了defibox的协议收入、节点收入、Y+奖励，每隔10分钟，系统自动调用income方法，从award.defi将奖励币种注入vault.defi，任何用户都可以调用该方法income，如下：

```bash
cleos  push action vault.defi income '[]' -p  testtesttest
```
Income方法没有任何参数，合约固定的逻辑，间隔10分钟转账的数量计算逻辑为: 根据vault.defi合约collaterals表配置的EOS币种的income_ratio/10000 * award.defi的当前EOS余额=10分钟需要注入vault.defi的EOS数量，如果中间10分钟没有执行，再下次执行时会进行补转


**合约审计报告**

https://www.certik.com/projects/defibox
