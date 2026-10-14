# Bài 1: Sửa điều kiện biên Grace Period

## Phân tích lỗi

Trong mã ban đầu, điều kiện `sub.daysOverdue >= 3` hạ cấp thuê bao ngay khi nợ đúng 3 ngày. Điều này không khớp chính sách: khách hàng vẫn được giữ gói cũ trong toàn bộ 3 ngày ân hạn; chỉ khi `daysOverdue > 3` mới chuyển về `FREE`, giới hạn còn một thiết bị và chuyển trạng thái sang `EXPIRED`.

Lỗi xảy ra do dùng toán tử `>=` thay vì `>` ở điều kiện hạ cấp. Với dữ liệu đúng tại biên (`daysOverdue === 3`), nhánh hạ cấp được thực thi sớm một ngày.

## Test cases đối chứng

| Trường hợp kiểm thử | Dữ liệu đầu vào | Kết quả sai thực tế | Kết quả đúng mong đợi |
|---|---|---|---|
| Nợ đúng 3 ngày (vẫn trong hạn ân hạn) | `daysOverdue: 3`, `planTier: FAMILY`, `deviceLimit: 5`, `status: ACTIVE` | `FREE`, `deviceLimit: 1`, `EXPIRED` | Giữ `FAMILY`, giữ `deviceLimit: 5`, đổi trạng thái thành `GRACE_PERIOD` |
| Nợ 4 ngày (đã quá hạn ân hạn) | `daysOverdue: 4`, `planTier: FAMILY`, `deviceLimit: 5`, `status: ACTIVE` | `FREE`, `deviceLimit: 1`, `EXPIRED` | `FREE`, `deviceLimit: 1`, `EXPIRED` |

## Cách sửa

Tệp [`app.js`](./app.js) kiểm tra quá hạn trước bằng `daysOverdue > 3`. Với số ngày nợ dương nhưng không quá 3, thuê bao chuyển sang `GRACE_PERIOD` và giữ nguyên gói cùng hạn mức thiết bị. Các thuộc tính được cập nhật trực tiếp trên từng đối tượng trong mảng. Tệp [`solution_ex1.js`](./solution_ex1.js) là bản nộp độc lập theo tên file yêu cầu và có cùng logic.

Chạy bằng `node app.js` hoặc `node solution_ex1.js`.
