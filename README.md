# V+ Student Analytics · ENJOY AI 2026

Dashboard theo dõi năng lực thi đấu V+ STEAM LAB so với các đối thủ, triển khai website tĩnh qua GitHub Pages.

## Tính năng

- **Tổng quan**: phân bố điểm/phân vị, tương quan điểm–thời gian, xếp hạng V+ và đối thủ.
- **Đội V+**: một hồ sơ duy nhất trên mỗi tên đội chuẩn hóa, tổng hợp các lần thi, giải thưởng, phân vị, thống kê và nguồn từng kết quả.
- **Đối thủ**: đầy đủ kết quả, tìm kiếm, phân tích đối thủ trực tiếp cùng bảng và host.
- **Học sinh**: tìm tên xuất hiện trong nhiều đội, bảng chi tiết và cảnh báo trùng tên cần xác minh.
- **Kịch bản**: mô phỏng tăng điểm / giảm thời gian với đối thủ giữ nguyên, không phải dự báo giải thưởng.
- **Dữ liệu**: 653 dòng kết quả chuẩn hóa; cho phép xuất CSV và in báo cáo PDF.

### Gộp V+ Wizards

`SMC-Wizards` và `V+ Wizards` là **một đội duy nhất** với **ba lần dự thi**:

| Host | Nội dung/bảng | Điểm | Thời gian (s) | Giải thưởng |
|---|---|---:|---:|---|
| Enjoy AI Hà Nội | Con đường phát minh, D | 540 | 299 | Top 10 |
| Enjoy AI Asian Open 2026 | Con đường phát minh, D | 720 | 257 | Giải Nhất |
| Enjoy AI Đà Nẵng | Con đường phát minh, E | 800 | 210 | Giải Nhì |

Cả ba kết quả đều được giữ; **không cộng thành 2.060 điểm thi**, vì chúng thuộc ba lượt thi và hai bảng đấu khác nhau. Hồ sơ đội hiển thị cả ba cùng với phân vị và thống kê mô tả, không tính thành ba đội độc lập.

## Nguồn dữ liệu

- `data/results.json`: 653 kết quả, 55 kết quả V+, 598 kết quả đối thủ, từ 17 bảng thi.
- Nguồn: `xet-diem-lan-2-3.xlsx` đã được chuẩn hóa trong `Vplus_EnjoyAI2026_ChuanHoa_PhanTich.xlsx`.
- Trường `sourceSheet` và `sourceRow` giữ dấu vết nguồn để đối chiếu.
- **Trùng tên học sinh không đồng nghĩa trùng người.** Danh tính cần xác minh vì dữ liệu không có mã định danh.
- Không so điểm thô khác nội dung/bảng, không khẳng định xu hướng tiến bộ theo thời gian khi thiếu ngày thi.
- Không tồn tại sheet hoặc giao diện “Đặc biệt”.

## GitHub Pages

Trang dự kiến: https://lebinh10092003.github.io/V-Student-Report/

Repository có workflow `.github/workflows/pages.yml` để **kiểm tra JavaScript, kiểm tra đủ dữ liệu và gộp tên, rồi triển khai**.

Lần đầu chạy, nếu Pages chưa được bật: vào **Settings → Pages → Build and deployment → Source: GitHub Actions**, sau đó chạy lại workflow **Verify and deploy V+ Student Analytics** ở tab Actions.

## Cấu trúc

```
index.html
styles.css
app.js
data/results.json
.github/workflows/pages.yml
```

Không yêu cầu backend; chỉ cần GitHub Pages hoặc máy chủ tĩnh để tải file JSON tương đối. Mở trực tiếp `index.html` bằng `file://` có thể bị chặn `fetch` bởi chính sách bảo mật trình duyệt.
