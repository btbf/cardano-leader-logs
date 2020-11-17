# cardano-leader-logs
https://github.com/MarcelKlammer/cardano-leader-logs  

以下の内容はリレーノードでも実行可能です。　　

## 1.任意のノードでnode.jsとpython3と関連パッケージをインストールする
https://github.com/MarcelKlammer/cardano-leader-logs/blob/main/README.md  
nodejsとpythonの項目を実行する  
上記マニュアル内のpythonバージョン確認でエラーが出る場合は
```bash
python3 --version
```

## 2.Githubからクローンを作成する
```bash
cd $git #←任意のフォルダでOK
git clone https://github.com/MarcelKlammer/cardano-leader-logs
cd cardano-leader-logs
```

## 各ファイルを編集する

### 1.設定ファイルを作成する
```bash
cp example.leaderlogs.json slotLeaderLogsConfig.json
nano slotLeaderLogsConfig.json
```

```bash
{
  "poolId":           "00000000000000000000000000000000000000000000000000000000",
  "vrfSkey":          "/path/to/vrf.skey",

  "genesisShelley":   "/path/to/mainnet-shelley-genesis.json",
  "genesisByron":     "/path/to/mainnet-byron-genesis.json",

  "ledgerState":      "",

  "libsodiumBinary":  "/usr/local/lib/libsodium.so",
  "cardanoCLI":       "cardano-cli",
  "nodeStatsURL":     "http://127.0.0.1:12798/metrics",
  
  "timeZone":         "Asia/Tokyo"
}
```
1.PoolID、各種ファイルパスを修正する（念の為homeからのパスで記述する）  
（libsodiumBinaryはそのままでOK）  
2.リレーノードで実行する場合は、BPからvrf.skeyを任意のフォルダへコピーする  
  

### 実行する
```bash
node cardanoLeaderLogs.js ~/git/cardano-leader-logs/slotLeaderLogsConfig.json epochNoneHash [optional: 1] [optional: additionalDParameter, eg. 0.0]

ex 222エポックの場合)
node cardanoLeaderLogs.js ~/git/cardano-leader-logs/slotLeaderLogsConfig.json 171625aef5357dfccfeaeedecd5de49f71fb6e05953f2799d3ff84419dbef0ac

過去のエポックを出力する場合 ex. 228）
node cardanoLeaderLogs.js ~/git/cardano-leader-logs/slotLeaderLogsConfig.json a21f1bc2eafb9dc040ba56453ed7f5a8dc00366d5941b28f47655cb309177e08 1 0.48
```
https://epoch-api.crypto2099.io:2096/epoch/  
1.eta0の値をコピーして、epochNoneHash　部分に貼り付ける  
slotLeaderLogsConfig.jsonのパスはご自身の環境に合わせて修正して下さい。

2.過去エポックを参照する場合は、dパラメータも付与してください。

### 出力
初回出力時のログ状況
```bash
Loading ledger state: /home/btbf/git/cardano-leader-logs/ledgerstate.json
Could not load ledger state from config. Trying to generate new lederstate.json
Loading protocol parameters
```
この表示になるとスロットリーダー割り当て時間が表示されるが
取得されるまで10分ぐらいはかかる

### 注意
出力されたスケジュールは、外部に公開しないようにし、  
自身のサーバにおけるノードアップデートのタイミングを知るものとして活用するようにしてください。
