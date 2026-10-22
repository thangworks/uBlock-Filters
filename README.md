# uBlock-Filters

| File | Dùng cho | Nội dung |
|---|---|---|
| `dns-family.txt` | Cả nhà (DNS) | Chặn quảng cáo/tracking (Google, YouTube, TikTok, Facebook, Pinterest, X, Spotify), web thuốc/TPCN/đông y, web rác, Shopee Live |
| `dns-work-ads.txt` | **Chỉ thiết bị làm việc** (DNS) | Mở khoá AdSense, Google Ads, Pinterest Ads, Meta Ads, TikTok Ads, X Ads, Microsoft Ads, công cụ affiliate |
| `fb-notify-filters.txt` | uBlock Origin (trình duyệt) | Ẩn thông báo nhóm Facebook (giao diện cũ) |

Cú pháp AdGuard (`||domain^`, `/regex/`, `$important`) — dùng với AdGuard Home hoặc AdGuard DNS.
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

### Lưu ý
- Sau khi đổi DNS, xoá cache: trình duyệt (`chrome://net-internals/#dns`) và máy (`ipconfig /flushdns` trên Windows).
- Trên máy làm việc nên tắt uBlock/AdBlock cho `adsense.google.com`, `ads.google.com`, `ads.pinterest.com`... vì tiện ích trình duyệt chặn riêng, không phụ thuộc DNS.
- Trên mạng gia đình, bấm vào kết quả quảng cáo (Sponsored) trên Google sẽ không mở được vì `googleadservices.com` bị chặn — đó là chủ ý.
