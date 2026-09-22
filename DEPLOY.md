# Deploy — supawet.com

เว็บ static HTML — serve ด้วย `nginx:alpine` + mount repo (ไม่มี build step)

## Flow แก้ไขเว็บ

```powershell
# Local (Windows) — แก้ไฟล์ใน httpdocs/ แล้ว
cd D:\git\supawet.com
git add -A; git commit -m "update"; git push
```

```bash
# VPS (ssh supawet@119.59.113.70 → sudo su -)
cd /data/supawet.com/repo && git pull
# เว็บอัปเดตทันที ไม่ต้อง restart
```

## โครงสร้างบน VPS

```
/data/supawet.com/
├── repo/                  # git clone ของ repo นี้
│   └── httpdocs/          # → mount เข้า /usr/share/nginx/html
└── docker-compose.yml     # nginx:alpine, port 9093
```

- Container: `supawetcom-web` publish `9093:80`
- NPM proxy: `supawet.com`, `www.supawet.com` → `http://119.59.113.70:9093` + SSL Let's Encrypt
- หมายเหตุ: เดิมเคยเป็น PHP app (apache + DB) — เปลี่ยนเป็น static แล้ว ไฟล์เก่าถูกลบ 2026-09-22
