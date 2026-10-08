# Bài 4: Lưu trữ dữ liệu đơn giản với Docker Volume

## 1. Mục tiêu
Tạo một Named Volume (`easy-volume`) để lưu trữ tệp dữ liệu bền vững giữa các lần chạy Container (kiểm tra dữ liệu không bị mất khi container đã dừng và bị hủy).

---

## 2. Cấu trúc thư mục nộp bài
```text
homework/session_09/ex4/
└── README.md
```

---

## 3. Các bước thực hiện

### Bước 1: Tạo Named Volume mới
Tạo một Docker Volume có tên là `easy-volume`:
```bash
docker volume create easy-volume
```

*Kiểm tra danh sách volume trên máy:*
```bash
docker volume ls
```

---

### Bước 2: Ghi dữ liệu vào Volume bằng Container Alpine thứ nhất
Khởi chạy một container Alpine tạm thời (tự động xóa sau khi chạy xong nhờ flag `--rm`), mount volume `easy-volume` vào thư mục `/data` bên trong container, và ghi nội dung vào tệp `/data/test.txt`:
```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```

> **Giải thích:**
> - `--rm`: Tự động xóa container sau khi hoàn thành lệnh.
> - `-v easy-volume:/data`: Gắn named volume `easy-volume` vào thư mục `/data` của container.
> - `alpine`: Sử dụng base image Alpine Linux siêu nhẹ.
> - `sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"`: Ghi dòng chữ vào tệp `test.txt` nằm trong thư mục được mount. Khi container này kết thúc và bị hủy, dữ liệu vẫn được lưu trữ an toàn trong volume.

---

### Bước 3: Đọc lại dữ liệu bằng Container Alpine thứ hai (Kiểm tra)
Khởi chạy một container Alpine hoàn toàn mới khác để kiểm tra xem dữ liệu trong `easy-volume` có còn tồn tại không:
```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```

**Kết quả mong đợi trên Terminal:**
```text
Luu tru du lieu Docker
```

---

## 4. Các lệnh kiểm tra và quản lý bổ sung (Tham khảo)

- **Xem thông tin chi tiết của Volume:**
  ```bash
  docker volume inspect easy-volume
  ```

- **Dọn dẹp / Xóa Volume sau khi hoàn thành bài tập (tùy chọn):**
  ```bash
  docker volume rm easy-volume
  ```
