# Threat Model - Lab 3

## Thông tin nhóm
- Thành viên 1: Nguyễn Văn A - 12345678
- Thành viên 2: Trần Thị B - 87654321

## Assets
- Source code của sender.py, receiver.py và des_socket_utils.py
- Bí mật: DES key và IV (được tạo ngẫu nhiên cho mỗi lần gửi)
- Bản tin (plaintext) mà người dùng gửi/nhận
- File log và output do chương trình tạo

## Attacker model
- Eavesdropper trên mạng có thể nghe gói TCP (passive network attacker)
- Attacker có khả năng thay đổi gói trong transit (MITM) để sửa ciphertext
- Insider hoặc attacker có quyền truy cập vào máy chủ/recorded logs

## Threats
1. Lộ plaintext khi key/IV hoặc bản mã bị rò rỉ (exposure of secrets).
2. Thay đổi ciphertext (bit-flip) dẫn đến phục hồi bản tin sai hoặc lỗi khi giải mã (tampering).
3. Sử dụng lại key/IV hoặc sinh khóa yếu dẫn đến bị phá mã (key compromise).

## Mitigations
- Sinh key và IV bằng nguồn ngẫu nhiên an toàn (os.urandom) và tránh ghi chúng vào logs công khai.
- Thêm cơ chế xác thực-integrity (MAC/HMAC) hoặc chuyển sang chế độ AEAD (ví dụ AES-GCM) để phát hiện tamper.
- Hạn chế thông tin lưu trong logs (không ghi key/IV/plaintext), và áp dụng quyền truy cập file chặt chẽ.
- Thiết kế timeout/giới hạn kết nối và kiểm tra padding/độ dài trước khi xử lý để giảm lỗi do dữ liệu bị sửa.

## Residual risks
- DES/CBC không cung cấp bảo mật hiện đại (DES có key 56 bit yếu) — còn rủi ro bị tấn công mật mã mạnh.
- Nếu attacker kiểm soát hoàn toàn host (insider), thì các biện pháp trên không đủ để bảo vệ bí mật.

