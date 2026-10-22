# uBlock-Filters

| File | Dùng cho | Nội dung |
|---|---|---|
| `dns-family.txt` | Cả nhà (DNS) | Chặn quảng cáo/tracking (Google, YouTube, TikTok, Facebook, Pinterest, X, Spotify), web thuốc/TPCN/đông y, web rác, Shopee Live |
| `dns-work-ads.txt` | **Chỉ thiết bị làm việc** (DNS) | Mở khoá AdSense, Google Ads, Pinterest Ads, Meta Ads, TikTok Ads, X Ads, Microsoft Ads, công cụ affiliate |
| `fb-notify-filters.txt` | uBlock Origin (trình duyệt) | Ẩn thông báo nhóm Facebook (giao diện cũ) |

Cú pháp AdGuard (`||domain^`, `/regex/`, `$important`) — dùng với AdGuard Home hoặc AdGuard DNS.

> **Giới hạn: mỗi file bộ lọc < 1000 dòng** (tính cả chú thích), vì AdGuard Home giới hạn số dòng *Custom filtering rules*.
> Viết gọn: dùng `||domain^` (tự khớp mọi subdomain, không cần liệt kê `ads.x.com`, `cdn.x.com`...), gộp nhiều tên miền cùng nhóm vào một regex khi cần.
> Kiểm tra nhanh: `wc -l *.txt`.
Mọi dòng trong `dns-work-ads.txt` đều có `$important` nên thắng cả luật chặn `$important` lẫn các blocklist công khai (AdGuard DNS filter, OISD, HaGeZi...).

## Cách tách "cả nhà chặn – mình được chạy quảng cáo"

### AdGuard DNS (cloud)
1. Server **Gia đình**: dán nội dung `dns-family.txt` vào *User rules*, gán tất cả thiết bị trong nhà.
2. Tạo server thứ hai **Làm việc**: dán `dns-family.txt` **rồi** `dns-work-ads.txt` vào *User rules*, chỉ gán máy tính/điện thoại làm việc.

### AdGuard Home (router / máy chủ trong nhà)
Blocklist trong AdGuard Home áp cho mọi máy, nên:
1. *Filters → DNS blocklists → Add*:
   `https://raw.githubusercontent.com/thangworks/uBlock-Filters/master/dns-family.txt`
2. **Không** thêm `dns-work-ads.txt` làm blocklist. Thay vào đó dán vào *Custom filtering rules* sau khi gắn tên/IP máy làm việc
   (ví dụ `192.168.1.50` — nên đặt IP tĩnh hoặc khai báo client trong *Settings → Client settings*):

   ```sh
   curl -s https://raw.githubusercontent.com/thangworks/uBlock-Filters/master/dns-work-ads.txt \
     | sed -E "/^@@/s/\\\$important$/\$important,client='192.168.1.50'/"
   ```

   Kết quả có dạng `@@||ads.pinterest.com^$important,client='192.168.1.50'` — chỉ máy đó được mở khoá.

### Tăng khả năng chặn mà không tốn dòng custom
Blocklist thêm bằng URL (*Filters → DNS blocklists*) **không** tính vào giới hạn custom rules. Nên thêm:
- AdGuard DNS filter (có sẵn trong AdGuard Home)
- HaGeZi Multi PRO: `https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/pro.txt`
- hostsVN (quảng cáo Việt Nam): `https://raw.githubusercontent.com/bigdargon/hostsVN/master/filters/adservers-all.txt`

Luật `@@...$important` của máy làm việc vẫn thắng các list này.

### YouTube / Spotify / Facebook — DNS chặn được đến đâu
| Ứng dụng | DNS chặn được | DNS **không** chặn được (cần cách khác) |
|---|---|---|
| YouTube | Tracking, quảng cáo banner/web (doubleclick, googlesyndication) | Quảng cáo trong video (đi cùng `googlevideo.com`) → trình duyệt + uBlock Origin, hoặc YouTube Premium |
| Spotify | Một phần quảng cáo audio, telemetry | Đa số quảng cáo bản Free (đi cùng `spclient`) → Spotify Premium, hoặc nghe trên web + uBlock Origin |
| Facebook / Instagram | Pixel/SDK trên web khác, Audience Network trong app khác | Bài quảng cáo trong feed (đi cùng `graph.facebook.com`) → web + uBlock Origin |

Đừng chặn `googlevideo.com`, `youtubei.googleapis.com`, `spclient.wg.spotify.com`, `graph.facebook.com`, `fbcdn.net`: ứng dụng sẽ hỏng.

### Lưu ý
- Sau khi đổi DNS, xoá cache: trình duyệt (`chrome://net-internals/#dns`) và máy (`ipconfig /flushdns` trên Windows).
- Trên máy làm việc nên tắt uBlock/AdBlock cho `adsense.google.com`, `ads.google.com`, `ads.pinterest.com`... vì tiện ích trình duyệt chặn riêng, không phụ thuộc DNS.
- Trên mạng gia đình, bấm vào kết quả quảng cáo (Sponsored) trên Google sẽ không mở được vì `googleadservices.com` bị chặn — đó là chủ ý.
