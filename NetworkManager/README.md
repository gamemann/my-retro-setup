When turning on the BlueTooth scanner, I found that it heavily impacted my WiFi card after some time causing very high latency regardless of the WiFi utilizing 5Ghz (BlueTooth uses 2.4GHz, so it has been reported that using a 5GHz WiFi connection would correct this issue so there isn't any interference between the two, but that was not the case for me). My kernel logs via `dmesg` were filled with the following while the issue was ongoing.

```bash
[ 1277.531392] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1279.433073] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1279.536996] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1279.540133] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1279.543248] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1279.546389] rtw_8821ce 0000:02:00.0: failed to send h2c command
[ 1281.416092] rtw_8821ce 0000:02:00.0: failed to send h2c command

# ...
[34331.816515] rtw_8821ce 0000:02:00.0: firmware failed to leave lps stat
```

My mini-PC also has a RealTek WiFi card that utilizes the `rtw_8821ce` driver. I tried building and installing the latest driver from [`lwfinger/rtw88`](https://github.com/lwfinger/rtw88), but this didn't help, unfortunately.

What I believe ended up fixing this issue was disabling power-saving mode. On Debian 12, copying [`powersave-mode.conf`](./powersave-mode.conf) to `/etc/NetworkManager/conf.d/` and restarting corrected this issue.