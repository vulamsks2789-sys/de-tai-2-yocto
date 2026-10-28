1. Quay về thư mục gốc poky:
 cd ~/yocto/poky
 
2. Nạp môi trường Yocto (trỏ vào thư mục build-rpi của bạn): source oe-init-build-env build-rpi

 3.Tạo layer mới (đặt ở thư mục ngoài để không bị lẫn vào thư mục build): bitbake-layers create-layer ../meta-mylayer

4.Thêm layer vừa tạo vào bblayers.conf: bitbake-layers add-layer ../meta-mylayer 

5.Tạo thư mục lưu trữ recipe:
mkdir -p ../meta-mylayer/recipes-kernel/my-driver/files 

6.Copy file code C vào recipe:
cp ~/my_driver/my_driver.c ../meta-mylayer/recipes-kernel/my-driver/files/ 

7.Tạo file COPYING kiểm tra License 
echo "GPLv2" > ../meta-mylayer/recipes-kernel/my-driver/files/COPYING 

8.Tạo file Makefile cho Yocto:
nano ../meta-mylayer/recipes-kernel/my-driver/files/Makefile
( Dán nội dung vào )


obj-m := my_driver.o

SRC := $(shell pwd)

all:
	$(MAKE) -C (KERNELSRC)M=(SRC) modules

modules_install:
	$(MAKE) -C (KERNELSRC)M=(SRC) modules_install

clean:
	$(MAKE) -C (KERNELSRC)M=(SRC) clean

9.Tạo file BitBake Recipe (my-driver_0.1.bb) : nano ../meta-mylayer/recipes-kernel/my-driver/my-driver_0.1.bb

Dán toàn bộ nội dung cấu hình này: 

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

10.Khai báo nhúng Driver vào OS & tiến hành Build:

 Khai báo gom driver vào OS Image: echo 'IMAGE_INSTALL:append = " my-driver"' >> conf/local.conf 

 Bắt đầu build lại OS Yocto: bitbake core-image-minimal 

			
11.Kiểm tra log của Driver: dmesg | grep my_driver
(Lệnh này giúp xem lại toàn bộ thông báo printk mà driver của bạn vừa ghi vào Kernel log). 


12.Kiểm tra File Node của Driver trong /dev:

ls -l /dev/my_driver
(Kiểm tra xem file thiết bị đã được tạo tự động chưa. Nếu thấy hiện thông báo file với chữ c ở đầu dòng, ví dụ crw-rw---- ..., tức là Character Device đã tạo thành công).


13.Đọc dữ liệu từ Driver: cat /dev/my_driver
(Nếu driver có hàm read(), lệnh này sẽ in ra chuỗi dữ liệu mà driver trả về). 


14.Ghi dữ liệu vào Driver: 
echo "Hello Driver" > /dev/my_driver 
(Lệnh này sẽ test hàm write() của driver). 
