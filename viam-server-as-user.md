# Setup user `viam` on Linux machine

## Setup python and other libraries
```
sudo apt update && sudo apt upgrade
sudo apt install sudo apt install libfuse2
# If you haven't setup python already then...
sudo apt install python3-pip python3-venv
```

## Create user and login

```
sudo useradd -m viam -G dialout,sudo,audio
sudo passwd viam # Give viam a sudo password, so root requests can only be made using that
sudo visudo # Near the bottom add     viam ALL=(ALL) ALL
```

## As viam user setup viam-server

`sudo su - viam`

- [These Instructions](https://docs.viam.com/get-started/installation/) are to install viam-server but we are going a bit off piste. 

1. Log into app-viam.com and create a machine and select the setup tab.

1. Select the machine architecture - note for this setup only Linux(Aarch64) for ARM and Linux(x86_64) for Intel and AMD are supported

1. Select Copy button for "1. Download Viam app config" and paste it into your terminal and hit return

1. Select Copy button for "2. Download and install viam-server" and paste *but do not hit return*.  Instead backspace over the ` && sudo ./viam-server --aix-install` to delete it and then hit return

1. Run viam-server for the first time, and check it is working ok with `/home/viam/viam-server -config=/etc/viam.json`

1. Once 3. Connect and configure is green, we are ready for the next part

## Install viam-server for user viam

End the terminal session with ctrl-c to cease viam-server

Run `sudo nano /etc/systemd/system/viam-server.service` as paste the following into the editor

```
[Unit]
Description=VIAM Server
After=network.target

[Service]
Type=simple
User=viam
Group=viam
ExecStart=/home/viam/viam-server -config=/etc/viam.json
Restart=on-failure
# Environment variables can be set here if needed
#Environment=VARIABLE=value

[Install]
WantedBy=multi-user.target
```

Now run 
```
sudo systemctl daemon-reload
sudo systemctl enable viam-server
sudo service viam-server status # Or continue to use systemctl
```

## Congratulations ##

You are now ready to add services
