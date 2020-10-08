# cardano-leader-logs
https://github.com/MarcelKlammer/cardano-leader-logs

## 1.任意のノードでnode.jsとpython3と関連パッケージをインストールする
https://github.com/MarcelKlammer/cardano-leader-logs/blob/main/README.md  
nodejsとpythonの項目を実行する


## 2.Githubからクローンを作成する
```bash
cd $git #←任意のフォルダでOK
git clone https://github.com/MarcelKlammer/cardano-leader-logs
cd cardano-leader-logs
```

## 各ファイルを編集する

### 1.取得するスロットリーダーを日本時間にする
```bash
nano isSlotLeader.py
```
'''text
pytz.timezone('Asia/Tokyo')
```bash
33行目のタイムゾーンを書き換える

### 2.設定ファイルを作成する
```bash
cp example.leaderlogs.json slotLeaderLogsConfig.json
nano slotLeaderLogsConfig.json
```

```bash
{
  "epochNonce":       "5ee77854fe91cc243b8d5589de3192e795f162097dba7501f8d1b0d5d7546bd5",

  "poolId":           "00000000000000000000000000000000000000000000000000000000",
  "vrfSkey":          "/path/to/vrf.skey",

  "genesisShelley":   "/path/to/mainnet-shelley-genesis.json",
  "genesisByron":     "/path/to/mainnet-byron-genesis.json",

  "ledgerState":      "/path/to/cardano-leader-logs/ledgerstate.json",

  "libsodiumBinary":  "/usr/local/lib/libsodium.so",
  "cardanoCLI":       "cardano-cli",
  "nodeStatsURL":     "http://127.0.0.1:12798/metrics"
}
```
https://epoch-api.crypto2099.io:2096/epoch/222

1.上記からeta0の値をコピーして、epochNonceに貼り付ける
2.BPからvrf.skeyを任意のフォルダへコピーする
3.PoolID、各種ファイルパスを修正する（念の為homeからのパスで記述する）
（libsodiumBinaryはそのままでOK）

### 実行する
```bash
node cardanoLeaderLogs.js ~/git/cardano-leader-logs/slotLeaderLogsConfig.json
```
slotLeaderLogsConfig.jsonのパスはご自身の環境に合わせて修正して下さい。
