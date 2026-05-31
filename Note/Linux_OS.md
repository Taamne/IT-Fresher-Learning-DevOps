## Linux

### Overview

- Multi-user, multi-processing, multitasking, multithreading
- **FHS (Filesystem Hierarchy Standard):** hệ thống tệp phân cấp, bắt đầu từ thư mục gốc `/`

### Hierarchical file system (FHS)

- `/` (root): thư mục gốc
- `/bin/`: chứa binary commands (move, remove…)
- `/boot/`: chứa các file tĩnh liên quan quá trình reboot
- `/dev/`: device files
- `/etc/`: file cấu hình system
- `/home/`: thư mục người dùng thường (ví dụ: `/home/tam/`)
- `/lib/`: thư viện hệ thống
- `/media/`: mount thiết bị rời
- `/mnt/`: mount tạm (thiết bị, hoặc mount từ internet ⇒ mount point)
- `/opt/`: packages/phần mềm bổ sung
- `/sbin/`: system binaries
- `/srv/`: service data
- `/tmp/`: file tạm
- `/usr/`: userland programs & data
- `/var/`: dữ liệu thay đổi (logs, DB như mysql, postgre…)
- `/root/`: home của user root (super user)
- `/proc/`: thông tin kernel/hardware/memory

### Bash (Linux shell)

- Command processor: trung gian giữa người dùng và HĐH

### Package & repository

- Repository: kho lưu trữ trực tuyến (official / unofficial / third-party)
- **APT:** cài đặt, tìm kiếm, lấy thông tin gói (apt-get, apt-cache)
    - CentOS: yum/dnf
- dpkg (Debian package)

### Basic commands

#### 1) File handling

- `mkdir`: tạo thư mục mới (ví dụ: `mkdir zahid`)
- `ls`: liệt kê (ví dụ: `ls`, `ls -l`)
- `cd`: chuyển thư mục (ví dụ: `cd zahid`)
- `pwd`: in đường dẫn hiện tại
- `vim`: trình soạn thảo
- `cp`: sao chép (ví dụ: `cp sample.txt sample_copy.txt`)
- `mv`: di chuyển/đổi tên (ví dụ: `mv old.txt new.txt`)
- `rm`: xóa (ví dụ: `rm file1.txt` hoặc `rm -rf`)
- `find`: tìm kiếm tệp
- `history`: lịch sử lệnh
- `tar`, `gzip`: nén/giải nén

#### 2) Text processing

- `cat`: xem nội dung/kết hợp tệp
- `echo`: in ra một dòng (ví dụ: `echo I love Debian`)
- `grep`: tìm dòng khớp pattern
- `wc`: đếm dòng/từ/byte
- `touch`: tạo file rỗng/cập nhật timestamp

#### 3) System administration

- `chmod`: đổi quyền (ví dụ: `chmod 744 calculate.sh`)
- `chown`: đổi owner/group
- `su`: chuyển user / lên superuser
- `passwd`: đổi mật khẩu
- `who`: xem user đang đăng nhập

#### 4) Advanced commands

- `reboot`: khởi động lại
- `poweroff`: tắt máy
- `man`: đọc tài liệu lệnh (ví dụ: `man ls`)