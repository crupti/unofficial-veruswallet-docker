# Unofficial Veruswallet for Docker (Depricated)

__This old repository is now depricated, please do not use. Now only here for informational purposes.__

## License

MIT License

Copyright (c) 2018 crupti

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Warning

ALWAYS BACKUP YOUR DOCKER CONTAINER VERUSCOIN WALLET AND PRIVATE KEYS REGULARLY ON A SEPERATE STORAGE DEVICE - NOT DOING SO MAY CAUSE YOU TO LOSE YOUR VERUSCOIN IF YOUR DOCKER CONTAINER IS REIMAGED OR OTHERWISE DELETED OR MALFUNCTIONS! NEVER SHARE YOU WALLET OR PRIVATE KEYS WITH ANYONE !

## Introduction

This repository once contained an unofficial VerusCoin wallet for Docker, but is now depricated. This repository is now only here to help any old users of the Docker image.

## Examples for Ubuntu Linux users

Note: The '\' characters are only usefull if you want to run multi-line commands, you must remove them if you run the commands on a single line otherwise there will be errors.

Please see to the official VerusCoin wallet documentation for how to run verus CLI commands, these examples are only for guidance and __does not__ cover all situations like multiple accounts etc.

### Starting the wallet interactively

WARNING: ALWAYS BACKUP WALLETS AND ALL PRIVATE KEYS __BEFORE__ REIMAGING DOCKER CONTAINERS - OTHERWISE YOU WILL LOOSE YOUR VERUSCOIN AS THE WALLET IN THE OLD DOCKER CONTAINER WILL BE DELETED!

```console
$ sudo docker run -ti --name veruswallet crupti/veruswallet
```

### Starting the wallet with automatic restart after host reboot

WARNING: ALWAYS BACKUP WALLETS AND ALL PRIVATE KEYS __BEFORE__ REIMAGING DOCKER CONTAINERS - OTHERWISE YOU WILL LOOSE YOUR VERUSCOIN AS THE WALLET IN THE OLD DOCKER CONTAINER WILL BE DELETED!

```console
$ sudo docker run --restart always -i -d -h veruswallet \
   --name veruswallet crupti/veruswallet 
```

### Adding an alias for the verus command

Step #1: Since we want the verus command to be accessible from the Docker Host, we'll need to add an alias inside your ~/.bashrc file, place the following line at the bottom of your .bashrc file:

```console
$ alias verus='sudo docker exec -ti veruswallet verus'
```

Step #2: Run the following command to load the new alias into your current bash shell.

```console
$ source ~/.bashrc
```

Step #3: Test the alias with the following command which also shows verus command help:

```console
$ verus help
```

Note: Please see to the official verus command for more information about how to use verus CLI commands.

## Taking a backup of the wallet that's in the container

Step #1: First you'll have to tell the Docker container to dump the wallet to it's /tmp/verusexport/main folder which is inside the Docker Container itself.

```console
$ verus dumpwallet walletbackupfile
```

Step #2: Then you have to copy the wallet to your local folder, always make sure that this folder is private and only accessible to yourself as you don't want others to access your wallet file.

```console
$ sudo docker cp veruswallet:/tmp/verusexport/walletbackupfile .
```

Now you should have a file called 'walletbackupfile' in your folder.

Step #3: Now that you hopefully have your backup locally in the folder that you ran your commands from, you should delete the backup inside the Docker container for security reasons:

```console
$ sudo docker exec veruswallet rm /tmp/verusexport/walletbackupfile
```

## Getting a private key from the wallet inside the Docker container

Note: Your wallet may contain multiple private keys, each tied to a public key, always make sure that you backup all your private keys.

```console
$ verus dumpprivkey "myaddress"
```

## Importing a private key into the wallet in the Docker container

```console
$ verus importprivkey "mykey"
```

## Overwriting existing wallet in Docker image with another wallet 

WARNING: MAKE SURE THAT YOU HAVE BACKED UP YOUR WALLETS AND YOUR PRIVATE KEYS BEFORE OVERWRITING YOUR WALLET IN YOUR DOCKER CONTAINER, OTHERWISE YOU MAY LOSE YOUR VERUSCOIN!

Run the following command from where your wallet.dat file is located:

```console
$ sudo docker cp wallet.dat veruswallet:/home/verus/.komodo/VRSC/wallet.dat
```

## Checking your balance 

```console
$ verus getbalance
```

## Sending VerusCoin to another address

Sending 0.1 VerusCoin.

```console
$ verus sendtoaddress "VRSC_address" 0.1 "Some comments here"
```

## Listing latest transactions

```console
$ verus listtransactions
```

## Troubleshooting: Showing live logging from a running Docker container in your console

```console
$ sudo docker logs veruswallet -f
```
CTRL+C to stop following the log.
