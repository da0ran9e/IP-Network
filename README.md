# IP-Network
⚙️💻
## Lab01: Kết nối liên mạng với static routing (version 4.0)
1. Tạo máy ảo kết nối Internet qua máy host:
   - Download ISO image hệ điều hành [Ubuntu Server 16.04.7 (bản 64 bit)](https://releases.ubuntu.com/16.04/ubuntu-16.04.7-server-amd64.iso)
   - Tạo VM trong Virtualbox 1G RAM
   - Thiết lập kết nối mạng: Adapter 1: Enable, Attached to: NAT. Các Adapter khác thiết lập không kết nối (Not attached). Chỉnh sửa 2 byte cuối của các MAC address để dễ quản lý. Byte #1 là ký hiệu router (01, 02, 03, v.v..) và byte số 2 là ký hiệu network trong router (01,02,03,04)
     ![image](https://github.com/user-attachments/assets/e216454d-82d7-445a-88b2-05526382c721)
     ![image](https://github.com/user-attachments/assets/ddbc1ccb-bf77-40a0-82a6-fe9fe8da3497)
  2. Config
     

