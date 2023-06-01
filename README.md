# Hướng dẫn ae chạy Validator Zeta

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
- Enter
mv zetaclientd-ubuntu-22-amd64 /usr/bin/zetaclientd && chmod +x /usr/bin/zetaclientd
- Enter
```
```
git clone https://github.com/zeta-chain/network-athens3.git && cd network-athens3
```
```
chmod +x ./scripts/*.sh
./scripts/node-setup.sh -o y
```
* Note: Nhớ lưu lại 24 ký tự của wallet
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
