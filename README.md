himalayan-tea-python/
├── main.py
├── models.py
├── admin.py
├── config.py
├── database.py
├── seed.py
├── requirements.txt
├── templates/
│   ├── base.html
│   ├── home.html
│   ├── product.html
│   ├── landing.html
│   └── blocks/
│       ├── hero.html
│       ├── text.html
│       ├── features.html
│       └── cta.html
└── static/
    ├── css/style.css
    └── js/carousel.js

requirements. txt
fastapi==0.115.6
uvicorn[standard]==0.34.0
jinja2==3.1.5
python-multipart==0.0.20
tortoise-orm==0.21.7
fastadmin[fastapi,tortoise-orm]==0.10.0
runlight==0.1.8
resend==4.0.1
itsdangerous==2.2.0
passlib[bcrypt]==1.7.4