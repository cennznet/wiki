For Windows, Cennznet may require the Linux subsystem to build.

### 1. Enable Linux Subsystem
Open your Windows PowerShell (you may need to run using Administrator Privilege), run this command:

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```


### 2. Install Ubuntu
Go to Microsoft Store and search for “Linux”, then select “Ubuntu”. Click Install
Ubuntu will finish installing after a while and prompt you to create a new Username and Password.

Now you should be able to use Ubuntu via the CLI. To access your HDD, use 

```
$cd /mnt/<drive letter>/<path>
```

Note: To be able to “paste” commands into your Ubunto shell, right-click on the Windows border to open up a menu, select “Properties”, and tick “Use Ctrl+Shift+C/V as Copy/Paste”.
Selecting “Quick Edit Mode” allows you to paste by right-clicking the console window.

### 3. Update the native package manager

```
sudo apt update
```

### 4. Install required tool-chain for Linux

```
sudo apt install -y cmake pkg-config libssl-dev git gcc build-essential clang libclang-dev
```

Now your Linux is ready to install Rust.

### 5. Install and set up Rust

1. Install Rust on your machine [here](https://rustup.rs/) 
2. Use Rust stable by default:
```
rustup default stable
```
3. Install the following nightly toolchains:
```
rustup install nightly
rustup target add --toolchain=nightly wasm32-unknown-unknown
```