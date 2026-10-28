CÁCH NẠP FILE VÔ PI 3
Dùng lệnh cp (Copy) trong Terminal
Tại thư mục ~/yocto/poky/build-rp:
cp tmp/deploy/images/raspberrypi3/core-image-minimal-raspberrypi3.rootfs.wic.bz2 ~/Desktop/


Giải nén file Image (nếu là file nén .bz2)
- Đã dùng lệnh bzip2 -d (hoặc click chuột phải chọn Extract) để giải nén file core-image-minimal-...wic.bz2 ra thành file đĩa hoàn chỉnh dạng .wic ngay trên Desktop.

Mở phần mềm BalenaEtcher
- Mở trình quản lý file vào thư mục Downloads  balenaEtcher-linux-x64.
- Nhấp đúp chuột chạy file thực thi balena-etcher để mở giao diện nạp thẻ.

Thao tác nạp thẻ (Flash Image)
- Tại cửa sổ BalenaEtcher, bấm Flash from file  chọn file image .wic ngoài Desktop.
- Bấm Select target  chọn thẻ nhớ MicroSD đã cắm qua máy ảo VMware.
- Bấm nút Flash!  Nhập mật khẩu máy ảo Ubuntu (123123) để cấp quyền ghi ổ đĩa.
- Chờ Etcher ghi dữ liệu và kiểm tra (Verify) hoàn tất 100% báo Flash Completed!.

Bật UART và Đăng nhập thành công
- Mở phân vùng boot  thêm dòng enable_uart=1 ở cuối file config.txt  Ctrl + S để lưu.
- Click chuột phải Eject cả 2 phân vùng (boot và root) rồi rút thẻ nhớ.
- Cắm thẻ vào Raspberry Pi 3, kết nối dây UART CP2102 vào máy tính, mở PuTTY (COM6 / 115200).
- Cắm nguồn cho Pi 3, chờ log chạy xong rồi đăng nhập với user root (mật khẩu để trống) là vào thẳng terminal root@raspberrypi3:~#!
