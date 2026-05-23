## GitLab

Việc đẩy code lên các nền tảng đám mây công cộng tiềm ẩn rủi ro lộ lọt thông tin.

Thay vào đó, bộ phận IT có thể tải bộ cài đặt của GitLab (phiên bản Enterprise hoặc Community) và cài thẳng lên máy chủ vật lý đặt tại phòng server của công ty.

Mô hình dựng một máy chủ Ubuntu chạy GitLab Server và dùng máy Kali Linux làm Client là một mô hình chuẩn mực, mô phỏng chính xác cấu trúc hạ tầng mạng trong các doanh nghiệp thực tế.

#### Giai đoạn 1: cấu hình mạng

*NAT*  (2 máy dùng chung mạng Wi-Fi/Mạng dây của máy tính thật để vừa có Internet vừa thông nhau).

#### Giai đoạn 2: cài đặt gitlab trên ubuntu

**Bước 1: Cập nhật hệ thống và cài đặt các gói bổ trợ bắt buộc**

Bash

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl openssh-server ca-certificates tzdata perl
```

**Bước 2: Thêm kho lưu trữ (Repository) của GitLab vào Ubuntu**

Bash

```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

**Bước 3: Tiến hành cài đặt GitLab**

Bash

```
sudo EXTERNAL_URL="http://192.168.1.130" apt-get install gitlab-ce
```

*Khi cài đặt xong, màn hình sẽ hiện biểu tượng con cáo lớn bằng ký tự text của GitLab.*

**Bước 4: Lấy mật khẩu quản trị mặc định**
Mật khẩu của tài khoản `root` sẽ được tự động sinh ra và lưu tạm trong hệ thống. Hãy chạy lệnh sau để xem:

Bash

```
sudo cat /etc/gitlab/initial_root_password
```

#### Giai đoạn 3:  kết nối và thực hành từ máy kali linux (client)

Bây giờ máy chủ Ubuntu đã vận hành xong, chuyển hoàn toàn sang giao diện máy **Kali Linux**.

**Bước 1: Đăng nhập vào giao diện Web của GitLab**

1. Mở trình duyệt Web (Firefox) trên Kali Linux.
2. Nhập địa chỉ IP của máy Ubuntu vào thanh URL: `http://192.168.1.130`
3. Giao diện đăng nhập GitLab xuất hiện:
    - **Username:** `root`
    - **Password:**

![image.png](attachment:cee291ab-6a4f-4257-9421-4745cb03f1d0:image.png)

**Bước 2: Thay đổi mật khẩu quản trị và tạo Project**

1. Sau khi vào được giao diện, thay đổi mật khẩu
2. Bấm vào nút **Create a project** -> Chọn **Create blank project** để tạo một kho chứa mới.
3. Đặt tên project (`test_gitlab_first`) rồi bấm **Create project**.

![image.png](attachment:8c32730a-187c-47fd-9a0a-e71a3c84210f:image.png)

**Bước 3: Thực hiện Clone và Push code từ Kali**
Trên giao diện project vừa tạo, bấm nút **Clone**, copy đường dẫn **Clone with HTTP**

Mở Terminal của Kali Linux lên và chạy chuỗi lệnh

Bash

```
# Tải repo từ máy chủ Ubuntu về máy Kali
git clone http://192.168.1.130/root/test_gitlab_first.git

# Di chuyển vào thư mục repo
cd test_gitlab_first

# Tạo một file note ghi chú
echo "# Ghi chú bài học dựng GitLab" > Readme.md

# Đẩy ngược lại lên máy chủ Ubuntu
git add .
git commit -m "Commit đầu tiên từ máy client Kali"
git push origin main
```

⇒ dựng lab thành công