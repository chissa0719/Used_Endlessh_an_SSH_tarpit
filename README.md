# Used_endless_SSH
<br>
endless_SSHの話題を耳にして、使いたくなったのでやってみた。<br>
<br>

> [!CAUTION]
> 実行環境は，仮想マシン(VMware): Ubuntu22.04


## 実行する

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
> `endless/util/smf/endless.conf`<br>
> とりあえず動かすなら，`Port 22`の部分を変更する

```
# vim util/smf/endless.conf

```

```
# The port on which to listen for new SSH connections.
Port 22

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

長時間返事か返ってこなければ，成功と見て問題ないはず(？)<br>
<br>
インターネットでは，上記の実行で700分レスポンスがなかったそう...<br>
ちなみに，なんとなくDelayを1にしても10分以上はレスポンスがなかった<br>
(Delayでいじるところが合っているかは不明)<br>
<br>

