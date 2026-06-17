# China Packet Guard

一個 Android app，用 `VpnService` 偵測並阻斷手機上經 Wi-Fi 或行動網路送往中國大陸 IP 網段的封包。

## 工作方式

- App 從 APNIC `delegated-apnic-latest` 下載 `CN` IPv4/IPv6 網段。
- VPN 只加入中國大陸目的地路由，不接管全部流量。
- 被路由進 VPN 的封包會被解析目的 IP、port、protocol，記錄後直接丟棄，因此達成阻斷。
- Android 10 以上會呼叫 `ConnectivityManager.getConnectionOwnerUid(...)` 嘗試反查發起連線的 app。
- 畫面與通知會顯示 app、UID、目的 IP、port、protocol 與位置「中國大陸」。

## 限制

- 未 root 的 Android 不能直接監聽所有網卡封包；這個 app 需由使用者授權成為 VPN。
- Android 9 以下無法可靠反查封包屬於哪個 app，紀錄可能顯示未知 UID。
- APNIC 國碼代表 IP 配發地，不等同精準城市定位。若要省市/城市級定位，需要另外授權 GeoIP 資料庫。
- 內建 seed route 只供第一次啟動測試；正式使用請先按「更新中國大陸 IP 路由表」。

