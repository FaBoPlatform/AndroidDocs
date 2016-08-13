# Bluetoothでのデバック

AndroidとPCをUSBでつなぎ下記コマンドを実行

```
/Users/ユーザ名/Library/Android/sdk/platform-tools/adb tcpip 8888
```

```
/Users/ユーザ名/Library/Android/sdk/platform-tools/adb shell ifconfig
```

```
/Users/ユーザ名/Library/Android/sdk/platform-tools/adb connect 192.168.100.7:8888
```

```
/Users/sasakiakira/Library/Android/sdk/platform-tools/adb shell setprop service.adb.tcp.port 666
```

```
/Users/sasakiakira/Library/Android/sdk/platform-tools/adb tcpip 6666

```

```
/Users/sasakiakira/Library/Android/sdk/platform-tools/adb connect 192.168.100.7:6666
```

