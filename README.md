# JCE Cryptonote CPU Miner
Welcome to the Fastest Cryptonote CPU Miner ever!

BitcoinTalk Topic: https://bitcointalk.org/index.php?topic=3281187.0

### Is that a Virus? No!
Like all miners, JCE gets detected as a virus/trojan by most Antiviruses, including Windows Defender. But it's not. Read more about Privacy and Security below.

### Is it just yet-another fork of a common miner? No!
You're not losing your time testing a made-up rip of a common miner, JCE is brand new, using 100% new code.

### Are the new Monero-V7, Cryptolight-V7, Cryptonight-Heavy, IPBC, Alloy and XTL forks supported? Yes!
The *--variation* parameter let you choose the fork. More details below.

# Index

* [Speed](#speed)
* [Getting started](#getting-started)
* [Third Party integration](#third-party-integration)
* [Basic topics](#basic-topics)
* [Advanced topics](#advanced-topics)
* [Cryptonight Forks](#cryptonight-forks)
* [Configuration](#configuration)
* [Large Pages](#large-pages)
* [Privacy and Security](#privacy-and-security)

## Speed

In short, JCE is:
* Crazy fast on non-AES 64-bits, usually 35-40% faster than other miners
* Compared to other 32-bits miners, still faster on non-AES 32-bits, sometimes beating even the other miners 64-bits versions
* And still comparatively faster on non-AES 32-bits Cryptonight-Heavy, with usually +50% speed.
* Barely faster than the other best on AES 64-bits, beating them by ~1%, +2.8% on V7 fork, +4% on Cryptonight-Heavy
* Also a lot faster on AES 32-bits, but it's a rare case (mostly seen on Intel Atom tablets)

Here's a benchmark against three other common miners.\
**The test is fair**: run on the exact same Win10 Pro computer, all Huge Pages enabled, no background task, best configuration.
* XMRStak means: the released Unified binary from github (not recompiled myself)
* XMRig means: the respective best released binary *gcc* (32-bits) and *msvc* (64-bits) from github (not recompiled myself)
* Claymore means: best Claymore CPU (3.4 for 32-bits, 3.9 for 64-bits)
* When not supported, score is zero, if not tested yet, score is *?*
* Fees are included in the score


##### Core2 Quad 2.666 GHz, 4 threads, 64-bits, Cryptonight
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
116 | 80 | 85 | 57

##### Core2 Quad 2.666 GHz, 4 threads, 32-bits, Cryptonight
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
93 | 0 | 68 | 50

##### Ryzen 1600, 8 threads, 64-bits, Cryptonight
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
506 | 502 | 502 | 443

##### Ryzen 1600, 8 threads, 32-bits, Cryptonight
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
434 | 0 | 327 | 275

##### Ryzen 1600, 8 threads, 64-bits, Cryptonight V7
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
503 | 492 | 491 | ?

##### Ryzen 1600, 8 threads, 32-bits, Cryptonight V7
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
424 | 0 | 320 | ?

##### Ryzen 1600, 4 threads, 64-bits, Cryptonight Heavy
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
252 | 169 | 250 | 0

##### Ryzen 1600, 4 threads, 32-bits, Cryptonight Heavy
JCE | XMRStak | XMRig | Claymore
------------ | ------------- | - | -
191 | 0 | 174 | 0

## Getting started

If you're new at mining Cryptonight, here's the simplest way:

1. Choose the coin to mine, see the list below. The most common is Monero.
2. Get a wallet, that's a ~95 character long identifier. If you don't have one yet, you can create it [here](https://cn-wallet-generator.hashvault.pro/monero-wallet-generator.html)
3. Choose a pool to mine on, and its port. For example Pool *pool.minexmr.com* and port *4444*
4. Edit the *start.bat* that's shipped in the .zip
	1. Change the example POOL by yours
	2. Change the example PORT by yours
	3. Change the example WALLET by yours
	4. You can leave the default password *x*
5. (Optional) If your coin is *exotic*, maybe you also need to change FORK=0 to another number. See the list in the *start.bat*
6. Run start.bat


## Third Party integration

If you're a Mining Tool dev (like Forager, Awsome Miner...) and want to integrate JCE, here's a good command to spawn JCE.
Most parameters are similar to other common miners.

```
jce_cn_cpu_miner64.exe --auto --any --forever --variation FORK --low -o POOL:PORT -u WALLET -p PASSWORD --mport MONITOR SSL
```

And replace:
* FORK by the Fork number, see list below, or set 0 for automatic
* POOL:PORT by the pool address (name or IP):(port), e.g. pool.minexmr.com:4444
* WALLET by the miner wallet
* PASSWORD by the miner password
* MONITOR by the local HTTP monitor server port
* SSL by either nothing, or "--ssl" if SSL is to be used

To monitor the miner, read http (not https!) at localhost:MONITOR and you'll get some simple JSON like

```
{
  "hashrate":
  {
    "thread_0": 13.75,
    "thread_1": 18.29,
    "thread_2": 21.19,
    "thread_3": 18.85,
    "total": 72.06
  },
  "result":
  {
     "pool": "pool.minexmr.com:4444",
     "ssl": false,
     "currency": "Monero (XMR/XMV/XMC/XMO)",
     "difficulty": 23684,
     "shares": 5,
     "hashes": 84473,
     "uptime": "0:08:28",
     "effective": 166.29
  }
}
```

Quite self-explanatory,\
*"effective"* is the net effective hashrate, fees, outdated and invalid shares deduced,\
*"total"* is the instant physical hashrate

#### XMRStak mode

If your mining tool does not support JCE yet, you can get a XMR-Stak compatible json by adding parameter --stakjson\
In such case, the JSON will be like

```
{"version":"jce/cpu/0.27c","hashrate":{"threads":[[28.0,28.0,28.0],[25.7,25.7,25.7],[26.3,26.3,26.3],[26.5,26.5,26.5]],"total":[106.5,106.5,106.5],"highest":106.5},"results":{"diff_current":15000,"shares_good":1,"shares_total":1,"avg_time":12.0,"hashes_total":15000,"best":[118228,0,0,0,0,0,0,0,0,0],"error_log":[]},"connection":{"pool": "pool.minexmr.com:4444","uptime":12,"ping":0,"error_log":[]}}
```

## Basic topics

#### Q. Is it free (as in beer, as in freedom)?
No and no. It has fees, and is not open source. But the program itself is free to distribute.

#### Q. How much cost the fees?
Current fees are:
* 3.0% when using at least one mining thread with non-AES architecture, or 32-bits
* 1.5% when using only 64-bits AES architecture

The fees are twice higher in non-AES mode and/or 32-bits because JCE offers a huge performance gain here.

#### Q. Can I avoid fees?
Not really. I plan to offer a paying per-licence-no-fee (pay-once-for-all) version, but it's a lot more complicated to set up than a fee-based miner.\
Also, JCE never takes any fee during the first minute, so if you run it, and kill it after one minute, and repeat again and again, then you'll never pay any fee, but JCE takes a few seconds to start, and your Pool probably won't let your reconnect continuously.

#### Q. Will it work on my computer?
Minimum is Windows Vista 32-bits, with a SSE2 capable CPU. 64-bits is faster, prefer it.\
For best performance, Huge Pages must be enabled, JCE will try to auto-configure them, but it may work or not depending on your Windows version and security configuration.

#### Q. What currency can I mine? On which pools?
You can mine any coin on any pool.\
If your coin is listed, all is automatic.\
Run the miner with *--coins* parameter to get the up-to-date list. Current list is:
* Monero (XMR/XMV)
* Electroneum (ETN)
* Karbowanec (KRB)
* Bytecoin (BCN)
* Sumokoin (SUMO)
* Bitcoal (COAL)
* Bitcedi (BXC)
* Dinastycoin (DCY)
* Leviarcoin (XLC)
* Fonero (FNO)
* Turtlecoin (TRTL)
* Graft (GRFT)
* Dero (DERO)
* Stellite (XTL)
* UltraNote (XUN)
* Intense (INTS)
* Crepcoin (CREP)
* Pluracoin (PLURA)
* Haven (XHV)
* FreelaBit (FBF)
* BlueberriesCoin (BBC)
* B2BCoin (B2B)
* Bitsum (BSM)
* SuperiorCoin (SUP)
* EDollar (EDL)
* Interplanetary Broadcast (IPBC)
* Masari (MSR)
* Alloy (XAO)
* BBSCoin (BBS)
* BitcoiNote (BTCN)
* Elya (ELYA)
* Iridium (IRD)
* Italo (ITA)
* Lines (LNS)
* Niobio (NBR)
* Ombre (OMB)
* Solace (SOL)
* Triton (TRIT)
* Truckcoin (TRKC)
* Qwertycoin (QWC)
* Loki (LOK)
* Gadcoin (GAD)
* MarketCash (MKT)
* Nicehash Cryptonight v7
* Minergate Cryptonight
* MiningPoolHub Cryptonight
* MiningRigRentals Cryptonight v7
* Suprnova Cryptonight

Otherwise, if your coin is not listed, or your wallet not recognized, use the __--any__ parameter, plus the __--variation N__ parameter, with N the fork number, see list below.
The fork detection is automatic on known coins, but manual on unknown coins. The coin list is periodically updated.

#### Q. Is Nicehash supported?
Yes, see list above. The Nicehash-specific Nonce is then automatically enabled.

#### Q. Is SSL supported?
Yes, with parameter *--ssl*

#### Q. I get only bad shares, what happens?
Your coin has probably forked. Add --variation N parameter, with N as listed below, until you find the one that works.

#### Q. What if my wallet is not recognized, or as a different coin?
Some coins use a wallet syntax so close that they're hard to differenciate, like Lines and Loki. If JCE fails to detect the coin, force it with *--any --variation N* (with N as listed below) and let the miner run. It will still display the wrong coin but mine the good one. And of course proof-check pool side that you correctly get the shares.

#### Q. Is there a HTTP server to monitor the miner?
Modern pools provide all you need to monitor your miners (average hashrate, worker-id...). Monitoring is now a pool's job. Still, a minimal HTTP Json server is available with parameter --mport P (P the port number) to ease integration of JCE into mining tools, but not intended for human reading. [Forager](http://bit.ly/2wagBKX) was the first tool to integrate JCE, take a look!\
For more compatibility, with extra parameter --stakjson, the JSON will be in XMR-Stak format.


## Advanced topics

#### Q. Are there requirements or dependencies?
No. JCE is just a big standalone .exe

#### Q. Is there a Linux version?
Not yet.

#### Q. Is there a GPU version?
Not yet.

#### Q. Is there a 32-bits version?
Yes, both 32 and 64 are always in the same release.

#### Q. Is low-power mode supported?
That's how other miners call the multi-hash mode. I call it multi-hash because it's about computing several hashes at the same time, barely related to cpu voltage. And yes it's now supported, with max multi = 6.

#### Q. Do I get a discount on fees if I use SSL?
I'm not Claymore.

#### Q. How is developed JCE?
The network and stratum handling is C++14, and the mining algos are assembly (to be precise, GNU Extended Assembly). Hence the speed increase.

#### Q. Can I plug it to a stratum proxy?
No, it must mine on a real pool on Internet.

#### Q. My Internet connection is bad, can JCE always reconnect instead of giving up after 5 attempts?
Yes, add parameter *--forever*

#### Q. Is it really new? It looks familiar to me...
Yes it is. But it reuses, on purpose, some de-facto conventions from other common miners, like a XMRStak-style cpu configuration, optional XMRStak-style Json monitoring and the colors of Claymore (green=share, red=error, blue=hashrate, yellow=status).

#### Q. How is the hashrate calculated?
That's the average speed of the last 512 hashes (not *shares* found, computed *hashes*), rounded at 0.01. And it's fair, the displayed number has no tweak, and includes the fees. The total is first summed from exact per-thread values, then rounded (said differently, it's a rounded sum, not sum of rounded).

#### Q. Can I get a long-time speed average?
Better look at your pool's reports, but JCE also gives the average effective net hashrate when pressing R. It's usually slightly lower than the physical hashrate because of outdated shares and fees.

#### Q. Can I do pool auto-switch in case of failure? Or periodically?
Not directly, but the *-q* and/or the *--autoclose* parameters, with the help of a simple .bat, can do the job. The packaged .zip comes with an exemple, edit it to fit your needs.

#### Q. Can I mix architectures when mining (i.e. thread 1 uses core2, thread 2 uses pentium4)?
It sounds strange, but yes. However, that's mostly useful for tests.

#### Q. Can I mix coins when mining (i.e. thread 1 mines XMR, thread 2 mines ETN)?
No.

#### Q. Can I mix simple-hash and multi-hash?
Yes, and it's a very common case when mining TurtleCoin or IPBC.

#### Q. What is "use_cache":false useful for?
The no-cache mode means the cache is mostly bypassed, depending on your hardware. When using a lot of cache but few cores (typically when mining Cryptonight-Heavy) assigning unused physical cores to no-cache mining can give you a few extra h/s for free. Be careful, mixing cache and no-cache of *logical* CPUs of the same *physical core* causes terrible performance on AMD CPUs, while it's good on Intel ones.

#### Q. What a great job! Can I make a donation?
Thanks bro. You can, with the *--donate* parameter which raise the fees to 80%, or by sending coins to the donation wallet (the one in the start.bat file included).

## Cryptonight Forks

All current forks are supported:
* N=0 *Automatic*
* N=1 Original Cryptonight
* N=2 Original Cryptolight (for AEON)
* N=3 Cryptonight V7 fork of April-2018
* N=4 Cryptolight V7 fork of April-2018
* N=5 Cryptonight-Heavy
* N=6 Cryptolight-IPBC
* N=7 Cryptonight-XTL
* N=8 Cryptonight-Alloy

The current *Automatic* mode **behaves the old way on alt-coins**:
* Monero, Monero-V, Graft and Intense are now Cryptonight V7,
* SuperiorCoin, BBSCoin and Lines are Cryptonight V7 too,
* Sumokoin, Loki, Ombre and Haven are now Cryptonight-Heavy,
* Aeon is still Cryptolight
* TurtleCoin is now Cryptolight V7
* Interplanetary Broadcast has is own Cryptolight-IPBC
* Stellite has is own Cryptonight-XTL
* Alloy has is own Cryptonight-Alloy
* Everything else is still assumed Cryptonight

More will be updated as more coins forks.

To force one of those forks, set the *--variation N* parameter, with N as stated above.

## Configuration

Almost everything is configured with command-line parameters. The config file is for cpu fine tuning only. See the embedded .bat for an example.\
Mandatory parameters are:
* *-u* the Wallet/Login
* *-p* the password ("x" usually works)
* *-o* the pool:port
* *--auto* or *-c* for CPU configuration

Important extra parameters are:
* *--ssl* if you use SSL
* *--low* not to freeze your PC if you mine with all cores
* *--variation* to use one of the new Cryptonight forks

Type --help to get the complete list.

### Super Easy CPU configuration

Use --auto and you're good.

### Normal Easy CPU configuration

Use --auto with:
* *--archi* to set the CPU architecture (if you force SSE4 or AES on a CPU with no support, it will crash).
* and/or *-t* to set the number of threads.

The list of architectures is in the config.example.txt file in the Zip.

### Advanced CPU configuration

Use -c <configfile>\
See the config.example.txt file in the Zip for details.

### Dual-thread mining

This is an exclusive feature of JCE!\
It is **not** like the double-hash, with one thread on two hashes. That's two threads on two hashes, but sharing the cache and the Huge Page, for CPUs with very low cache.\
The principle is closer to Claymore Dual: use the *smooth* parts of the Cryptonight algo to let another thread use the cache and memory, then take it back. It allows the main thread to run at ~90% and the side thread at ~25%, totaling a speed increase of ~15%.\
However, if this mode is powerful, it offers a gain only on some rare, old CPU:
* With no hardware AES (hardware AES is so fast that the master thread has no *time* to share)
* Not using Cryptonight-Heavy (it's so... heavy that the master thread has no *resource* to share)
* With low cache but decent compute power (read: no Atom or antique P4)

Remains some entry-level Core2, Athlons and Celeron/Pentium.

### Multi-hash

To be explained...

## Large Pages

To be explained...

## Privacy and Security

#### Q. Is it a virus?
No. There's no malicious code at all, but since the source code is closed, I cannot prove it. The best I can do is to give a complete list of what the program does and doesn't.

#### Q. So, what does it do or not?
It does:
* Read the configuration file, if asked to (parameter -c)
* Scan your CPU and cache to autoconfigure and/or check the manual configuration
* Connect to pools on Internet to mine
* Write the log, if asked to (parameter --log)
* Try to autoconfigure the Huge Pages privileges if they're not working at first. That's the only *intrusive* action, but when it does it, it says so.
* Start a local HTTP server to get monitored, if asked to (default: disabled)

It doesn't:
* Write anything to your computer, except its own log, when enabled (default is disabled)
* Send any information, to me nor anywhere else
* Identify your computer or miner instance, not even using a hash
* Punch through your firewall: you have to open it manually if needed
* Run any command, not even *attrib* (see below)

#### Q. I see the JCE process punching the attrib command, what is it doing?
JCE does never run attrib, nor any other command, but it disguises its mining process into a *attrib* to avoid being detected and erased by antiviruses. Again, JCE does nothing malicious, but like all other miners it's detected as a virus so I've to do such a trick. That's the normal behavior of the 64-bits version. I never had the 32-bits detected, so I don't use that trick with it.
