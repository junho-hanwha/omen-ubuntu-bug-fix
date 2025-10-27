# omen-ubuntu-bug-fix


## WiFi/Network Speed
### Disable Power Management (Prevent Speed Throttling)
```
sudo mkdir -p /etc/NetworkManager/conf.d
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

```
wifi.powersave = 2
```

save and restart NetworkManager

```
sudo systemctl restart NetworkManager
```


#### Network speed test
```
sudo apt install speedtest-cli
speedtest
```
