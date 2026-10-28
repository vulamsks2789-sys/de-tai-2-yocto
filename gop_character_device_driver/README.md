BÁO CÁO ĐỒ ÁN HỆ THỐNG NHÚNG - YOCTO PROJECT(Gộp Character Device Driver vào bên trong kernel)

Tên đề tài: Biên dịch Hệ điều hành nhúng với Yocto Project trên RAS PI 3

Tên nhóm: Group 8

Giảng viên hướng dẫn: Thầy Huỳnh Hoàng Hà

Thành viên thực hiện:

Nguyễn Phúc Lộc - 24161109

Nguyễn Vũ Lâm - 24161105

Phan Thị Như Ý - 24161155

Nguyễn Văn Quý - 24161128

Trần Vạn Phước - 24161126

CÁC BƯỚC THỰC HIỆN
Quay về thư mục gốc poky

Lệnh thực hiện: cd ~/yocto/poky

Nạp môi trường Yocto (trỏ vào thư mục build-rpi)

Lệnh thực hiện: source oe-init-build-env build-rpi

Tạo layer mới (đặt ở thư mục ngoài để không bị lẫn vào thư mục build)

Lệnh thực hiện: bitbake-layers create-layer ../meta-mylayer

Thêm layer vừa tạo vào bblayers.conf

Lệnh thực hiện: bitbake-layers add-layer ../meta-mylayer

Tạo thư mục lưu trữ recipe

Lệnh thực hiện: mkdir -p ../meta-mylayer/recipes-kernel/my-driver/files

Copy file code C vào recipe

Lệnh thực hiện: cp ~/my_driver/my_driver.c ../meta-mylayer/recipes-kernel/my-driver/files/

Tạo file COPYING kiểm tra License

Lệnh thực hiện: echo "GPLv2" > ../meta-mylayer/recipes-kernel/my-driver/files/COPYING

Tạo file Makefile cho Yocto

Lệnh thực hiện: nano ../meta-mylayer/recipes-kernel/my-driver/files/Makefile

Nội dung file:

Makefile
obj-m := my_driver.o
SRC := $(shell pwd)
all:
	$(MAKE) -C $(KERNELSRC) M=$(SRC) modules

modules_install:
	$(MAKE) -C $(KERNELSRC) M=$(SRC) modules_install

clean:
	$(MAKE) -C $(KERNELSRC) M=$(SRC) clean
Tạo file BitBake Recipe (my-driver_0.1.bb)

Lệnh thực hiện: nano ../meta-mylayer/recipes-kernel/my-driver/my-driver_0.1.bb

Nội dung cấu hình:

Đoạn mã
SUMMARY = "Custom Character Device Driver for Raspberry Pi 3"
LICENSE = "GPL-2.0-only"
LIC_FILES_CHKSUM = "file://COPYING;md5=a14fe441b6663e8969e960a52006e395"

SRC_URI = "file://my_driver.c \
           file://Makefile \
           file://COPYING \
          "

S = "${WORKDIR}"

inherit module

KERNEL_MODULE_AUTOLOAD += "my_driver"
Khai báo nhúng Driver vào OS & tiến hành Build

Khai báo gom driver vào OS Image: echo 'IMAGE_INSTALL:append = " my-driver"' >> conf/local.conf

Bắt đầu build lại OS Yocto: bitbake core-image-minimal

Kiểm tra log của Driver

Lệnh thực hiện: dmesg | grep my_driver

Mô tả: Lệnh này giúp xem lại toàn bộ thông báo printk mà driver vừa ghi vào Kernel log.

Kiểm tra File Node của Driver trong /dev

Lệnh thực hiện: ls -l /dev/my_driver

Mô tả: Kiểm tra xem file thiết bị đã được tạo tự động chưa. Nếu thấy hiện thông báo file với chữ c ở đầu dòng (ví dụ crw-rw---- ...), tức là Character Device đã tạo thành công.

Đọc dữ liệu từ Driver

Lệnh thực hiện: cat /dev/my_driver

Mô tả: Nếu driver có hàm read(), lệnh này sẽ in ra chuỗi dữ liệu mà driver trả về.

Ghi dữ liệu vào Driver

Lệnh thực hiện: echo "Hello Driver" > /dev/my_driver

Mô tả: Lệnh này dùng để test hàm write() của driver.
