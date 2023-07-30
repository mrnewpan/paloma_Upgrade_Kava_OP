## paloma_Upgrade_Kava_OP
### Создаем ключ для OP (Optimism Mainnet (op-main))
```
pigeon evm keys generate-new ~/.pigeon/keys/evm/op-main
```
### Создаем ключ для Kava (Kava Mainnet (kava-main))
```
pigeon evm keys generate-new ~/.pigeon/keys/evm/kava-main
```
### Задаем пароли для каждого кошелька
```
OP_PASSWORD=<pass>
KAVA_PASSWORD=<pass>
```
### Прописываем адреса кошельков для созданных ключей
```
OP_SIGNING_KEY=0x$(cat $HOME/.pigeon/keys/evm/op-main/*  | jq -r .address | head -n 1)
KAVA_SIGNING_KEY=0x$(cat $HOME/.pigeon/keys/evm/kava-main/*  | jq -r .address | head -n 1)
```
### Проверить адрес пополнения
```
echo "0x$(cat $HOME/.pigeon/keys/evm/op-main/*  | jq -r .address | head -n 1)"
echo "0x$(cat $HOME/.pigeon/keys/evm/kava-main/*  | jq -r .address | head -n 1)"
```
### В файл конфигурации здесь~/.pigeon/config.yaml дописать
```
  op-main:
    chain-id: 10
    base-rpc-url: ${OP_RPC_URL}
    keyring-pass-env-name: OP_PASSWORD
    signing-key: ${OP_SIGNING_KEY}
    keyring-dir: /root/.pigeon/keys/evm/op-main
    gas-adjustment: 1
    tx-type: 2

  kava-main:
    chain-id: 2222
    base-rpc-url: ${KAVA_RPC_URL}
    keyring-pass-env-name: KAVA_PASSWORD
    signing-key: ${KAVA_SIGNING_KEY}
    keyring-dir: /root/.pigeon/keys/evm/kava-main
    gas-adjustment: 1.2
    tx-type: 2

```
### В сервисный файл ~/.pigeon/env.sh дописать
```
cat <<EOT >~/.pigeon/env.sh
OP_SIGNING_KEY=<Your OP SIGNING KEY>
KAVA_SIGNING_KEY=<Your KAVA SIGNING KEY>
OP_PASSWORD=<Your OP Key Password>
KAVA_PASSWORD=<Your KAVA Key Password>
KAVA_RPC_URL=https://evm.kava.io
OP_RPC_URL=https://optimism.blockpi.network/v1/rpc/public
EOT
```
### Рестарт
```
sudo systemctl restart pigeond
```
