# Used_Endlessh_an_SSH_tarpit
<br>
Endlessh: an SSH tarpit の話題を耳にして，使いたくなったのでやってみた．<br>
<br>
ランダムに<b>超ゆっくりと</b>SSHバナーを延々と送信することで，攻撃者のスタック化を狙っているっぽい？<br>
(英語微妙です．指摘などあればお願いします．)<br>
<br>
作成者はこちら → https://github.com/skeeto/endlessh<br>
<br>

> [!CAUTION]
> 実行環境は，仮想マシン(VMware) : Ubuntu22.04


## 実行手順

```
# apt update
# apt upgrade
```

```
# git clone https://github.com/skeeto/endlessh
# cd endless/
# git log | head -1
# make
```

> [!WARNING]
> 実行環境やコマンドに応じて，configの設定が必要<br>
> configはここ → `endless/util/smf/endless.conf`<br>
> とりあえず動かすなら`Port 22`の部分を変更する<br>
> 以下に示すのは，22222番で実行する場合のconfig設定

```
# vim util/smf/endless.conf
```

```
# The port on which to listen for new SSH connections.
Port 22222

# The endless banner is sent one line at a time. This is the delay
# in milliseconds between individual lines.
Delay 10000

# The length of each line is randomized. This controls the maximum
# length of each line. Shorter lines may keep clients on for longer if
# they give up after a certain number of bytes.
MaxLineLength 32

# Maximum number of connections to accept at a time. Connections beyond
# this are not immediately rejected, but will wait in the queue.
MaxClients 4096

# Set the detail level for the log.
#   0 = Quiet
#   1 = Standard, useful log messages
#   2 = Very noisy debugging information
LogLevel 1

# Set the family of the listening socket
#   0 = Use IPv4 Mapped IPv6 (Both v4 and v6, default)
#   4 = Use IPv4 only
#   6 = Use IPv6 only
BindFamily 0
```

```
# ./endlessh -v -p22222 &
```

## 別コンソールを起動し，以下コマンドを入力

```
# time ssh localhost -p 22222
```

長時間返事が返ってこなければ成功<br>
<br>
インターネットの記事では，上記の実行で<u>700分レスポンスがなかった</u>そう...<br>
ちなみに，なんとなくDelayを1にしても10分以上はレスポンスがなかった<br>
(Delayでいじるところが合っているかは不明)<br>
<br>

