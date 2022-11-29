**Introduction**

Welcome to Vault (link: https://eos.defibox.io/vault/) documentation!
This document is divided into the following sections:
About a quick overview of the Vault program.
A user guide explaining to DeFi projects how to use the Vault system.
Contract Audit Report


**About Vault**

The Vault protocol is the first single-token lossless yield protocol launched by Defibox. Users can earn corresponding token income by depositing tokens. Assets can be accessed flexibly and the chain is open and transparent. Vault income mainly comes from Defibox protocol income, Yield+ rewards, node income, REX income, etc. At the same time, in order to improve the utilization rate of funds, the protocol will issue sToken representing the deposit certificate, which is a standard EOS token, and supports and other protocol use in combination to get more benefits, for example: the deposit of sEOS is supported by lending, and the generating USN as collateral is supported by USN.
This documentation focuses on providing relevant, accessible guidance for users and developers. For an in-depth exploration and discussion of the vault system, see the requirements design document:


**User Guide:**

Deposit EOS for yield: transfer method


**Deposit EOS in vault.defi:**

***cleos push action   eosio.token transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 EOS","memo":""}' -p testtesttest***

The transfer method, which requires three parameters:
From: The name of the deposited user
To: Deposited contract address, fixed vault.defi
Quantity: The amount of EOS deposited, which needs to be formatted according to the token precision of EOS
Memo: It is the remark of the transfer, which can be empty and has no logic
After the deposit is successful, vault.defi will issue a corresponding amount of sEOS to the user based on the current exchange rate between sEOS and EOS. The user can query the token.defi(sEOS) on their account, and the deposited EOS will automatically buy REX, start enjoying the benefits of REX.


**Withdraw EOS:**

The first step is to transfer sEOS to vault.defi and send the command for withdraw

***cleos push action token.defi transfer '{"from": "testtesttest ","to":"vault.defi","quantity":"200.0000 SEOS","memo":""}' -p testtesttest***

After the transfer is successful, a withdrawal record will be generated in the release table of the vault.defi contract. 

There are three fields in this table record:
Quantity: The amount of sEOS withdrawn
Rate: Record the exchange rate between sEOS and EOS at the time of withdrawal. After 5 days, the amount of EOS that needs to be transferred (deducting the withdrawal fee) by the recorded exchange rate 
Time: After the withdrawal, there is a 5-day unlock period, so the withdrawal will be completed after 5 days, and the sEOS will be exchanged back to EOS and transferred back to the user

The second step is to use the release method to withdraw EOS after the 5-day unlock expires
The release method has only one parameter owner, which is for the user who needs to withdraw when it expires:

***cleos push action vault.defi release '["testtesttest"]' -p testtesttest***

The EOS in the withdrawal meeting will be deducted from the corresponding withdrawal fee and returned to the user. The user can withdraw it by himself when the unlock expires. At present, the system background triggers the automatic withdrawal and returns it to the user


**Vault pool reward injection: income method**
The award.defi account preserves Defibox’s protocol income, BP income, and Y+ rewards. Every 10 minutes, the system automatically calls the income method to inject the reward token from award.defi into vault.defi. Any user can call this method income, as follows:

***cleos push action vault.defi income '[]' -p testtesttest***

The Income method does not have any parameters, the logic of the contract is fixed, and the calculation logic of the amount transferred at intervals of 10 minutes is: income_ratio/10000 of the EOS token configured according to the collaterals table of the vault.defi contract * the current EOS balance of award.defi = 10 minutes to inject EOS amount to vault.defi, if it is not executed in the interval of 10 minutes, it will be re-rolled in next execution


**Contract audit report**

https://www.certik.com/projects/defibox
