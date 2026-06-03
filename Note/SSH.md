## **Đăng nhập server dùng SSH Keys, không cần Password**

SSH Key là phương thức xác thực đăng nhập với máy chủ qua SSH thông qua đối chiếu 1 cặp key bao gồm private key (chìa khóa) và public key (ổ khóa) tương ứng.

sử dụng giao thức challange-response

Đăng nhập thông thường qua username root và password ⇒ dễ brute force attack ⇒ sử dụng SSH Keys bảo mật hơn nhiều

- private key: file txt  chứa dữ liệu được mã hóa, sử dụng thay cho password, được lưu trữ trên máy tính. độ dài ít nhất 2048 bit và được mã hóa bằng cụm mật khẩu. định dạng tuy thuộc công cụ tạo key
- public key: một file txt chứa dữ liệu được mã hóa , được đặt ở bất kì một server nào. định dạng .pub
- khi gửi yêu cầu đăng nhập kèm private key, server sẽ kiểm tra sự trùng khớp với public key trên server nhằm xác thực yêu cầu đăng nhập
1. Tạo SSH Keys
    - tạo cặp keys qua terminal với ssh-keygen. trong quá trình cài đặt chương trình sẽ hỏi nơi lưu key và mật khẩu sử dụng private
    - 
        
        !image.png
        
    - Keys được tạo theo thuật toán mã hóa RSA, độ dài 2048bit và lưu tại `/root/.ssh/`. Trong đó, Private Key là *id_rsa* còn Public Key là *id_rsa.pub*, đều là OpenSSH Keys
2. thêm public key vào vps
    - Đối với server Linux, cần lưu thông tin Public Key tại `~/.ssh/authorized_keys` để xác thực đăng nhập sử dụng SSH Keys.
    - Copy toàn bộ nội dung Public key (dạng `ssh-rsa AAAA...`) chèn thêm phía cuối file. Nhấn Ctrl+O để lưu lại nội dung và Ctrl+X để thoát khỏi editor.
    - **Bật chế độ đăng nhập bằng SSH Keys**: kích hoạt (uncomment) các tham số sau trong SSH Config tại `/etc/ssh/sshd_config`
        - PubkeyAuthentication yes
        - AuthorizedKeysFile .ssh/authorized_keys
    - Sau đó, khởi động lại SSH Service
        
        `# service sshd restart`
        
3. Sử dụng ssh keys
    - Để sử dụng SSH Keys truy cập VPS, chỉ cần login thông qua các phần mềm SSH  và lựa chọn file Private Key đã tạo khi trước.
4. cấu hình sử dụng SSH Keys
    - Để gia tăng bảo mật, nên thay đổi port truy cập SSH mặc định (22)
    - Bên cạnh đó, cũng nên vô hiệu hóa đăng nhập sử dụng mật khẩu bằng cách chỉnh sửa tham số sau trong `/etc/ssh/sshd_config`:
    
    ```
    PasswordAuthentication no
    ```
    
    - Sau đó, khởi động lại SSH Service
    
    ```
    # service sshd restart
    ```
    

https://quickref.me/

## Giả lập các Case không login được và xử lý từ Boot

**"Xử lý từ Boot" (Boot-level Troubleshooting / Rescue Mode)** là việc can thiệp vào quá trình khởi động của hệ điều hành Linux trước khi nó tải xong giao diện hoặc các dịch vụ mạng (như SSH).

thông thường, khi linux khởi động, đi qua các bước:

BIOS/UEFI ⇒ bootloader  (GRUB) ⇒ kernel & initramfs ⇒ tiến trình init (SYStemd)

màn hình BIOS/UEFI

- Đầu tiên là logo của nhà sản xuất phần cứng (Dell, HP, v.v.) hoặc các dòng chữ kiểm tra bộ nhớ (POST).
- Ngay sau đó là **Menu GRUB**. Đây là màn hình quan trọng nhất để xử lý sự cố. Nó thường là một danh sách đen trắng đơn giản, cho phép chọn phiên bản Kernel để khởi động.

màn hình kernel loading :

Màn hình thường đen xì trong vài giây.

*(Nếu hệ thống được cấu hình hiện log)*: sẽ thấy hàng trăm dòng chữ trắng chạy cực nhanh. Đây là lúc Kernel đang nhận diện phần cứng (CPU, RAM, ổ cứng, card mạng).
⇒ giai đoạn này thường không can thiệp được bằng bàn phím

màn hình init/Systemd & service

Kernel đã nạp xong và nhường quyền cho tiến trình đầu tiên (thường là `systemd`).

trên desktop thấy logo Ubuntu và một vòng tròn tải (Loading).
Trên Server, sẽ thấy các dòng log được căn lề, có chữ **`[ OK ]`** màu xanh lá cây hiện lên liên tục ở đầu dòng. Ví dụ: `[ OK ] Started Network Manager`, `[ OK ] Started SSH Service`

màn hình Hoàn tất

Khi mọi dịch vụ đã khởi động xong.
hiển thị nhập login

Khi hệ thống gặp lỗi nghiêm trọng (hỏng ổ cứng, sai cấu hình mount, lỗi kernel) hoặc khi mất hoàn toàn quyền truy cập (mất mật khẩu root), hệ điều hành sẽ không thể hoàn thành chu trình này. Lúc đó, phải dùng **Console** (màn hình kết nối trực tiếp) để can thiệp vào **Bootloader (GRUB)**, ép hệ thống khởi động vào một môi trường tối giản (Single-user mode hoặc Emergency mode) để sửa lỗi.
****

### Case 1: quên password root

Khi quên mật khẩu root, không thể dùng `su` hay `sudo` được nữa.

**Định nghĩa tình huống:** Hệ thống hoạt động bình thường, nhưng người quản trị mất quyền kiểm soát cao nhất. Cần can thiệp vào kernel lúc khởi động để ép hệ thống cho phép đổi mật khẩu mà không cần hỏi mật khẩu cũ.

**Cách xử lý (Can thiệp vào GRUB):**

1. Khởi động lại máy chủ (Reboot).
2. Ngay khi màn hình **GRUB menu** (chọn hệ điều hành) hiện lên, bấm phím `e` để chỉnh sửa (edit) thông số khởi động.

!image.png

1. Tìm dòng bắt đầu bằng chữ `linux` hoặc `linux16`. Di chuyển con trỏ xuống cuối dòng đó và thêm tham số `init=/bin/bash` (với Ubuntu/Debian) hoặc `rd.break` (với CentOS/RedHat).
2. Nhấn `Ctrl + X` hoặc `F10` để tiếp tục quá trình boot với tham số vừa thêm.
3. Lúc này, sẽ vào được giao diện dòng lệnh với quyền root. Tuy nhiên, hệ thống file đang ở trạng thái chỉ đọc (Read-only). Cần cấp quyền ghi:
`mount -o remount,rw /`
4. Tiến hành đổi mật khẩu: `passwd root` và nhập mật khẩu mới.
5. *(Lưu ý quan trọng cho các hệ thống dùng SELinux như CentOS)*: Gõ lệnh `touch /.autorelabel` để cập nhật lại nhãn bảo mật, nếu không sẽ không login được.
6. Khởi động lại: `exec /sbin/init` hoặc `reboot -f`.

### **Case 2: Cấu hình sai `/etc/fstab` hoặc lỗi Mount ổ đĩa**

File `/etc/fstab` chứa danh sách các phân vùng ổ đĩa tự động gắn (mount) khi khởi động.

**Định nghĩa tình huống:** sau khi vừa thêm một ổ cứng mới và ghi sai cú pháp vào file `/etc/fstab`. Khi khởi động, Linux cố gắng đọc file này, báo lỗi, quá trình boot bị treo cứng và đẩy vào **Emergency Mode** (Chế độ khẩn cấp). Máy chủ lúc này mất hoàn toàn kết nối mạng.

**Cách xử lý:**

1. Tại màn hình Emergency Mode, hệ thống thường sẽ yêu cầu: *"Give root password for maintenance"*. nhập mật khẩu root vào.
2. Lúc này, thư mục gốc `/` đang bị khóa ở chế độ Read-only để bảo vệ dữ liệu.  phải mở khóa để có thể sửa file:
`mount -o remount,rw /`
3. Dùng trình soạn thảo văn bản (vi/nano) mở file fstab:
`vi /etc/fstab`
4. Tìm đến dòng vừa thêm vào bị sai cú pháp. Sửa lại cho đúng, hoặc cách nhanh nhất là thêm dấu `#` ở đầu dòng để vô hiệu hóa nó (Comment out).
5. Lưu file lại và gõ lệnh `reboot` để khởi động lại máy chủ.

### Case 3: Mất Private Key (SSH) hoặc cấu hình sai tường lửa

- **Định nghĩa tình huống:** Máy chủ vẫn hoạt động hoàn hảo, nhưng lỡ tay xóa mất file Private Key trên laptop của mình, hoặc lỡ tay cấu hình Firewall chặn luôn Port 22 (SSH)
- **Cách xử lý (Sử dụng Out-of-band Console):**
    1. Trường hợp này không cần can thiệp từ GRUB (không cần khởi động lại máy).
    2. mở màn hình Console trực tiếp (nếu dùng lab thì là cửa sổ VirtualBox/VMware, nếu làm thật thì dùng giao diện web của nhà cung cấp Cloud).
    3. Đăng nhập bằng tài khoản (user thường hoặc root) và mật khẩu cục bộ.
    4. Mở file cấu hình SSH:
    `sudo vi /etc/ssh/sshd_config`
    5. Tạm thời cho phép đăng nhập bằng mật khẩu trở lại bằng cách tìm dòng `PasswordAuthentication` và đổi thành `yes`.
    6. Khởi động lại dịch vụ SSH: `sudo systemctl restart sshd` (hoặc `ssh`).
    7. Quay lại laptop cá nhân, SSH vào bằng mật khẩu, tạo lại Key mới, ném Public Key lên lại server, rồi mới vào tắt `PasswordAuthentication` đi cho an toàn.