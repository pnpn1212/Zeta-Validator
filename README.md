# Hướng dẫn ae chạy Validator Zeta

Hiện tại Guide này chỉ để ae chạy tham khảo vì Zetachain chưa công bố Whitelist. Nếu chạy mà gặp lỗi phân quyền thì vui lòng dừng hoặc có thể tham khảo google về phân quyền user và folder

## Bắt buộc phải dùng Ubuntu 22.04

## Thay đổi các thông số
```
nano /etc/security/limits.conf
```
Thêm các thông số này vào cuối:
```
*       soft    nproc   262144
*       hard    nproc   262144
*       soft    nofile  262144
*       hard    nofile  262144
```
```
nano /etc/sysctl.conf
```
Thêm các thông số này vào cuối:
```
fs.file-max=262144
```
## Tạo user Zetachain
```
useradd zetachain

su zetachain
```
## Tạo thư mục
```
mkdir -p /home/zetachain/.zetacored/bin

mkdir /home/zetachain/.zetacored/config
```
## Install
```
sudo apt update

sudo apt install -y jq curl git

jq --version
```
```
wget https://zetachain-external-files.s3.amazonaws.com/binaries/athens3/latest/zetacored-ubuntu-22-amd64

wget https://zetachain-external-files.s3.amazonaws.com/binaries/athens3/latest/zetaclientd-ubuntu-22-amd64
```
```
mv zetacored-ubuntu-22-amd64 /usr/bin/zetacored && chmod +x /usr/bin/zetacored

mv zetaclientd-ubuntu-22-amd64 /usr/bin/zetaclientd && chmod +x /usr/bin/zetaclientd

```
```
git clone https://github.com/zeta-chain/network-athens3.git && cd network-athens3
```
```
chmod +x ./scripts/*.sh

./scripts/node-setup.sh -o y
```
* Note: Nhớ lưu lại 24 ký tự wallet của Operator và Hotkey
## Update Genesis File
```
git switch main

git pull
```

## Start Validator
```
./scripts/start-zetacore.sh
```
```
SEEDIP=3.218.170.198
./scripts/start-zetaclient.sh -s $SEEDIP
```
```
pkill zetacored
```
## SystemD
* Note: Chỗ này ae nên quay lại root mới có thể lưu đc file nhé
```
nano /etc/systemd/system/zetacored.service
```
Thêm các thông số ở dưới vào:
```
[Unit]
Description=Zetacored Service
After=multi-user.target
StartLimitIntervalSec=0
[Install]
WantedBy=multi-user.target
[Service]
Type=simple
Restart=always
RestartSec=90
User=zetachain
LimitNOFILE=262144
ExecStart=zetacored start --home /home/zetachain/.zetacored/ --log_format json  --log_level info --moniker <Thay chỗ này thành tên Node của bạn>
```
```
systemctl start zetacored
```
## Check logs
```
journalctl -o cat -f -u zetacored
```
