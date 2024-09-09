This is a guide to use rust for arduino development on wsl.  Through this guide we will setup windows wsl (Windows subsystem for linux) for rust development and more specificly for arduino.  Evertyhing will be setup and available in the wsl terminal.

* zsh
* neovim
* rust with lsp


# Useful links

* [Rust Before Main](https://youtu.be/q8irLfXwaFM):  Information about rust binary. 


# WSL

1. Install wsl ubuntu.  `wsl --install` this command this will install a default ubuntu distro.  If you want to install a different distro refer to this [page](https://learn.microsoft.com/en-us/windows/wsl/install) 
2. We also need to change the font of the terminal to be able to use neo vim properly.
    a. Download any [nerdfont](https://www.nerdfonts.com) as required by neovim.
    b. Select all the files
    c. Right click and choose install.
3. Setup window terminal to use this new font.
    a. Open a windows terminal
    b. `Ctrl+,` to open settings
    c. Clik on open json file at the bottom left corner. (This will open the json settings file in the default text editor)
    d. You can create a new or update the existing ubuntu profile
        ```json
            "profiles":
            {
                "list": [
                    {
                        "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                        "hidden": false,
                        "name": "Ubuntu",
                        "source": "Windows.Terminal.Wsl",
                        "colorScheme": "Ubuntu-ColorScheme",  
                        "cursorShape": "filledBox",
                        "font": {
                            "face": "Cousine Nerd Font",  <-- specify the installed nerd font here
                            "size": 12
                        }
                    },
                ]
            }
        ```
    e. restart the terminal and notice the new font.
4. Install [usbipd-win](https://learn.microsoft.com/en-us/windows/wsl/connect-usb).
    a. Connect the arduino to the pc.
    b. list the usb device with `usbipd list`
    c. share arduino bus with wsl `usbipd bind --busid <busid>`  **You only need to share it once**
    d. Open wsl and run this command to attach it `usbipd attach --wsl --busid <busid>`
    e. you should now see the arduino if you list the usb device in wsl with `lsusb`

You can also create a shortcut to be able to launch a new windows terminal directly with the **Ubuntu** profile. To do that simply navigate to `C:\Users\{username}\AppData\Roaming\Microsoft\Windows\Start Menu\Programs`, right click, new and shortcut. Use `wt.exe` as target and add `-p Ubuntu` to start the ubuntu wsl profile.  So the resulting shortcut target should be `wt.exe -p Ubuntu`.  Note that you might need to use the full path to `wt.exe`.  To get the full path you can simply run `where wt` in a windows terminal.

# Install zsh

Follow this [page](https://dev.to/equiman/zsh-on-windows-with-wsl-1jck) to install zsh in wsl.  

**Note:** You don't need to install the font since we already installed nerdfonts in the previous step.


# Setup Rust

Let's get Rusty youtube channel as a nice [video](https://youtu.be/E2mKJ73M9pg) on how to setup linux for rust development.  Follow there step in the wsl terminal.


1. Install nvim
2. Install nvcahd. **Make sure to install nerdfonts in the windows terminal** to do that see WSl setup above.
3. Install rust anylizer
4. Install rustaceanvim


# Setup Arduino tools

Look at this [page](https://blog.logrocket.com/complete-guide-running-rust-arduino/) to setup rust for arduino.  This will install all the necessary dependencies and init a new project with `cargo generate`.

## USB

To make sure what `/dev/tty**` file to use for ravendude you can follow these steps.

First, make sure that you have installed **usbipd-win** and that the bus is shared with wsl in visble in wsl with `lsusb`.  To determine what `/dev/tty***` unbeffured file to use you can do 

```bash
❯ lsusb
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 002: ID 2341:0043 Arduino SA Uno R3 (CDC ACM)  <-- arduino device.  this is a acm so looking for ttyACM* file
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

If this is the only usb device shared with wsl we can suppose that the file name will be `/dev/ttyACM0`.   But if you want to be sure you can use this command `dmesg -H > test.txt`.  This will output to the `test.txt` file.  Open this file and search for arduino.
```bash
[  +0.000002] usb 1-1: Manufacturer: Arduino (www.arduino.cc)
[  +0.000002] usb 1-1: SerialNumber: 75130303036351106150
[  +0.024063] cdc_acm 1-1:1.0: ttyACM0: USB ACM device < -- ttyACM0
```

You could also use this method:
1. disconnect the arduino.
2. list all the tty files to a file `ls \dev\tty* > test.txt`
3. connect the arduino 
4. attach the device to wsl (see command above in the wsl section.)
5. list the tty files to a new tmp file `ls \dev\tty* > test2.txt`
6. compare both files:
```bash
❯ comm -1 -3 test.txt test2.txt
/dev/ttyACM0

```

