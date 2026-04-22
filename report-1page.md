# Report 1 page - Lab 3

## Thông tin nhóm
- Thành viên 1: Nguyễn Văn A - 12345678
- Thành viên 2: Trần Thị B - 87654321

## Mục tiêu
- Mục tiêu: Hiểu và cài đặt luồng gửi/nhận dữ liệu mã hoá DES-CBC qua TCP socket; triển khai padding PKCS#7 và header độ dài; viết test kiểm thử positive/negative; nhận diện điểm yếu bảo mật và đề xuất mitigations.

## Phân công thực hiện
- Phân công:
  - Nguyễn Văn A: triển khai Sender, viết test cho sender, thu thập logs cho ca happy-path.
  - Trần Thị B: triển khai Receiver, viết threat model, thu thập logs cho ca negative.
  - Làm chung: des_socket_utils.py, tích hợp test end-to-end, báo cáo và peer-review.

## Cách làm
- Cách làm: Sender tạo key 8 byte và IV 8 byte (ngẫu nhiên), mã hoá dữ liệu với DES-CBC và padding PKCS#7, sau đó gửi tuần tự `key || iv || length(4-byte big-endian) || ciphertext` qua TCP. Receiver đọc header cố định 20 byte, phân tích key/iv/length, đọc chính xác số byte ciphertext, giải mã và loại padding. Kiểm thử gồm unit test cho padding/header, test contract order, và kiểm thử tích hợp local bằng khởi chạy receiver subprocess và chạy sender.

## Kết quả
- Kết quả: Tất cả test unit và kiểm thử tích hợp local (happy path) thực hiện thành công. Đã thêm negative tests cho tamper và wrong-key để chứng minh tính không bền của DES-CBC khi bị thay đổi ciphertext hoặc dùng sai key. Logs minh chứng được lưu trong thư mục `logs/`.

## Kết luận
- Kết luận: Lab giúp nắm rõ luồng truyền dữ liệu mã hoá qua socket và các lỗi phổ biến (padding, header parsing, recv exact). Về bảo mật, gửi key và IV trên cùng luồng plaintext là không an toàn; DES là thuật toán cũ và có nhiều điểm yếu — trong thực tế cần dùng TLS, AES-GCM/HKDF và cơ chế trao đổi key an toàn.
