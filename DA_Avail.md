## create a new rollap according to the [guide](https://docs.dymension.xyz/build/quick-start/roller-quick/install)
# install
- install Roller
```
curl -L https://dymensionxyz.github.io/roller/install.sh | bash
roller version
```
- init
```
roller config init <rollapp-id> <denom>
```
- go to discord dymension faucet dym and check balance
```
$request <dym-address>
$request <dym-address>
$request <celestia-address>

$balance <dym-address>
```
- register
```
roller tx register
```
- run as systemd
```
systemctl --version

roller services load
sudo systemctl enable sequencer
sudo systemctl enable relayer

sudo systemctl start sequencer
sudo systemctl start relayer

sudo systemctl status sequencer
sudo systemctl status relayer

tail -f ~/.roller/rollapp/rollapp.log
tail -f ~/.roller/relayer/relayer.log
```
# IBC transfer
- check src-channel
```
roller relayer status
```
- fund the Dymension Hub faucet with your RollApp token on froopyland testnet
```
roller tx fund-faucet
```
- ibc transfer
```
rollapp_evm tx ibc-transfer transfer transfer <src-channel> dym1g8sf7w4cz5gtupa6y62h3q6a4gjv37pgefnpt5 5000000000000000000000000<base-denom> --from rollapp_sequencer --keyring-backend test --home ~/.roller/rollapp --broadcast-mode block
#example
rollapp_evm tx ibc-transfer transfer transfer channel-0 dym1g8sf7w4cz5gtupa6y62h3q6a4gjv37pgefnpt5 1000000000000000000uDYM --from rollapp_sequencer --keyring-backend test --home ~/.roller/rollapp --broadcast-mode block
```
- check the balance of your RollApp token on the Dymension Hub's froopyland faucet discord channel
```
$balances dym1g8sf7w4cz5gtupa6y62h3q6a4gjv37pgefnpt5 <rollapp-id>
#example
$balances dym1g8sf7w4cz5gtupa6y62h3q6a4gjv37pgefnpt5 thonguyen_8920268-1
```
- Users will be able to request tokens
```
$request <user-address> <rollapp-id>
```
# keys
- List keys
```
roller keys list
```
- Export keys
```
roller keys export hub_sequencer
roller keys export rollapp_sequencer
roller keys export my_celes_key
roller keys export my_avail_key
```
##  migrate DA from celestia to A vail
# migrate
```
systemctl stop da-light-client 
systemctl stop sequencer
systemctl stop relayer

rm -rf /etc/systemd/system/sequencer.service
rm -rf /etc/systemd/system/da-light-client.service
rm -rf cat /etc/systemd/system/relayer.service

roller config set da avail

sed -i 's|wss://dymension-devnet.avail.tools/ws|wss://goldberg.avail.tools/ws|g' /root/.roller/rollapp/config/dymint.toml

***edit $HOME/.roller/da-light-node/avail.toml and $HOME/.roller/rollapp/config/dymint.toml to correct your Mnemonic and check again roller keys list***
- faucet avail to the [guide](https://docs.availproject.org/about/faucet/)

roller services load


sudo systemctl enable sequencer
sudo systemctl enable relayer


sudo systemctl restart sequencer
sudo systemctl restart relayer

sudo systemctl status sequencer
sudo systemctl status relayer

tail -f ~/.roller/rollapp/rollapp.log
tail -f ~/.roller/relayer/relayer.log
```
# Update Your RollApp in the Dymension Registry
- you will need to submit a new PR [here](https://docs.dymension.xyz/build/production/portal-listing)
- check rollapp
```
roller config export
```
- edit file json
```
{
  "chainId": "your_chain_id",
  "chainName": "Your Chain Name",
  "rpc": "http://your.rpc.url:port",
  "rest": "http://your.rest.url:port",
  "bech32Prefix": "your_prefix",
  "currencies": [
    {
      "displayDenom": "YOUR_TOKEN",
      "baseDenom": "uYOUR_TOKEN",
      "decimals": 18,
      "logo": "/path/to/your/logo.png",
      "type": "main"
    }
  ],
  "coinType": 60,
  "faucetUrl": "http://link.to.your.faucet",
  "website": "http://link.to.your.website",
  "logo": "/path/to/your/logo.png",
  "ibc": {
    "hubChannel": "your_hub_channel",
    "channel": "your_channel",
    "timeout": 172800000
  },
  "evm": {
    "chainId": "your_evm_chain_id",
    "rpc": "http://your.evm.rpc.url:port"
  },
  "type": "RollApp",
  "da": "Avail",
  "description": "Description of your RollApp",
  "analytics": true,
  "goldberg": true,
  "availAddress": "Your RollApp's Avail address"
}
```

