## **Cài đặt máy chủ GitLab một cách tự động hóa bằng công cụ Vagrant**.

sử dụng VMWare workstation

### Bước 1: Cài đặt  Vagrant - VMware

1. **Tải và cài đặt Vagrant VMware Utility:** tải *Vagrant VMware Utility* từ trang chủ của HashiCorp, tải file `.msi` dành cho Windows và cài đặt (Next -> Finish).
2. Cài đặt Vagrant cho window bản AMD64
3. **Cài đặt Plugin:** để kết nối Vagrant
    
    ```
    vagrant plugin install vagrant-vmware-desktop
    ```
    

### Bước 2: Chỉnh sửa lại kịch bản (`Vagrantfile`)

Cấu trúc kịch bản dành cho VMware sẽ khác một chút ở phần khai báo phần cứng so với virtualBox.  bằng ngôn ngữ Ruby

```
Vagrant.configure("2") do |config|
  # Sử dụng box 'bento' vì nó được tối ưu độ tương thích cực tốt cho cả VMware và VirtualBox
  config.vm.box = "bento/ubuntu-20.04"

  # Cấu hình IP tĩnh cho máy chủ GitLab
  config.vm.network "private_network", ip: "192.168.56.50"

  # Cấu hình phần cứng chuyên biệt cho VMware
  config.vm.provider "vmware_desktop" do |v|
    v.vmx["displayname"] = "GitLab-Auto-Server-VMware"
    v.vmx["memsize"] = "4096"      # Cấp 4GB RAM
    v.vmx["numvcpus"] = "2"        # Cấp 2 CPU
  end

  # KỊCH BẢN TỰ ĐỘNG HÓA CÀI ĐẶT (Phần này giữ nguyên)
  config.vm.provision "shell", inline: <<-SHELL
    echo "Bắt đầu cập nhật hệ thống..."
    sudo apt-get update -y

    echo "Cài đặt các gói bổ trợ..."
    sudo apt-get install -y curl openssh-server ca-certificates tzdata perl

    echo "Thêm kho lưu trữ của GitLab..."
    curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

    echo "Tiến hành cài đặt GitLab CE..."
    sudo EXTERNAL_URL="http://192.168.56.50" apt-get install gitlab-ce -y

    echo "Hoàn tất cài đặt! Lấy mật khẩu Root mặc định:"
    sudo cat /etc/gitlab/initial_root_password
  SHELL
end
```

### Bước 3: Khởi chạy trên VMware

tại Terminal đang mở thư mục chứa file `Vagrantfile` đó

```
vagrant up --provider=vmware_desktop
```

 VMware sẽ tự động chạy ngầm, tải Ubuntu về, cấu hình RAM/CPU và tự động chạy toàn bộ hàng chục dòng lệnh cài đặt GitLab kia. 

![image.png](attachment:76e149a0-af24-4c71-a993-ab3df6101589:image.png)

![image.png](attachment:31f0bc19-1c1b-46c6-9168-8a3d91593af4:image.png)