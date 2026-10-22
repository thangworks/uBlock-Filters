# Quy ước dự án

- Mỗi file bộ lọc (`*.txt`) phải **< 1000 dòng**, tính cả chú thích — AdGuard Home giới hạn số dòng custom rules. Kiểm tra bằng `wc -l *.txt` trước khi commit.
- Viết gọn: ưu tiên `||domain^` (khớp cả subdomain) thay vì liệt kê từng subdomain; không thêm dòng trùng.
- Cú pháp AdGuard DNS. `dns-family.txt` = chặn cho cả nhà; `dns-work-ads.txt` = allowlist `@@...$important` chỉ cho máy làm quảng cáo.
- Không chặn tên miền dùng chung với nội dung (`googlevideo.com`, `youtubei.googleapis.com`, `spclient.wg.spotify.com`, `graph.facebook.com`, `fbcdn.net`).
