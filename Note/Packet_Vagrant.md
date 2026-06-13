# Devops Introduction Packet Vagrant

## 1. HashiCorp Packer

Packer là công cụ dùng để tạo các **Machine Images** (hình ảnh máy ảo) tự động.

- **Machine Image**: Là tài nguyên lưu trữ mọi cấu hình, quyền hạn và siêu dữ liệu cần thiết để tạo một máy ảo.**2**
- **Định dạng Template**: Packer sử dụng các tệp cấu hình viết bằng **JSON** hoặc **HCL2** (HashiCorp Configuration Language).**3**
- **Lợi ích**:
    - Tạo hình ảnh qua template, không cần cấu hình thủ công trên VM.**4**
    - Tự động hóa hoàn toàn, dễ dàng tích hợp vào quy trình CI/CD.**5**
    - Tạo ra các hình ảnh đồng nhất trên nhiều nền tảng đám mây khác nhau.**6**

### Các thành phần chính của Packer (Packer Breakdown)

- **Builders**: Định dạng nền tảng mong muốn (như AWS, VirtualBox) và thông tin xác thực (API key, source images).**7**
- **Provisioners**: Định nghĩa cách cấu hình hình ảnh bằng các công cụ quản lý cấu hình hiện có (Shell, File, Ansible...).**8**
- **Post-Processors**: Chạy sau khi hình ảnh được tạo để xử lý các tệp đầu ra (artifact) như import lên đám mây.**9**
- **Communicator**: Cách Packer giao tiếp với máy ảo trong quá trình tạo (mặc định là SSH, hoặc WinRM cho Windows).**10**
- **Artifact**: Kết quả cuối cùng của quá trình build, thường là machine image.**11**

## 2. Vagrant

- **Định nghĩa**: Vagrant là công cụ quản lý quy trình làm việc trong môi trường phát triển (Development Environment) thông qua một tệp cấu hình duy nhất.
- **Box (Golden Image)**: Là các hình ảnh máy ảo cơ sở (ví dụ: CentOS, Ubuntu) dùng để khởi tạo môi trường.
- **Vagrantfile**: Tệp tin chứa các mô tả về môi trường, giúp việc chia sẻ và tái lập môi trường chính xác giữa các thành viên.
- **Cơ chế hoạt động**: Vagrant tương tác với các Hypervisor (như VirtualBox) để cung cấp máy ảo và kết nối với các hệ thống quản lý cấu hình (Provisioning) để cài đặt máy ảo đó.

### 2. Quản lý Plugins

Vagrant cho phép mở rộng tính năng thông qua các plugin:

- **Cài đặt**: `vagrant plugin install <tên_plugin>`.
- **Cập nhật**: `vagrant plugin update <tên_plugin>`.
- **Gỡ lỗi/Sửa chữa**: `vagrant plugin repair`.
- **Gỡ cài đặt**: `vagrant plugin uninstall <tên_plugin>`.
- **Xóa sạch**: `vagrant plugin expunge` (xóa tất cả plugin và dữ liệu liên quan).

### 3. Các lệnh quản lý VM thông dụng

Create a Vagrant environment

- Boxes: virtual machine are based upon golden images  “The vagant cloud box repository. được cung cấp bởi:
    - Hashicorp
    - mã nguồn mở
    - tự tạo
- **Khởi tạo**: `vagrant init` (tạo file Vagrantfile mẫu).
- **Khởi động**: `vagrant up`. #tìm kiếm file Vagrantfile
- **Kiểm tra trạng thái**: `vagrant status` (cho 1 máy) hoặc `vagrant global-status` (cho tất cả VM trên máy chủ).
- **Tắt máy**: `vagrant halt` (tắt an toàn).
- **Tạm dừng**: `vagrant suspend` (lưu lại trạng thái tại thời điểm hiện tại).
- **Xóa máy**: `vagrant destroy`.
- **Truy cập SSH**: `vagrant ssh`.
- Start suspended VM(s): vagrant reload <hostname>
- Delete VM(s): vagrant destroy <hostname>  #nhưng vẫn giữ lalj vagrant file
- 

### 4. Tính năng nâng cao

### Basic Syncing

- CLI Upload: vagrant upload <source>  <destination> [hostname]
- 

### a. Synced Folders (Đồng bộ thư mục)

Giúp tự động đồng bộ tệp tin giữa máy chủ (host) và máy ảo (guest).

- **Cấu hình**: `config.vm.synced_folder "nguồn/", "đích/"`
- **Các kiểu đồng bộ**:
    - **NFS**: Tốc độ cao, thường dùng cho Linux.
    - **Rsync**: Đồng bộ một chiều, có tính năng `rsync__auto` để theo dõi thay đổi.
    - **SMB**: Thường dùng trên môi trường Windows.

### b. Networking (Mạng)

- **Port Forwarding**: Chuyển tiếp cổng (ví dụ: truy cập cổng 8081 của máy chủ để vào cổng 8080 của máy ảo).
- **Private Network**: Mạng nội bộ giữa host và guest, có thể dùng DHCP hoặc đặt IP tĩnh.
- **Public Network**: Cho phép máy ảo xuất hiện như một thiết bị trong mạng vật lý của bạn (Bridged Network).

### c. Provisioning (Cấu hình tự động)

Đảm bảo máy ảo khi dựng xong sẽ có đầy đủ phần mềm và cấu hình cần thiết.

- **Shell**: Chạy các lệnh trực tiếp (inline), script nhúng hoặc file script bên ngoài.
- **File**: Sao chép file/thư mục từ host sang guest.
- **Ansible**: Sử dụng Ansible từ máy chủ (host) hoặc cài đặt và chạy trực tiếp trên máy ảo (Ansible Local).
- **Lệnh chạy lại**: `vagrant provision` hoặc `vagrant up --provision`.

### d. Snapshots (Ảnh chụp trạng thái)

Lưu lại trạng thái máy ảo để khôi phục khi môi trường bị lỗi.

- **Lưu**: `vagrant snapshot save <tên>`.
- **Khôi phục**: `vagrant snapshot restore <tên>`
- **Push/Pop**: Lưu nhanh một snapshot duy nhất và xóa ngay sau khi khôi phục
- vagrant snapshot list
- vagrant snapshot delete [vm] <name>

### 5. Quản lý Boxes (Hình ảnh máy ảo)

- **Liệt kê**: `vagrant box list`
- **Kiểm tra cập nhật**: `vagrant box outdated`
- **Dọn dẹp**: `vagrant box prune` (xóa các phiên bản cũ)
- **Đóng gói máy ảo hiện tại thành box**: `vagrant package <tên> --output <file.box>`