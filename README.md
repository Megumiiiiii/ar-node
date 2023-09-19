<div align="center">
 
# AR

</div>

<div align="center">

[![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23e609e6)](https://github.com/sponsors/Megumiiiiii)

[![Twitter](https://img.shields.io/twitter/follow/sarcophagusio?style=social)](https://twitter.com/megumii_tez)

[![](https://img.shields.io/static/v1?label=Telegram&message=%E2%9D%A4&logo=Telegram&color=%23e609e6)](https://t.me/KatouMegumii)

<img align="top" src="https://komarev.com/ghpvc/?username=Megumiiiiii&color=e609e6&style=plastic&label=Visitors" height='35'/>


</div>

#

<div align="center">
  
## ${\color{violet} COPAS \space SERTAIN \space SUMBER \space SU}$

### ${\color{violet} DIKIRA \space BIKIN \space GINIAN \space GAK \space PERLU \space USAHA \space APA}$ 

</div>

## Official Links

- [Announcement](https://ar.io/testnet)
- [Docs](https://ar.io/docs/)
- [Twitter](https://x.com/ar_io_network)
- [Discord](https://discord.gg/Y3DJuFb3qE)
- [Website](https://ar.io)

## NOTE!


Perlu modal token $AR sedikit, untuk biaya gas fee. $AR bisa beli di binance, atau ngecer
![Screenshot_58](https://github.com/Megumiiiiii/ar-node/assets/98658943/de2cfade-8c74-4049-9893-d5533cd438a5)

## Spek Minimal

| Spek | Ukuran |
|----------|----------|
| CPU | 4 |
| RAM | 4 GB |
| SSD | 500 GB |
| Bandwith | 50 Mbps |

## Install Docker dkk

```
sudo apt update; sudo apt upgrade -y
```

```
sudo apt-get update && sudo apt install jq git certbot nginx sqlite3 build-essential -y && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```

### Install Nodejs & Yarn

#### Nodejs

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 16.15.1
nvm use 16.15.1
```

#### Yarn

```
curl -sSL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update -y
sudo apt-get install yarn -y
```

## Open Port

```
sudo ufw allow ssh; sudo ufw allow 80; sudo ufw allow 443; sudo ufw enable
```

## Clone AR Repo

```
git clone https://github.com/ar-io/ar-io-node.git
cd ~/ar-io-node
```

## Atur `.env`

```
nano .env
```

- isi dengan ini, `domainmu.zzz` ganti dengan domainmu, `Password123` ganti dengan passwordmu, bebassss, `Address` ganti dengan wallet addressmu dari [ARConnect](https://chrome.google.com/webstore/detail/arconnect/einnioafmpimabjcddiinlhmijaionap)

```
GRAPHQL_HOST=arweave.net
GRAPHQL_PORT=443
START_HEIGHT=1000000
ARNS_ROOT_HOST=domainmu.zzz
ADMIN_API_KEY=Password123
AR_IO_WALLET=Address
```

- Simpan, `CTRl+X` `Y` `Enter`

## Atur Domain

### Masuk ke Manage Domain, terserah beli dimana. Pastikan domain tidak digunakan untuk projek lain ataupun website pribadi

1. Atur A Record yang mengarah ke IP VPS MU, beri nama `@`
2. Atur A Record yang mengarah ke IP VPS MU, beri nama `*`

3. ![Screenshot_34](https://github.com/Megumiiiiii/ar-node/assets/98658943/7d878692-bd6b-4920-8f60-21e77d9fc15c)

## Atur SSL

Ganti `EmailMU@gmail.com` dengan emailmu , seluruh `domainmu.zzz` dengan domain mu
```
sudo certbot certonly --manual --preferred-challenges dns --email EmailMu@gmail.com -d domainmu.zzz -d '*.domainmu.zzz'
```


#### dilangkah ini akan ada arahan untuk mengatur `TXT Records`, ikuti saja sesuai arahan yang ada
- copy itu, jangan Enter/Contine dulu
- ![Screenshot_57](https://github.com/Megumiiiiii/ar-node/assets/98658943/87c1aa40-f464-4c7e-b86c-d5a5f4fe810e)
- pergi ke domain manager, tambahkan DNS Record
- pilih TXT
- name isi dengan `_acme-challenge`
- value isi dengan yang tadi copy di vps
- ![Screenshot_58](https://github.com/Megumiiiiii/ar-node/assets/98658943/0e506705-878a-437e-ac7f-1f19b6cce535)
- lalu cek https://dnschecker.org/#TXT/
- ![Screenshot_59](https://github.com/Megumiiiiii/ar-node/assets/98658943/9e66bce1-9ebe-4a34-8ec2-c690ea865344)
- kalo udh centang semua dan isinya benar, baru ke vps lagi, continue


## Set Nginx

```
rm /etc/nginx/sites-available/default
nano /etc/nginx/sites-available/default
```

- isi dengan ini, ganti seluruh `domainmu.zzz` dengan domain mu

```
# Force redirects from HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name domainmu.zzz *.domainmu.zzz;

    location / {
        return 301 https://$host$request_uri;
    }
}

# Forward traffic to your node and provide SSL certificates
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name domainmu.zzz *.domainmu.zzz;

    ssl_certificate /etc/letsencrypt/live/domainmu.zzz/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/domainmu.zzz/privkey.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
    }
}
```

- Simpan, `CTRl+X` `Y` `Enter`

#### Cek Nginx

```
sudo nginx -t
```

#### Restart nginx dan cek ulang

```
sudo service nginx restart
sudo nginx -t
```

![Screenshot_35](https://github.com/Megumiiiiii/ar-node/assets/98658943/27f4b34d-8782-4170-a8f8-5e36253a8344)

**OK!!**


## Setup Node

```
cd ~/ar-io-node
sudo docker compose up -d --build
```

Tunggu sampai selesai...........

#### Selanjutnya Cek apakah berjalan

- buka ini di browser mu, `IP.VPS.MU` ganti dengan IP VPS MU

```
http://IP.VPS.MU:3000/3lyxgbgEvqNSvJrTX2J7CfRychUD5KClFhhVLyTPNCQ
```

- Jika ada orang ini, SELAMAT!
- ![3lyxgbgEvqNSvJrTX2J7CfRychUD5KClFhhVLyTPNCQ](https://github.com/Megumiiiiii/ar-node/assets/98658943/37ca28a4-b9bd-454e-b9ec-62c4aee48796)

## Jika ada update mendatang

### Stop node

```
cd ~/ar-io-node
sudo docker compose down
```

### Update repo

```
git pull
```

### Update image

```
sudo docker compose pull
```

### Re-build

```
sudo docker compose up -d --build
```

## Request Test Token di discord
- Join [Discord](https://discord.gg/Y3DJuFb3qE)
- Ke channel `#testnet`
- Gunakan command `/apply`
- Isi survey, lalu tunggu dikirim

***

***

# Setelah Mendapat Test Token

## Clone Repo Contract

```
cd
git clone https://github.com/ar-io/testnet-contract.git
cd ~/testnet-contract
```

```
yarn install
```

```
yarn build
```

## Buka Extension Wallet [ARConnect](https://chrome.google.com/webstore/detail/arconnect/einnioafmpimabjcddiinlhmijaionap)

- Export dan beri nama `key.json`
![Screenshot_38](https://github.com/Megumiiiiii/ar-node/assets/98658943/05eaa252-765e-46be-98c5-56adf22a7c63)

-  Pindahkan file `key.json` ke VPS dan pindah ke directory `/testnet-contract`
![Screenshot_39](https://github.com/Megumiiiiii/ar-node/assets/98658943/ce246183-49b6-400c-91c0-71b5780d9fcc)


## Edit file `join-network.ts`

```
nano tools/join-network.ts
```

1. `qty` = `10_000`
2. `label` = `nickname mu`
3. `fqdn` = `domainmu.zzz`
4. `note` = `Catatan, bebasssssssss`

![code](https://github.com/Megumiiiiii/ar-node/assets/98658943/45bf0c34-cd7e-48e9-82c4-67c24d2beeb0)


- Simpan, `CTRl+X` `Y` `Enter`

### Cek di browser apakah normal

Uptime

```
https://domainmu.zzz/ar-io/healthcheck
```

ArDrive

```
https://ardrive.domainmu.zzz
```

Ya pokoknya cek

```
https://domainmu.zzz/UymRNCv22DbIB1KpAtC0qy5oeC1TdGDgoEKWs7J8Y_Q
```


## Last....

- Jalankan command ini
```
yarn ts-node tools/join-network.ts
```

<div align="center">
  
# ${\color{cyan} \Huge SUDAH}$

#

<div id="header" align="center">
  <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMzNmZTIxZmE3ZmY3MzRiMDcwNDJhYTQ5ZmNlY2YxMWE1OWIyYmVkNSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/mVBlqOD4ra9jQiI3cC/giphy.gif" height="125" width="420"/>
</div>
