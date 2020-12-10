

Akash Week 2 Challange 6 Note







Before we get started, a word about Akash in passing.



Phase 3 will accelerate our vision for a decentralized and open cloud computing marketplace and serve as the launchpad for Mainnet 2. 

#### **Rewards** **_____**

You can check out [The Akashian Challenge site](https://akash.network/challenge/) for a detailed schedule of reward opportunities. A total of **500,000 AKT** is allocated for Phase 3 Rewards including:

1. **Guided Challenges**
2. **Open Ended Challenges**
3. **Network Support Challenges**
4. **Community Content Challenges**
5. **Bonus Challenges**



#### **This Week’s Live Stream on Friday _____**

Don’t miss our weekly Phase 3 live stream this Friday December 11th at 9am PST / 5pm UTC, featuring our CEO Greg Osuri and CTO Adam Bozanich, and hosted by Michael Gushansky, our Senior Global MarComm Manager.

We’ll have exclusive updates, a Q&A, and special prizes. 



#### **Don’t Miss the Latest Phase 3 Updates**

For more information about Akash, please use the following links to learn more.

Official Website: https://akash.network/?lang=zh-hans

Twitter: https://twitter.com/akashnet_

Telegram: https://t.me/AkashNW









The final challenge is to integrate the previous ones, which is difficult. If you get the previous ones right, you will have no problem deploying it.

The general flow is to use deploy-2-3.yaml (which is basically the same as 2-2), run the cloud node, and then call the cloud node's RPC to generate the authenticator information and deploy it as an authenticator. Once the validators are added to the list, you can close the deployment. The most important thing is to set the verifier node name (MONIKER) to an invitation code, which can be seen in the list of verifiers.

The official documentation can usually

Before deploying, you need to create a wallet and receive the test coins, see 

https://github.com/StephenPan/Akash-Challenge-3-process-akash-node and https://github.com/StephenPan/Akash-Week-2-Challenge-1-Process/blob/main/Akash%20Week%202%20Challenge%201%20Process.md  . The code is below.

The running code is below.







```

export SECONDARY_NODE=http://147.75.47.235:26657
export SECONDARY_MONIKER=fpszuvlusqdyia2
export SECONDARY_KEY_NAME=yaakov
export SECONDARY_CHAIN_ID=akash-edgenet-2



export AKASH_NET="https://raw.githubusercontent.com/ovrclk/net/master/edgenet"
export AKASH_CHAIN_ID="akash-edgenet-1"
export AKASH_NODE=tcp://rpc-edgenet.akashdev.net:26657
export KEY_NAME=yaakov
export KEYRING_BACKEND=os
export ACCOUNT_ADDRESS=akash15rrya4ayu8yewh3d2agg552phtttpkghpd0paf


# 部署应用
akash tx deployment create deploy-2-3.yaml --from $KEY_NAME --node $AKASH_NODE --chain-id $AKASH_CHAIN_ID -y

akash query market lease list --owner $ACCOUNT_ADDRESS --node $AKASH_NODE --state active

export PROVIDER=akash1uu8wfvxscqt7ax89hjkxral0r2k73c6ee97dzn
export DSEQ=173151
export OSEQ=1
export GSEQ=1

akash provider send-manifest deploy-2-3.yaml --node $AKASH_NODE --dseq $DSEQ --oseq $OSEQ --gseq $GSEQ --owner $ACCOUNT_ADDRESS --provider $PROVIDER
akash provider lease-status --node $AKASH_NODE --dseq $DSEQ --oseq $OSEQ --gseq $GSEQ --provider $PROVIDER --owner $ACCOUNT_ADDRESS
akash provider lease-status --dseq $DSEQ --gseq $GSEQ --oseq $OSEQ --provider $PROVIDER --owner $ACCOUNT_ADDRESS --node $AKASH_NODE | jq '.["forwarded-ports"].akash[] | select(.port==26657)'


export DEPLOY_NODE_RPC=http://na7nl1vkch8apdo4hp4ln1ljc4.provider3.akashdev.net:30036

akash --node $DEPLOY_NODE_RPC status | jq '.sync_info.latest_block_height,.sync_info.latest_block_time,.sync_info.catching_up'

akash provider lease-status --dseq $DSEQ --gseq $GSEQ --oseq $OSEQ --provider $PROVIDER --owner $ACCOUNT_ADDRESS --node $AKASH_NODE | jq '.services.akash.uris[0]'

export NODE_ENDPOINT=na7nl1vkch8apdo4hp4ln1ljc4.provider3.akashdev.net

curl -s "$NODE_ENDPOINT/validator-pubkey.txt"

export VALIDATOR_PUBKEY="$(curl -s "$NODE_ENDPOINT/validator-pubkey.txt")"

akash tx staking create-validator --node="$SECONDARY_NODE" --amount=1000000uakt --pubkey="$VALIDATOR_PUBKEY" --moniker="$SECONDARY_MONIKER" --chain-id="$SECONDARY_CHAIN_ID" --commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1" --gas-prices="0.025uakt" --from="$SECONDARY_KEY_NAME"


export SECONDARY_VALOPER_ADDRESS="$(akash keys show "$SECONDARY_KEY_NAME" --bech=val -a)"
akash --node "$SECONDARY_NODE" query staking validator "$SECONDARY_VALOPER_ADDRESS"

export CODE=fpszuvlusqdyia2

```













**That's it, it's easy, now go ahead and try it and complete your own Akash mission.**



To make it easier for beginners to complete the challenges, you can also refer to the following.









```
#!/bin/bash /bin/bash
AKASH_NET="https://raw.githubusercontent.com/ovrclk/net/master/edgenet"
AKASH_NODE="https://akash.rpc.best:443"
TESTNET_NODE="http://174.138.34.238:26657"
AKASH_CHAIN_ID="akash-edgenet-1"
ACCOUNT_ADDRESS="" #Need to fill in your own
KEY_NAME="" # Need to fill in your own
KEYRING_BACKEND="os"
PROVIDER= # need to fill in your own
DSEQ= # need to fill in your own
GSEQ=1
OSEQ=1
CODE= # need to fill in your own

SECONDARY_NODE=$TESTNET_NODE
SECONDARY_MONIKER=$CODE
SECONDARY_KEY_NAME=$KEY_NAME
SECONDARY_CHAIN_ID=akash-edgenet-2
DEPLOY_YML=deploy-2-3.yaml

# 1. initiate the deployment transaction, taking care to change the yml file

 --chain-id $AKASH_CHAIN_ID --fees 50000uakt -y

# 2. query the list of deployed mirrors, need to wait a while, and modify the variables above after exporting the PROVIDER and DSEQ.
akash query market lease list --owner $ACCOUNT_ADDRESS --node $AKASH_NODE \ \ NODE
--chain-id $AKASH_CHAIN_ID --state active

# 3. upload deployment license, no output
akash provider send-manifest $DEPLOY_YML --node $AKASH_NODE --dseq $DSEQ \\\
--oseq $OSEQ --gseq $GSEQ --owner $ACCOUNT_ADDRESS --provider $PROVIDER

# If you don't have jq on your Mac, use brew install jq to install it.
akash provider lease-status\\\
  --dseq $DSEQ\\.
  gseq $GSEQ \\--gseq $GSEQ
  --oseq $OSEQ\
  --provider $PROVIDER \\-\-\.
  --owner $ACCOUNT_ADDRESS \-\-\
  --node $AKASH_NODE\\
  | jq -r '. ["forwarded-ports"].akash[] | select(.port==26657)''

# Set the above output to the DEPLOY_NODE_RPC variable.
DEPLOY_NODE_RPC= # need to fill in your own

# View Sync Status
akash --node "$DEPLOY_NODE_RPC" status | jq '.sync_info.latest_block_height,\\\\.
.sync_info.latest_block_time, .sync_info.catching_up'

# 7. view RPC
akash provider lease-status\\\
  --dseq $DSEQ\\.
  gseq $GSEQ \\--gseq $GSEQ
  --oseq $OSEQ\
  --provider $PROVIDER \\-\-\.
  --owner $ACCOUNT_ADDRESS \-\-\
  --node $AKASH_NODE\\
  | jq -r '.services.akash.uris[0]''

# The result of the previous step is set to the NODE_ENDPOINT variable.
NODE_ENDPOINT= # need to fill in your own

# 9. obtaining verifier information
VALIDATOR_PUBKEY="$(curl -s "$NODE_ENDPOINT/validator-pubkey.txt")"

# 10. creating validators
akash tx staking create-validator\\
  --node="$SECONDARY_NODE" dhaka
  --amount=1,000,000uakt\\.
  ---pubkey="$VALIDATOR_PUBKEY" \\\\-$VALIDATOR_PUBKEY
  --moniker="$SECONDARY_MONIKER" \\\-moniker
  --chain-id="$SECONDARY_CHAIN_ID" \\\\
  --commission-rate-="0.10" \\␤
  --commission-max-rate="0.20" \\\-commission-max-rate="0.20" \-commission-max-rate="0.20
  --commission-max-change-rate-="0.01" \\␤
  --min-self-delegation="1" \\\
  --gas-prices="0.025uakt" \\␤asan
  --from="$SECONDARY_KEY_NAME"

# Check the validation status, and if the result is in the validator, you can commit and close the deployment.
SECONDARY_VALOPER_ADDRESS="$(akash keys show "$SECONDARY_KEY_NAME" --bech=val -a)"
akash --node "$SECONDARY_NODE" query staking validator "$SECONDARY_VALOPER_ADDRESS"

# Get json for pull request.
akash query market lease get\\
  --dseq $DSEQ\\.
  gseq $GSEQ \\--gseq $GSEQ
  --oseq $OSEQ\
  --provider $PROVIDER \\-\-\.
  --owner $ACCOUNT_ADDRESS \-\-\
  --node $AKASH_NODE -o json d\\\.
  > $CODE.json

# 13. close deployment
akash tx deployment close --node $AKASH_NODE --chain-id $AKASH_CHAIN_ID --dseq $DSEQ\\.
 --owner $ACCOUNT_ADDRESS --from $KEY_NAME --fees 50000uakt -y

Translated with www.DeepL.com/Translator (free version)
```







#### **Don’t Miss the Latest Phase 3 Updates**

For more information about Akash, please use the following links to learn more.

Official Website: https://akash.network/?lang=zh-hans

Twitter: https://twitter.com/akashnet_

Telegram: https://t.me/AkashNW

