# Plutus Bitcoin Brute Forcer

A Bitcoin wallet collider that brute forces random wallet addresses

# About This Fork

Updated the chainstate database (March 15 2021)

# Like This Project? Give It A Star

[![](https://img.shields.io/github/stars/SayWhat69/Plutus.svg)](https://github.com/SayWhat69/Plutus)

# Wanna Support Me?

```
BTC: 16vp3EYZoP35MtdquQHxyhxT6ocbNFtw1K
ETH: 0xD7C174Bd0B8b46700e0570a852180D16f3b16aa5
LTC: MT2LsA7LHHsP2H84fhXBKWkU2B4ez7MerZ
```

# Dependencies

<a href="https://www.python.org/downloads/">Python 3.6</a> or higher

Python modules listed in the <a href="/requirements.txt">requirements.txt<a/>
  
Minimum <a href="#memory-consumption">RAM requirements</a>

# Installation

```
$ git clone https://github.com/SayWhat69/Plutus.git plutus

$ cd plutus && pip3 install -r requirements.txt
```

# Quick Start

```
$ python3 plutus.py
```

# Proof Of Concept

A private key is a secret number that allows Bitcoins to be spent. If a wallet has Bitcoins in it, then the private key will allow a person to control the wallet and spend whatever balance the wallet has. So this program attempts to find Bitcoin private keys that correlate to wallets with positive balances. However, because it is impossible to know which private keys control wallets with money and which private keys control empty wallets, we have to randomly look at every possible private key that exists and hope to find one that has a balance.

This program is essentially a brute forcing algorithm. It continuously generates random Bitcoin private keys, converts the private keys into their respective wallet addresses, then checks the balance of the addresses. If a wallet with a balance is found, then the private key, public key and wallet address are saved to the text file `plutus.txt` on the user's hard drive. The ultimate goal is to randomly find a wallet with a balance out of the 2<sup>160</sup> possible wallets in existence. 

# How It Works

Private keys are generated randomly to create a 32 byte hexidecimal string using the cryptographically secure `os.urandom()` function.

The private keys are converted into their respective public keys using the `starkbank-ecdsa` Python module. Then the public keys are converted into their Bitcoin wallet addresses using the `binascii` and `hashlib` standard libraries.

A pre-calculated database of every P2PKH Bitcoin address with a positive balance is included in this project. The generated address is searched within the database, and if it is found that the address has a balance, then the private key, public key and wallet address are saved to the text file `plutus.txt` on the user's hard drive.

This program also utilizes multiprocessing through the `multiprocessing.Process()` function in order to make concurrent calculations.

# Efficiency

It takes `0.0032457721` seconds for this progam to brute force a __single__ Bitcoin address. 

However, through `multiprocessing.Process()` a concurrent process is created for every CPU your computer has. So this program can brute force addresses at a speed of `0.0032457721 ÷ cpu_count()` seconds.

# Database FAQ

An offline database is used to find the balance of generated Bitcoin addresses. Visit <a href="/database/">/database</a> for information.

# Expected Output

Every time this program checks the balance of a generated address, it will print the result to the user. If an empty wallet is found, then the wallet address will be printed to the terminal. An example is:

>1Kz2CTvjzkZ3p2BQb5x5DX6GEoHX2jFS45

However, if a wallet with a balance is found, then all necessary information about the wallet will be saved to the text file `plutus.txt`. An example is:

>hex private key: 5A4F3F1CAB44848B2C2C515AE74E9CC487A9982C9DD695810230EA48B1DCEADD<br/>
>WIF private key: 5JW4RCAXDbocFLK9bxqw5cbQwuSn86fpbmz2HhT9nvKMTh68hjm<br/>
>public key: 04393B30BC950F358326062FF28D194A5B28751C1FF2562C02CA4DFB2A864DE63280CC140D0D540EA1A5711D1E519C842684F42445C41CB501B7EA00361699C320<br/>
>address: 1Kz2CTvjzkZ3p2BQb5x5DX6GEoHX2jFS45<br/>

# Memory Consumption

This program uses approximately 2GB of RAM per CPU. Because this program uses multiprocessing, some data gets shared between threads making it difficult to accurately measure RAM usage.

![Imgur](https://i.imgur.com/9Cq0yf3.png)

The memory consumption stack trace was made by using <a href="https://pypi.org/project/memory-profiler/">mprof</a> to monitor this program brute force 10,000 addresses on a 4 logical processor machine with 8GB of RAM. As a result, 4 child processes were created, each consuming 2100MiB of RAM (~2GB).

# Calculation of the probability of finding an address with a balance
  
Let's talk about math :
  I've 8 cores running with the program.
  0.0032457721/8 = 0.000405722
  it needs 0.000405722 sec to get one address
  So for one sec, i'll found 2464.741867584 addresses 1/0.000405722
  Convert in one day : 212953697.359275563 addresses a day
  Convert in one month : 6.3886E9
  Convert in one year : 7.7728E10
  There's 2^160 = 1.4615E48 addresses in the whole blockchain. (10^82 = number of atoms in the universe)
  If we divide the number of addresses possible per the number of addresses found in a day : 2^160/212953697.359275563 = 6.863001936×10³⁹
  So It would take 6.863001936×10³⁹ days to find all the address in the blockcahin. But we don't want the whole blockchain. We only want the adresses who are in the
  database.
  we only have 33M addresses which are interesting.
  In fact, we have 33165253×100÷(1.4615×10⁴⁸)×100 per cent of chance to get an interesting address per day
  Response : I have :
  2.269258696E-37% chance to get an address per day
  6.8076E-36 per month
  8.2E^-35% per year
  To have 1% chance of having an address, I have to scan 1% of the blockchain = 1,46E46 address
  It would take 1.878E35 year to have 1% chance having an address
  these calculations do not take into account the chance, so if you want to use this program, I hope you're lucky in real life.
  PS : If you find some faults, i'm open
  

# Recent Improvements & TODO

- [X] Fixed typos/formatting

- [ ] Update database

- [ ] Pickle loader

- [ ] Try to fix Memory Error

<a href="https://github.com/SayWhat69/Plutus/issues">Create an issue</a> so I can add more stuff to improve
