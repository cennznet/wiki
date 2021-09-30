# Dev environment setup

## Creating an account

You will need to have an account to create and sign transactions.
The easiest way is to use the [CENNZnet Portal](https://cennznet.io/). 

[Here](CENNZnet-infrastructures/Exploring-the-CENNZnet-UI) are the guides to help you get familiar with it!
Alternatively you can use the [Browser Extension](CENNZnet-infrastructures/CENNZnet-browser-extension) to create an account.

### Development notes
The account you created can be used on both [the MainNet and the TestNet](Getting-started/CENNZnet-networks). You can search for the transactions related to the account using [UNcover](CENNZnet-infrastructures/Uncover). To view transactions on the TestNet(Nikau), make sure you select Nikau or use [this URL](https://uncoverexplorer.com/?network=Nikau) to access UNcover.

To issue yourself tokens to use on the TestNet, you can use the [Faucet](CENNZnet-infrastructures/CENNZnet-faucet).

## Depedencies for the CENNZnet API
Install the latest stable versions of the following,

* [Node.js (`>=10.16.3`)](https://nodejs.org/en/) 
* [Yarn (`>= 1.19.0`)](https://yarnpkg.com/getting-started/install)

## Depedencies for running a local dev node
* [Docker desktop](https://www.docker.com/products/docker-desktop)

### Windows specific depedencies for running a local dev node

You will need Windows 10 which supports the Linux subsystem. This is used by Docker.

#### Additional tools and packages
* [Git](https://git-scm.com/download/win)

* [Download the Linux kernel update package](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package)

Restart the computer after you've completed the above steps!

#### Trouble shooting
Run the docker desktop app to see if it gives errors. It's likely to prompt you to go through a few steps. 

You may need to enable hardware assisted virtualization following the steps [here](https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization-must-be-enabled).


## CENNZnet development

### Rust
The recommended way to install Rust is to follow the official instructions [here](https://www.rust-lang.org/tools/install).

### Windows Subsystem for Linux
Note that if you want to build Cennznet locally(instead of running a docker image) on Windows, you may need to use the Linux subsystem. You can follow the instructions here to [Setting up Linux subsystem on Windows](CENNZnet-development/Guides/Set-up-Linux-Sub-system-for-Windows).

## Next steps

Head to the guide [How to build a DApp](Dapp-development/Guides/How-to-build-a-DApp) to get started!