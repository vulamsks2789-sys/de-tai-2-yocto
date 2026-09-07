BÁO CÁO ĐỒ ÁN HỆ THỐNG NHÚNG - YOCTO PROJECT (MÔ PHỎNG QEMU - TUẦN 4)

Tên đề tài: Biên dịch Hệ điều hành nhúng với Yocto Project trên môi trường giả lập QEMU

Tên nhóm: Group 8

Giảng viên hướng dẫn: Thầy Huỳnh Hoàng Hà

Thành viên thực hiện:
1. Nguyễn Vũ Lâm - 24161105
2. Nguyễn Phúc Lộc - 24161109
3. Phan Thị Như Ý - 24161155
4. Nguyễn Văn Quý - 24161128
5. Trần Vạn Phước - 24161126


1.1. Yêu cầu hệ thống (Host System)
Để biên dịch Yocto Project không bị lỗi, máy chủ (Host System) cần đáp ứng các yêu cầu tối thiểu sau:
- Hệ điều hành: Ubuntu 22.04 LTS / Ubuntu 24.04 LTS (64-bit)
- Dung lượng đĩa trống: Tối thiểu 80 GB - 100 GB
- Bộ nhớ RAM: Tối thiểu 8 GB (khuyến nghị 16 GB)

1.2. Cài đặt các gói phụ thuộc (Install Host Packages)

Cập nhật danh sách gói và cài đặt toàn bộ các thư viện/công cụ bắt buộc trên máy host trước khi tiến hành build:

$ sudo apt update
$ sudo apt install -y gawk wget git diffstat unzip texinfo gcc build-essential \ chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils \
 iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \ python3-subunit mesa-common-dev zstd liblz4-tool file

1.3. Clone Poky

Đầu tiên tạo thư mục để build:

$ mkdir yocto
$ cd yocto

 clone Poky repo để build từ trang chủ yoctoproject (sử dụng branch wrynose):

$ git clone git://git.yoctoproject.org/poky
$ cd poky
$ git checkout -b wrynose origin/wrynose

1.4. Thiết lập môi trường build cho QEMU

Thiết lập môi trường build hệ thống cho mô phỏng QEMU:

$ source oe-init-build-env build-qemu

1.5. Cấu hình build trong local.conf

Cấu hình máy mục tiêu là giả lập QEMU x86-64 trong file conf/local.conf:

$ vim conf/local.conf

Chỉnh sửa hoặc kiểm tra dòng MACHINE trong file local.conf:

MACHINE ?= "qemux86-64"

1.6. Tiến hành biên dịch và chạy mô phỏng QEMU

Chạy lệnh bitbake để tiến hành biên dịch image tối giản (core-image-minimal):

$ bitbake core-image-minimal

Sau khi tiến trình biên dịch hoàn tất 100%, tiến hành chạy mô phỏng giao diện hệ điều hành bằng QEMU:

$ runqemu qemux86-64
	
