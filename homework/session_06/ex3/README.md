# Bài 3: Cấu hình tường lửa UFW và chuẩn đoán cổng mạng

## 1. Mục tiêu

- Cấu hình tường lửa UFW để bảo vệ máy chủ Cloud VPS.
- Chặn toàn bộ kết nối đi vào không cần thiết.
- Cho phép kết nối đi ra.
- Mở cổng SSH `22/tcp` để quản trị máy chủ.
- Mở cổng ứng dụng Web `8080/tcp`.
- Kiểm tra trạng thái tường lửa và các cổng đang lắng nghe.

## 2. Bối cảnh

Máy chủ chuẩn bị deploy một ứng dụng web lắng nghe tại cổng `8080`.
Để đảm bảo an toàn, tường lửa chỉ mở các cổng cần thiết:

- `22/tcp`: SSH.
- `8080/tcp`: ứng dụng Web.

## 3. Cấu hình chính sách mặc định

Chặn toàn bộ kết nối đi vào:

```bash
sudo ufw default deny incoming
```

Kết quả:

```text
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
```

Cho phép toàn bộ kết nối đi ra:

```bash
sudo ufw default allow outgoing
```

Kết quả:

```text
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
```

## 4. Mở cổng SSH và cổng ứng dụng Web

Mở cổng SSH:

```bash
sudo ufw allow 22/tcp
```

Kết quả:

```text
Rule added
Rule added (v6)
```

Mở cổng ứng dụng Web:

```bash
sudo ufw allow 8080/tcp
```

Kết quả:

```text
Rule added
Rule added (v6)
```

## 5. Kích hoạt UFW

```bash
sudo ufw enable
```

Kết quả:

```text
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```

## 6. Kiểm tra trạng thái tường lửa

Lệnh kiểm tra:

```bash
sudo ufw status verbose
```

Kết quả:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
8080/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
8080/tcp (v6)              ALLOW IN    Anywhere (v6)
```

## 7. Kiểm tra các cổng đang lắng nghe

Lệnh kiểm tra:

```bash
ss -tlnp
```

Kết quả tham khảo:

```text
State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process
LISTEN  0       128            0.0.0.0:22          0.0.0.0:*      users:(("sshd",pid=681,fd=3))
LISTEN  0       511            0.0.0.0:8080        0.0.0.0:*      users:(("node",pid=1248,fd=21))
LISTEN  0       128               [::]:22             [::]:*      users:(("sshd",pid=681,fd=4))
LISTEN  0       511               [::]:8080           [::]:*      users:(("node",pid=1248,fd=22))
```

## 8. Kiểm tra kết nối ứng dụng Web

Nếu ứng dụng đang chạy ở cổng `8080`, có thể kiểm tra bằng:

```bash
curl http://localhost:8080
```

Kết quả ví dụ:

```text
Hello from web application on port 8080
```

## 9. Kết luận

Đã cấu hình UFW theo đúng yêu cầu:

- Chính sách mặc định: `deny incoming`, `allow outgoing`.
- Đã mở cổng `22/tcp` cho SSH.
- Đã mở cổng `8080/tcp` cho ứng dụng Web.
- UFW đang hoạt động với trạng thái `Status: active`.
- Kết quả `ufw status verbose` hiển thị đầy đủ rule `ALLOW IN` cho cổng `22/tcp` và `8080/tcp`.
