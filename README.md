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


----------------------------------------------------------------------------------


### Ubuntu에서 HF_6Q 핫스팟 인터넷 문제 해결 가이드

Ubuntu에서 HF_6Q Wi-Fi 핫스팟 연결 시 인터넷이 불안정하거나 연결되지 않을 때, 아래 명령을 순서대로 실행
wifi(HQ_6F) 자체에는 문제가 없으며, 다른 기기에서는 정상 동작하는 경우 해결법.
문제는 Ubuntu와 HQ_6F 핫스팟 간 네트워크 호환성 문제일 가능성이 높음.

```bash
# 1) 기존 HF_6Q 연결 초기화 (캐시 IP 및 이전 연결 정보 삭제)
nmcli connection delete id HF_6Q

# 2) IPv6 비활성화 (일부 핫스팟과 IPv6 충돌 방지)
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1

# 3) HF_6Q 핫스팟 재연결 (DHCP로 새 IP 할당)
nmcli device wifi connect HF_6Q

# 4) DNS 서버 변경 (HF_6Q DNS 불안정 시 Google DNS 사용)
sudo bash -c 'echo -e "nameserver 8.8.8.8\nnameserver 8.8.4.4" > /etc/resolv.conf'
sudo resolvectl flush-caches

# 5) MTU 조정 (패킷 손실이 계속 발생하면 적용)
sudo ip link set dev wlo1 mtu 1400

# 6) DHCP 재요청 (IP 재갱신)
sudo dhclient -r wlo1
sudo dhclient wlo1

# 7) 인터넷 연결 테스트
ping -c 5 8.8.8.8   # 외부 IP 연결 확인
ping -c 5 google.com # DNS 및 인터넷 연결 확인
