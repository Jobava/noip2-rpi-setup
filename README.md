### noip2-rpi-setup
Set up noip dynamic DNS client on a raspberry pi

Requires `build-essential` to be installed already:

```bash
sudo apt-get install build-essential -y

cd /usr/local/src
wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
tar xzf noip-duc-linux.tar.gz
cd no-ip-2.1.9
make
make install
```

Set up a systemd unit file:

```bash
sudo nano /etc/systemd/system/noip2.service
```

Paste in the file:

```
[Unit]
Description=Adjusts noip dynamic DNS IP based on host current external IP
Requires=network.target
Wants=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/bin/noip2 -c /home/pi/.no-ip2.conf
User=pi
Group=pi
WorkingDirectory=/home/pi

[Install]
WantedBy=multi-user.target
```

Create and configure the config file (interactive prompt):

```bash
sudo /usr/local/bin/noip2 -C
```

Then copy the created config file in the home folder and make it a file owned by user `pi`:

```bash
sudo cp /usr/local/etc/no-ip2.conf /home/pi/.noip2.conf
sudo chown pi:pi /home/pi/.noip2.conf
```
