# Utilities for [Flashbots](https://github.com/flashbots/pm)

* Go API client for the [Flashbots blocks API](https://blocks.flashbots.net/): [`flashbotsapi/flashbotsapi.go`](https://github.com/metachris/flashbots-failed-tx/blob/master/flashbotsapi/flashbotsapi.go)
* Tool to detect failed Flashbots as well as other 0-gas transactions (can run over history or in 'watch' mode, webserver that serves recent detections)

Uses:

* https://github.com/metachris/go-ethutils
* https://github.com/ethereum/go-ethereum

Notes:

* You should use IPC connections to the geth node, as there are a lot of API calls (one for each tx).

---

## Detecting 0-gas and Flashbots failed transactions

There's a Telegram bot which publishes the latest of those failed transactions: https://t.me/FlashbotsBot ([GitHub](https://github.com/metachris/flashbots-tx-telegram-bot))

Notes:

* You should use an IPC connection to the geth node, as there are a lot of API calls (one for each tx).


### Getting started

Set the geth node URI as environment variable `ETH_NODE`, or pass it in as `-eth` argument.

```bash
# Build
go build -o flashbots-failed-tx cmd/flashbots-failed-tx/main.go

# Subscribe to new blocks and find failed Flashbots tx:
./flashbots-failed-tx -watch
./flashbots-failed-tx -watch -silent  # print only failed transactions

# Historic, using a starting block
./flashbots-failed-tx -block 12539827           # 1 block
./flashbots-failed-tx -block 12590513
./flashbots-failed-tx -block 12539827 -len 5    # 5 blocks
./flashbots-failed-tx -block 12539827 -len 10m  # all blocks within 10 minutes of given block
./flashbots-failed-tx -block 12539827 -len 1h   # all blocks within 1 hour of given block
./flashbots-failed-tx -block 12539827 -len 1d   # all blocks within 1 day of given block
./flashbots-failed-tx -block 12539827 -len 1d -silent  # don't print information for every block

# Historic, using a starting date
./flashbots-failed-tx -date 2021-05-31 -len 1d  # all blocks from this day (00:00:00 -> 23:59:59 UTC)
./flashbots-failed-tx -date 2021-05-31 -hour 3 -min 53 -len 5m  # all blocks within 1 hour of given date and time (UTC)
./flashbots-failed-tx -date -1d -len 1h         # all blocks within 1 hour of yesterday 00:00:00 (UTC)
```

### Installing`flashbots-failed-tx` without cloning the repo

You can install this tool as `flashbots-failed-tx` binary without cloning the repository:

```bash
go install github.com/metachris/flashbots-failed-tx/cmd/flashbots-failed-tx@master
flashbots-failed-tx -h
```

## Various

![Screenshot](https://user-images.githubusercontent.com/116939/120549797-532fa500-c3f4-11eb-84fc-1e02d1db4cd6.png)

Interesting blocks:

* `12539827` - 1 failed 0-gas tx
* `12590513` - ~100 failed 0-gas tx
* `12527162` - failed Flashbots tx

---

## Feedback

Please reach out to [twitter.com/metachris](https://twitter.com/metachris).
