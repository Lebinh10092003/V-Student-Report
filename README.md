# V+ Student Analytics · ENJOY AI 2026

Dashboard theo dõi năng lực thi đấu V+ STEAM LAB so với các đối thủ, triển khai website tĩnh qua GitHub Pages.

## Tính năng

- **Tổng quan**: phân bố điểm/phân vị, tương quan điểm–thời gian, xếp hạng V+ và đối thủ.
- **Đội V+**: một hồ sơ duy nhất trên mỗi tên đội chuẩn hóa, tổng hợp các lần thi, giải thưởng, phân vị, thống kê và nguồn từng kết quả.
- **Đối thủ**: đầy đủ kết quả, tìm kiếm, phân tích đối thủ trực tiếp cùng bảng và host.
- **Học sinh**: tìm tên xuất hiện trong nhiều đội, bảng chi tiết và cảnh báo trùng tên cần xác minh.
- **Kịch bản**: mô phỏng tăng điểm / giảm thời gian với đối thủ giữ nguyên, không phải dự báo giải thưởng.
- **Dữ liệu**: 653 dòng kết quả chuẩn hóa; cho phép xuất CSV và in báo cáo PDF.

## Nguyên tắc xếp hạng toàn bộ host

- Bảng xếp hạng được phân nhóm theo **nội dung + bảng thi**, ví dụ `Con đường phát minh - D`.
- Gom **tất cả host** (Hà Nội, Đà Nẵng, Asian Open, TP. HCM, v.v.) vào cùng một bảng thi khi nội dung/bảng tương ứng.
- Mỗi đội chỉ xuất hiện **một lần trong một bảng**: điểm xếp hạng là **trung bình cộng tất cả kết quả cùng bảng trên toàn bộ host**.
- Với bảng đối kháng có số trận khác nhau, so sánh bằng điểm trung bình mỗi trận (Match Points/trận).
- **Bộ lọc host chỉ phục vụ xem dữ liệu từng host, không thay đổi thứ hạng toàn bộ host.**
- Có bài kiểm thử `tests/standings.test.mjs` chạy trước mỗi lần GitHub Pages triển khai để đảm bảo logic này.

### Gộp V+ Wizards

`SMC-Wizards` và `V+ Wizards` là **một đội duy nhất** với **ba lần dự thi**:

| Host | Nội dung/bảng | Điểm | Thời gian (s) | Giải thưởng |
|---|---|---:|---:|---|
| Enjoy AI Hà Nội | Con đường phát minh, D | 540 | 299 | Top 10 |
| Enjoy AI Asian Open 2026 | Con đường phát minh, D | 720 | 257 | Giải Nhất |
| Enjoy AI Đà Nẵng | Con đường phát minh, E | 800 | 210 | Giải Nhì |

Cả ba kết quả được giữ trong **một đội V+ Wizards**. Quy tắc mới là **lấy trung bình cộng, không chọn điểm cao nhất**.

- Điểm trung bình cả ba lần: **(540 + 720 + 800) / 3 = 686,67 điểm** (chỉ số mô tả tổng hợp).
- **Bảng D:** 2 lần thi, trung bình **630 điểm**. Xếp hạng trung bình trên toàn bộ host **9/77 đội**, thuộc **Top 11,69%** khi so với trung bình thành tích của tất cả đội cùng bảng.
- **Bảng E:** 1 lần thi, trung bình **800 điểm**. Xếp hạng tham chiếu **2/49 đội**, thuộc **Top 4,08%**.
- Vì hai bảng có thang điểm/điều kiện có thể khác nhau, **điểm trung bình tổng 686,67 không được dùng để xếp hạng chung tất cả đội khác bảng**.
- Với các đội thi nhiều bảng, nhóm Top tổng hợp sử dụng trung bình vị trí tương đối có trọng số theo số lần thi của từng bảng.

Cách đối chiếu: mỗi đội trong mỗi bảng chỉ xuất hiện một lần, điểm được tính `SUM(điểm các lượt hợp lệ cùng bảng) / số lượt hợp lệ`. Đối kháng dùng trung bình số Match Points/trận nếu số trận giữa các host khác nhau. Thứ hạng tại từng host do nguồn cung cấp vẫn được bảo toàn ở hồ sơ.

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
