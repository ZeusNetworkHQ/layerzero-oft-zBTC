0. Use Solana 1.17.31 & Anchor 0.29.0

1. Create the OFT `programId` keypair by running:

```bash
anchor keys sync -p oft
```

2. View the program ID's based on the generated keypairs:

```
anchor keys list
```

You will see an output such as:

```bash
endpoint: H3SKp4cL5rpzJDntDa2umKE9AHkGiyss1W8BNDndhHWp
oft: DLZdefiak8Ur82eWp3Fii59RiCRZn3SjNCmweCdhf1DD
```

4. Copy the `oft` program ID value and Build the Solana OFT program

```bash
anchor build -v -e OFT_ID=<OFT_PROGRAM_ID>
```

5. Deploy the Solana OFT Program

Temporarily switch to Solana `v1.18.26` (remember to switch back to `v1.17.31` later)

```bash
sh -c "$(curl -sSfL https://release.anza.xyz/v1.18.26/install)"
```

Run the deploy command

```bash
solana program deploy --program-id target/deploy/oft-keypair.json target/verifiable/oft.so -u https://tame-long-surf.solana-mainnet.quiknode.pro/ef3559aa525fdd44bb2a68722630ee88f7d983af -k <DEPLOYER_KEYPAIR_PATH>
```

Switch back to Solana `1.17.31`

6. Deploy the OFT contract

```bash
pnpm hardhat lz:deploy --tags ZBTC
```

```bash
pnpm hardhat verify --network bsc \
  <OFT_ADDRESS> \
  "zBTC" \
  "zBTC" \
  <LAYERZERO_ENDPOINT_ADDRESS> \
  <DEPLOYER_ADDRESS>
```

7. Create OFT Adapter on Solana

- **Mechanism**: Lock and unlock
- **Token**: Use existing
- **Note**: ⚠️ Last resort option when you can't or won't transfer Mint Authority of existing token

```bash
pnpm hardhat lz:oft-adapter:solana:create --eid 30168 --program-id <OFT_PROGRAM_ID> --mint zBTCug3er3tLyffELcvDNrKkCymbPWysGcWihESYfLg --token-program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA
```

8. Enable Messaging

Run the following command to initialize the SendConfig and ReceiveConfig Accounts. This step is unique to pathways that involve Solana.

```bash
npx hardhat lz:oft:solana:init-config --oapp-config layerzero.config.ts
```

Run the wiring task:

```bash
pnpm hardhat lz:oapp:wire --oapp-config layerzero.config.ts
```

9. Sending OFTs

Send From 1 OFT from **Solana Mainnet** to **BSC Mainnet**

```bash
npx hardhat lz:oft:send --src-eid 30168 --dst-eid 30102 --to <EVM_ADDRESS>  --amount 1
```

Send 1 OFT From **BSC Mainnet** to **Solana Mainnet**

```bash
npx hardhat lz:oft:send --src-eid 30102 --dst-eid 30168 --to <SOLANA_ADDRESS>  --amount 1
```
