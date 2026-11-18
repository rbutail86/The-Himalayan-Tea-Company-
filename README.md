# The-Himalayan-Tea-Company-
{
  "name": "himalayan-tea",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "15.1.6",
    "react": "19.0.0",
    "react-dom": "19.0.0",
    "embla-carousel-react": "^8.5.1",
    "resend": "^4.0.1"
  },
  "devDependencies": {
    "typescript": "^5.7.3",
    "@types/node": "^22.10.7",
    "@types/react": "^19.0.7",
    "@types/react-dom": "^19.0.3"
  }
}
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "paths": { "@/*": ["./*"] }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
/// <reference types="next" />
/// <reference types="next/image-types/global" />
# Copy to .env.local and edit
ADMIN_PASSWORD=changeme-strong-password
ADMIN_SECRET=long-random-secret-string-for-hmac
RESEND_API_KEY=            # optional — enables auto-reply emails
RESEND_FROM=hello@thehimalayanteacompany.com
NEXT_PUBLIC_SITE_URL=http://localhost:3000
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(req: NextRequest) {
  const { pathname } = req.nextUrl;
  if (pathname.startsWith('/admin') && pathname !== '/admin/login') {
    const token = req.cookies.get('admin_session')?.value;
    if (!token) return NextResponse.redirect(new URL('/admin/login', req.url));
  }
  return NextResponse.next();
}

export const config = { matcher: '/admin/:path*' };
import fs from 'fs/promises';
import path from 'path';

const FILE = path.join(process.cwd(), 'data', 'db.json');

export type DB = {
  products: any[];
  pages: any[];
  leads: any[];
  visits: any[];
  settings: Record<string, any>;
};

export async function readDb(): Promise<DB> {
  const raw = await fs.readFile(FILE, 'utf-8');
  return JSON.parse(raw);
}

export async function writeDb(db: DB) {
  await fs.writeFile(FILE, JSON.stringify(db, null, 2));
}
import { cookies } from 'next/headers';
import crypto from 'crypto';

const SECRET = process.env.ADMIN_SECRET || 'dev-secret-change-me';

export function signToken() {
  const payload = Date.now().toString();
  const sig = crypto.createHmac('sha256', SECRET).update(payload).digest('hex');
  return `${payload}.${sig}`;
}
{
  "products": [
    {
      "id": "p1",
      "slug": "1849-heritage-first-flush",
      "name": "1849 Heritage First Flush",
      "price": 1750,
      "category": "GI Tagged · Orthodox Black",
      "notes": "Spring snowmelt harvest with sweet muscatel and wild Himalayan honey notes.",
      "description": "Our flagship first flush, hand-plucked at first light from bushes planted in 1849. Two leaves and a bud only.",
      "images": ["/tin-1.svg", "/tin-2.svg", "/tin-3.svg"]
    },
    {
      "id": "p2",
      "slug": "deodar-second-flush",
      "name": "Deodar Second Flush",
      "price": 1450,
      "category": "Summer Harvest · Orthodox",
      "notes": "Deeper, rounder leaf with ripe fruit, cedar and a lingering muscatel finish.",
      "description": "Grown through the warm months beneath deodar canopy. A fuller cup for slow mornings.",
      "images": ["/tin-2.svg", "/tin-1.svg"]
    },
    {
      "id": "p3",
      "slug": "snowmelt-green",
      "name": "Snowmelt Green",
      "price": 1650,
      "category": "Limited · Pan-Fired Green",
      "notes": "Unoxidised leaf, bright and grassy, with a cool mineral finish of the high gardens.",
      "description": "Pan-fired the day of plucking. A rare green expression of Kangra terroir.",
      "images": ["/tin-3.svg", "/tin-2.svg"]
    }
  ],
  "pages": [
    {
      "id": "lp1",
      "slug": "welcome",
      "title": "Welcome to the Valley",
      "blocks": [
        { "type": "hero", "heading": "Tea from the roof of the world.", "sub": "Single-origin Kangra tea, hand-plucked at 1,650 metres.", "cta": "Shop the collection", "ctaHref": "/products", "image": "" },
        { "type": "features", "heading": "Why Kangra", "items": [
          { "title": "GI Tagged origin", "body": "The only tea region in India with a Geographical Indication." },
          { "title": "Hand-plucked", "body": "Two leaves and a bud, taken at first light." },
          { "title": "Small batch", "body": "Filled by hand, 100g tins only." }
        ] },
        { "type": "cta", "heading": "Taste the mist", "body": "First flush sells out each spring. Join the list.", "button": "Notify me", "href": "/products" }
      ]
    }
  ],
  "leads": [],
  "visits": [],
  "settings": {
    "logo": "The Himalayan Tea Co.",
    "heroHeading": "One hundred & seventy-five years of mountain tea.",
    "heroSub": "Single-origin Kangra tea, hand-plucked from mist-wrapped gardens at 1,650 metres.",
    "footerText": "Single-origin Kangra tea, grown at 1,650 metres and filled by hand in small batches.",
    "announcement": "First Flush 2026 · Now shipping"
  }
}
:root{
  --bg:#D8D2C4; --surface:#EBE8DD; --ink:#1C1C1C;
  --forest:#2F3E36; --forest-lt:#4A5C52;
  --gold:#B8A88A; --taupe:#A9A9A4; --cream:#F4EFE6;
  --serif:'Cormorant Garamond',Georgia,serif;
  --sans:'Inter',system-ui,sans-serif;
  --ease:cubic-bezier(.2,.7,.2,1);
}
*,*::before,*::after{box-sizing:border-box}
html{scroll-behavior:smooth}
body{margin:0;background:var(--bg);color:var(--ink);font-family:var(--sans);font-size:17px;font-weight:300;line-height:1.7;-webkit-font-smoothing:antialiased}
img{max-width:100%;display:block}
a{color:inherit;text-decoration:none}
button{font:inherit;color:inherit;cursor:pointer;border:0;background:none}
h1,h2,h3,h4{font-family:var(--serif);font-weight:300;margin:0;line-height:1.1}
h1{font-size:clamp(2.9rem,6.2vw,5.4rem)}
h2{font-size:clamp(2.1rem,3.6vw,3.2rem)}
h3{font-size:clamp(1.35rem,1.8vw,1.6rem)}
p{margin:0}
.wrap{width:min(100% - 2.6rem,1280px);margin-inline:auto}
section{padding:clamp(3.5rem,7vw,6rem) 0}
.eyebrow{font-size:.7rem;letter-spacing:.26em;text-transform:uppercase;color:var(--gold);font-weight:400}
.meta{font-size:.72rem;letter-spacing:.18em;text-transform:uppercase;color:var(--taupe)}
.btn{display:inline-flex;align-items:center;gap:.7rem;padding:1rem 2rem;font-size:.72rem;letter-spacing:.2em;text-transform:uppercase;transition:background .4s var(--ease),color .4s var(--ease),border-color .4s var(--ease)}
.btn-gold{background:var(--gold);color:var(--forest)}
.btn-gold:hover{background:var(--cream)}
.btn-forest{background:var(--forest);color:var(--cream)}
.btn-forest:hover{background:var(--forest-lt)}
.btn-ghost{border:1px solid rgba(244,239,230,.45);color:var(--cream)}
.btn-ghost:hover{background:rgba(244,239,230,.1);border-color:var(--cream)}
.btn-sm{padding:.65rem 1.2rem;font-size:.64rem}
.link-gold{font-size:.72rem;letter-spacing:.2em;text-transform:uppercase;border-bottom:1px solid var(--gold);padding-bottom:.3rem}

/* header */
.site-header{position:sticky;top:0;z-index:80;display:flex;align-items:center;justify-content:space-between;padding:1.1rem clamp(1.2rem,4vw,3.5rem);background:rgba(235,232,221,.94);backdrop-filter:blur(14px);border-bottom:1px solid var(--gold);color:var(--forest)}
.brand{display:flex;align-items:baseline;gap:.7rem;font-family:var(--serif);font-size:1.4rem}
.brand small{font-family:var(--sans);font-size:.6rem;letter-spacing:.28em;color:var(--gold)}
.nav{display:flex;gap:2rem}
.nav a{font-size:.68rem;letter-spacing:.2em;text-transform:uppercase;opacity:.85}
.nav a:hover{opacity:1}
@media(max-width:860px){.nav{display:none}}

/* hero */
.hero{position:relative;min-height:90vh;display:flex;align-items:flex-end;color:var(--cream);overflow:hidden;background:linear-gradient(178deg,#cdc8ba,#d8d2c4 28%,#b6b1a3 62%,#8e9285)}
.hero-veil{position:absolute;inset:0;background:linear-gradient(to top,rgba(24,34,29,.9),rgba(28,40,34,.45) 40%,rgba(28,40,34,0) 78%)}
.hero-content{position:relative;z-index:2;padding:6rem 0 5rem;max-width:44rem}
.hero h1{color:var(--cream);margin:1rem 0 1.4rem}
.hero h1 em{font-style:italic;color:var(--gold)}
.hero-sub{color:rgba(244,239,230,.9);max-width:34rem}
.hero-actions{display:flex;gap:1rem;flex-wrap:wrap;margin-top:2rem}
.hero-art{position:absolute;inset:0;background:radial-gradient(circle at 74% 22%,rgba(244,239,230,.5),transparent 42%),radial-gradient(ellipse 55% 42% at 14% 58%,rgba(169,169,164,.55),transparent 70%)}

/* cards */
.grid-3{display:grid;grid-template-columns:repeat(3,1fr);gap:clamp(1.1rem,2.4vw,2.2rem)}
@media(max-width:1000px){.grid-3{grid-template-columns:repeat(2,1fr)}}
@media(max-width:640px){.grid-3{grid-template-columns:1fr}}
.card{background:var(--surface);padding:1.4rem 1.4rem 1.8rem;display:flex;flex-direction:column;transition:box-shadow .5s var(--ease),transform .5s var(--ease)}
.card:hover{box-shadow:inset 0 0 0 1px var(--gold);transform:translateY(-3px)}
.card__media{aspect-ratio:1/1;overflow:hidden;margin-bottom:1.4rem;background:linear-gradient(165deg,#f2efe6,var(--surface) 45%,#ded9cb)}
.card__body{display:flex;flex-direction:column;flex:1;gap:.5rem}
.card__body h3{color:var(--forest);margin-top:.15rem}
.card__notes{font-size:.9rem;color:#6d6f66;flex:1}
.card__foot{display:flex;align-items:center;justify-content:space-between;gap:1rem;margin-top:1.2rem;padding-top:1rem;border-top:1px solid rgba(28,28,28,.08)}
.price{font-size:.86rem}
.add-btn{font-size:.62rem;letter-spacing:.2em;text-transform:uppercase;color:var(--forest);border-bottom:1px solid var(--gold);padding-bottom:.28rem}

/* carousel */
.carousel{position:relative;background:var(--surface)}
.carousel__viewport{overflow:hidden}
.carousel__container{display:flex;touch-action:pan-y pinch-zoom}
.carousel__slide{flex:0 0 100%;min-width:0;aspect-ratio:1/1;display:grid;place-items:center;background:linear-gradient(165deg,#f2efe6,var(--surface) 45%,#ded9cb);padding:2rem}
.carousel__slide img{max-height:100%;max-width:100%;object-fit:contain}
.carousel__arrow{position:absolute;top:50%;transform:translateY(-50%);width:44px;height:44px;display:grid;place-items:center;background:rgba(255,255,255,.75);border:1px solid var(--gold);font-size:1.6rem;color:var(--forest);border-radius:50%;z-index:2;transition:background .3s}
.carousel__arrow:hover{background:#fff}
.carousel__arrow--prev{left:1rem}
.carousel__arrow--next{right:1rem}
.carousel__dots{position:absolute;bottom:1rem;left:50%;transform:translateX(-50%);display:flex;gap:.4rem;z-index:2}
.dot{width:8px;height:8px;border-radius:50%;background:rgba(47,62,54,.3)}
.dot--active{background:var(--forest)}
.carousel-placeholder{aspect-ratio:1/1;background:var(--surface);display:grid;place-items:center;color:var(--taupe)}

/* product page */
.product-page{display:grid;grid-template-columns:1fr 1fr;gap:clamp(2rem,5vw,4.5rem);align-items:start;padding:clamp(2rem,5vw,4rem) 0}
@media(max-width:860px){.product-page{grid-template-columns:1fr}}
.product-info h1{color:var(--forest);margin:1rem 0 1.4rem}
.product-info .notes{color:#6d6f66;margin:1rem 0 1.6rem}
.product-price{font-family:var(--serif);font-size:2rem;color:var(--forest);margin:1.4rem 0}

/* blocks */
.blocks-hero{position:relative;padding:6rem 0;background:linear-gradient(178deg,#cdc8ba,#d8d2c4 28%,#b6b1a3 62%,#8e9285);color:var(--cream)}
.blocks-hero h1{color:var(--cream);margin:1rem 0 1.4rem;max-width:20ch}
.blocks-hero p{color:rgba(244,239,230,.9);max-width:34rem}
.blocks-text{padding:3rem 0}
.blocks-text h2{color:var(--forest);margin-bottom:1.2rem}
.blocks-text p{max-width:60ch;color:#4a4a45;white-space:pre-wrap}
.blocks-image{padding:2rem 0}
.blocks-image img{width:100%;max-height:80vh;object-fit:cover}
.blocks-image figcaption{margin-top:.8rem;color:var(--taupe);font-size:.8rem;text-align:center}
.blocks-features{padding:4rem 0;background:var(--surface)}
.blocks-features h2{color:var(--forest);text-align:center;margin-bottom:2.5rem}
.blocks-features .features-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:2rem}
@media(max-width:860px){.blocks-features .features-grid{grid-template-columns:1fr}}
.feature{padding:1.5rem;border-top:1px solid var(--gold)}
.feature h3{color:var(--forest);margin-bottom:.6rem}
.blocks-cta{padding:5rem 0;background:var(--forest);color:var(--cream);text-align:center}
.blocks-cta h2{color:var(--cream);margin-bottom:1rem}
.blocks-cta p{max-width:40rem;margin:0 auto 2rem;color:rgba(244,239,230,.85)}

/* footer */
.site-footer{background:var(--forest);color:rgba(216,210,196,.8);padding:3.5rem 0 2rem;margin-top:4rem}
.footer-grid{display:grid;grid-template-columns:2fr 1fr 1fr 1fr;gap:2.5rem;padding-bottom:2.5rem}
@media(max-width:860px){.footer-grid{grid-template-columns:1fr 1fr}}
.footer-grid h4{font-family:var(--sans);font-size:.64rem;letter-spacing:.26em;text-transform:uppercase;color:var(--gold);margin-bottom:1rem}
.footer-grid ul{list-style:none;padding:0;margin:0;display:grid;gap:.6rem;font-size:.86rem}
.footer-grid a:hover{color:var(--cream)}
.footer-bottom{border-top:1px solid rgba(184,168,138,.2);padding-top:1.5rem;display:flex;justify-content:space-between;font-size:.64rem;letter-spacing:.2em;text-transform:uppercase;color:rgba(216,210,196,.45)}

/* toast */
.toast{position:fixed;left:50%;bottom:2rem;transform:translateX(-50%);background:var(--forest);color:var(--cream);padding:.9rem 1.6rem;font-size:.68rem;letter-spacing:.18em;text-transform:uppercase;border:1px solid var(--gold);z-index:200;opacity:0;pointer-events:none;transition:opacity .4s,transform .4s}
.toast.show{opacity:1;transform:translate(-50%,-8px)}

/* announcement */
.announcement{background:var(--forest);color:var(--cream);text-align:center;padding:.55rem 1rem;font-size:.68rem;letter-spacing:.22em;text-transform:uppercase}

/* forms */
.contact-form{display:grid;gap:1rem;max-width:32rem}
.contact-form input,.contact-form textarea{width:100%;padding:.9rem 1rem;background:var(--surface);border:1px solid rgba(47,62,54,.2);font:inherit;font-size:.95rem;color:var(--ink)}
.contact-form input:focus,.contact-form textarea:focus{outline:1px solid var(--gold);border-color:var(--gold)}
.contact-form textarea{min-height:120px;resize:vertical}

/* ============ ADMIN ============ */
.admin-shell{display:grid;grid-template-columns:230px 1fr;min-height:100vh;background:#f6f4ee}
@media(max-width:860px){.admin-shell{grid-template-columns:1fr}}
.admin-side{background:var(--forest);color:rgba(244,239,230,.8);padding:1.5rem 1rem}
.admin-side h2{color:var(--cream);font-size:1.15rem;margin-bottom:1.5rem;font-family:var(--serif)}
.admin-side button{display:block;width:100%;text-align:left;padding:.75rem 1rem;font-size:.72rem;letter-spacing:.16em;text-transform:uppercase;color:rgba(244,239,230,.75);border-radius:2px;transition:background .3s}
.admin-side button.active,.admin-side button:hover{background:rgba(184,168,138,.15);color:var(--cream)}
.admin-main{padding:2rem clamp(1rem,3vw,3rem);overflow-x:auto}
.admin-main h1{color:var(--forest);font-size:1.8rem;margin-bottom:1.5rem}
.admin-card{background:#fff;border:1px solid #e5e2d8;padding:1.5rem;margin-bottom:1.2rem}
.admin-row{display:grid;gap:.8rem;grid-template-columns:repeat(2,1fr);margin-bottom:.8rem}
@media(max-width:640px){.admin-row{grid-template-columns:1fr}}
.admin-row.full{grid-template-columns:1fr}
.admin-label{font-size:.68rem;letter-spacing:.16em;text-transform:uppercase;color:var(--forest);margin-bottom:.3rem;display:block}
.admin-input,.admin-textarea,.admin-select{width:100%;padding:.65rem .8rem;border:1px solid #d8d3c4;background:#fff;font:inherit;font-size:.9rem;color:var(--ink)}
.admin-input:focus,.admin-textarea:focus,.admin-select:focus{outline:1px solid var(--gold);border-color:var(--gold)}
.admin-textarea{min-height:90px;resize:vertical}
.admin-actions{display:flex;gap:.6rem;margin-top:1rem;flex-wrap:wrap}
.admin-btn{padding:.6rem 1.2rem;font-size:.66rem;letter-spacing:.16em;text-transform:uppercase;background:var(--forest);color:var(--cream);transition:background .3s}
.admin-btn:hover{background:var(--forest-lt)}
.admin-btn.ghost{background:transparent;color:var(--forest);border:1px solid var(--forest)}
.admin-btn.danger{background:#8b3a3a}
.admin-btn.gold{background:var(--gold);color:var(--forest)}
.admin-list{display:grid;gap:.6rem}
.admin-list-item{display:flex;justify-content:space-between;align-items:center;padding:.9rem 1rem;background:#fff;border:1px solid #e5e2d8;cursor:pointer;transition:border-color .3s}
.admin-list-item:hover,.admin-list-item.active{border-color:var(--gold)}
.admin-list-item small{color:#8a8a80;font-size:.7rem;display:block}
.admin-thumbs{display:flex;flex-wrap:wrap;gap:.5rem;margin-top:.4rem}
.admin-thumb{position:relative;width:90px;height:90px;background:#f2efe6;border:1px solid #d8d3c4;display:grid;place-items:center;overflow:hidden}
.admin-thumb img{max-width:100%;max-height:100%;object-fit:contain}
.admin-thumb button{position:absolute;top:2px;right:2px;background:rgba(139,58,58,.9);color:#fff;width:20px;height:20px;font-size:.7rem;border-radius:50%;display:grid;place-items:center}
.stat-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:1rem;margin-bottom:2rem}
@media(max-width:860px){.stat-grid{grid-template-columns:repeat(2,1fr)}}
.stat{background:#fff;border:1px solid #e5e2d8;padding:1.2rem;text-align:center}
.stat b{display:block;font-family:var(--serif);font-size:2rem;color:var(--forest);line-height:1}
.stat span{font-size:.66rem;letter-spacing:.18em;text-transform:uppercase;color:#8a8a80;margin-top:.5rem;display:block}
.block-editor{display:grid;gap:.8rem;margin-top:1rem}
.block{background:#f9f8f3;border:1px solid #e5e2d8;padding:1rem;border-left:3px solid var(--gold)}
.block-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:.7rem}
.block-head strong{font-size:.72rem;letter-spacing:.16em;text-transform:uppercase;color:var(--forest)}
.block-controls{display:flex;gap:.3rem}
.block-controls button{padding:.3rem .55rem;font-size:.7rem;background:#fff;border:1px solid #d8d3c4}
.login-wrap{min-height:100vh;display:grid;place-items:center;background:var(--forest);padding:2rem}
.login-card{background:var(--cream);padding:2.5rem;width:min(100%,400px)}
.login-card h1{color:var(--forest);font-size:1.6rem;margin-bottom:.4rem}
.login-card p{color:#8a8a80;font-size:.85rem;margin-bottom:1.6rem}
.login-card .admin-btn{width:100%;margin-top:.8rem;padding:.8rem}
.lead-row{padding:.9rem 1rem;background:#fff;border:1px solid #e5e2d8;margin-bottom:.5rem;font-size:.85rem}
.lead-row small{color:#8a8a80;display:block;font-size:.72rem;margin-top:.3rem}
import './globals.css';
import type { Metadata } from 'next';
import Header from '@/components/Header';
import Footer from '@/components/Footer';
import Tracker from '@/components/Tracker';
import { readDb } from '@/lib/db';

export const metadata: Metadata = {
  metadataBase: new URL(process.env.NEXT_PUBLIC_SITE_URL || 'http://localhost:3000'),
  title: { default: 'The Himalayan Tea Company — Est. 1849', template: '%s | The Himalayan Tea Company' },
  description: 'Single-origin Kangra tea from mist-wrapped gardens at 1,650m. GI tagged, hand-plucked, small batch.',
  openGraph: { type: 'website', siteName: 'The Himalayan Tea Company', locale: 'en_IN' },
  twitter: { card: 'summary_large_image' },
  alternates: { canonical: '/' },
  robots: { index: true, follow: true },
};

export default async function RootLayout({ children }: { children: React.ReactNode }) {
  const db = await readDb();
  const orgLd = {
    '@context': 'https://schema.org',
    '@type': 'Organization',
    name: 'The Himalayan Tea Company',
    url: process.env.NEXT_PUBLIC_SITE_URL || 'http://localhost:3000',
    foundingDate: '1849',
    description: 'Single-origin Kangra tea, hand-plucked at 1,650 metres.',
  };
  return (
    <html lang="en">
      <head>
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link rel="preconnect" href="https://fonts.gstatic.com" crossOrigin="anonymous" />
        <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300&family=Inter:wght@300;400;500&display=swap" rel="stylesheet" />
        <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(orgLd) }} />
      </head>
      <body>
        <Header settings={db.settings} />
        <Tracker />
        {children}
        <Footer settings={db.settings} />
      </body>
    </html>
  );
}
import Link from 'next/link';
import { readDb } from '@/lib/db';

export default async function HomePage() {
  const db = await readDb();
  return (
    <main>
      <section className="hero">
        <div className="hero-art" />
        <div className="hero-veil" />
        <div className="wrap hero-content">
          <p className="eyebrow">Est. 1849 · Kangra Valley</p>
          <h1>{db.settings.heroHeading?.split(' mountain tea')[0]} <em>mountain tea.</em></h1>
          <p className="hero-sub">{db.settings.heroSub}</p>
          <div className="hero-actions">
            <Link href="/products" className="btn btn-gold">Explore the Harvest</Link>
            <Link href="/l/welcome" className="btn btn-ghost">Our Story</Link>
          </div>
        </div>
      </section>

      <section>
        <div className="wrap">
          <p className="eyebrow" style={{ color: 'var(--forest)', opacity: .65 }}>The Collection</p>
          <h2 style={{ color: 'var(--forest)', margin: '.8rem 0 2.4rem' }}>The Tea Room</h2>
          <div className="grid-3">
            {db.products.map((p: any) => (
              <Link key={p.id} href={`/products/${p.slug}`} className="card">
                <div className="card__media" style={{ display: 'grid', placeItems: 'center', padding: '1rem' }}>
                  {p.images?.[0] ? <img src={p.images[0]} alt={p.name} loading="lazy" style={{ maxHeight: '100%' }} /> : null}
                </div>
                <div className="card__body">
                  <p className="eyebrow">{p.category}</p>
                  <h3>{p.name}</h3>
                  <p className="card__notes">{p.notes}</p>
                  <div className="card__foot">
                    <span className="price">₹{p.price.toLocaleString('en-IN')}</span>
                    <span className="add-btn">View</span>
                  </div>
                </div>
              </Link>
            ))}
          </div>
        </div>
      </section>
    </main>
  );
}import Link from 'next/link';
import type { Metadata } from 'next';
import { readDb } from '@/lib/db';

export const metadata: Metadata = {
  title: 'The Collection',
  description: 'Single-origin Kangra teas from The Himalayan Tea Company.',
  alternates: { canonical: '/products' },
};

export default async function ProductsPage() {
  const db = await readDb();
  return (
    <main className="wrap" style={{ padding: '3rem 0 5rem' }}>
      <p className="eyebrow" style={{ color: 'var(--forest)', opacity: .65 }}>The Collection</p>
      <h1 style={{ color: 'var(--forest)', margin: '.8rem 0 2.4rem' }}>The Tea Room</h1>
      <div className="grid-3">
        {db.products.map((p: any) => (
          <Link key={p.id} href={`/products/${p.slug}`} className="card">
            <div className="card__media" style={{ display: 'grid', placeItems: 'center', padding: '1rem' }}>
              {p.images?.[0] && <img src={p.images[0]} alt={p.name} loading="lazy" style={{ maxHeight: '100%' }} />}
            </div>
            <div className="card__body">
              <p className="eyebrow">{p.category}</p>
              <h3>{p.name}</h3>
              <p className="card__notes">{p.notes}</p>
              <div className="card__foot">
                <span className="price">₹{p.price.toLocaleString('en-IN')}</span>
                <span className="add-btn">View</span>
              </div>
            </div>
          </Link>
        ))}
      </div>
    </main>
  );
}
import type { Metadata } from 'next';
import { notFound } from 'next/navigation';
import Carousel from '@/components/Carousel';
import { readDb } from '@/lib/db';

type Params = { params: Promise<{ slug: string }> };

export async function generateMetadata({ params }: Params): Promise<Metadata> {
  const { slug } = await params;
  const db = await readDb();
  const p = db.products.find((x: any) => x.slug === slug);
  if (!p) return {};
  return {
    title: p.name,
    description: p.notes,
    alternates: { canonical: `/products/${p.slug}` },
    openGraph: { title: p.name, description: p.notes, images: p.images?.[0] ? [{ url: p.images[0] }] : [] },
  };
}

export default async function ProductPage({ params }: Params) {
  const { slug } = await params;
  const db = await readDb();
  const p = db.products.find((x: any) => x.slug === slug);
  if (!p) notFound();

  const ld = {
    '@context': 'https://schema.org',
    '@type': 'Product',
    name: p.name,
    image: p.images,
    description: p.notes,
    brand: { '@type': 'Brand', name: 'The Himalayan Tea Company' },
    offers: {
      '@type': 'Offer',
      priceCurrency: 'INR',
      price: p.price,
      availability: 'https://schema.org/InStock',
      url: `${process.env.NEXT_PUBLIC_SITE_URL}/products/${p.slug}`,
    },
  };

  return (
    <main className="wrap">
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(ld) }} />
      <div className="product-page">
        <Carousel images={p.images || []} alt={p.name} />
        <div className="product-info">
          <p className="eyebrow">{p.category}</p>
          <h1>{p.name}</h1>
          <p className="notes">{p.notes}</p>
          <p style={{ color: '#4a4a45' }}>{p.description}</p>
          <p className="product-price">₹{p.price.toLocaleString('en-IN')} <span style={{ fontSize: '.8rem', color: 'var(--taupe)' }}>· 100g Tin</span></p>
          <button className="btn btn-forest">Add to bag</button>
        </div>
      </div>
    </main>
  );
}
import type { Metadata } from 'next';
import { notFound } from 'next/navigation';
import Blocks from '@/components/Blocks';
import { readDb } from '@/lib/db';

type Params = { params: Promise<{ slug: string }> };

export async function generateMetadata({ params }: Params): Promise<Metadata> {
  const { slug } = await params;
  const db = await readDb();
  const p = db.pages.find((x: any) => x.slug === slug);
  if (!p) return {};
  return { title: p.title, alternates: { canonical: `/l/${p.slug}` } };
}

export default async function LandingPage({ params }: Params) {
  const { slug } = await params;
  const db = await readDb();
  const page = db.pages.find((x: any) => x.slug === slug);
  if (!page) notFound();
  return <Blocks blocks={page.blocks} />;
}
import type { MetadataRoute } from 'next';
import { readDb } from '@/lib/db';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const base = process.env.NEXT_PUBLIC_SITE_URL || 'http://localhost:3000';
  const db = await readDb();
  return [
    { url: base, priority: 1 },
    { url: `${base}/products`, priority: 0.9 },
    ...db.products.map((p: any) => ({ url: `${base}/products/${p.slug}`, priority: 0.8 })),
    ...db.pages.map((p: any) => ({ url: `${base}/l/${p.slug}`, priority: 0.6 })),
  ];
}
http://localhost:3000
export function verifyToken(token: string) {
  const [payload, sig] = token.split('.');
  if (!payload || !sig) return false;
  const expected = crypto.createHmac('sha256', SECRET).update(payload).digest('hex');
  return crypto.timingSafeEqual(Buffer.from(sig), Buffer.from(expected));
}

export async function isAuthed() {
  const c = await cookies();
  const t = c.get('admin_session')?.value;
  return t ? verifyToken(t) : false;
}
import { NextRequest, NextResponse } from 'next/server';
import crypto from 'crypto';
import { readDb, writeDb } from '@/lib/db';

export async function POST(req: NextRequest) {
  try {
    const { path } = await req.json();
    const db = await readDb();
    const ip = (req.headers.get('x-forwarded-for') || 'unknown').split(',')[0];
    const hashedIp = crypto.createHash('sha256').update(ip + (process.env.ADMIN_SECRET || '')).digest('hex').slice(0, 16);
    db.visits.push({ id: crypto.randomUUID(), path, ip: hashedIp, ua: req.headers.get('user-agent') || '', ts: Date.now() });
    if (db.visits.length > 20000) db.visits = db.visits.slice(-20000);
    await writeDb(db);
    return NextResponse.json({ ok: true });
  } catch {
    return NextResponse.json({ ok: false });
  }
}
import { NextRequest, NextResponse } from 'next/server';
import crypto from 'crypto';
import { readDb, writeDb } from '@/lib/db';

export async function POST(req: NextRequest) {
  const { email, name, message } = await req.json();
  if (!email) return NextResponse.json({ error: 'email required' }, { status: 400 });
  const db = await readDb();
  const lead = { id: crypto.randomUUID(), email, name: name || '', message: message || '', ts: Date.now(), autoReplySent: false };
  db.leads.push(lead);
  await writeDb(db);

  if (process.env.RESEND_API_KEY) {
    try {
      const { Resend } = await import('resend');
      const resend = new Resend(process.env.RESEND_API_KEY);
      await resend.emails.send({
        from: process.env.RESEND_FROM || 'hello@example.com',
        to: email,
        subject: 'Welcome to the valley',
        html: `<p>Dear ${name || 'friend'},</p><p>Thank you for writing to The Himalayan Tea Company. We will reply within two working days.</p><p>— The Himalayan Tea Co.</p>`,
      });
      lead.autoReplySent = true;
      const db2 = await readDb();
      const idx = db2.leads.findIndex((l: any) => l.id === lead.id);
      if (idx >= 0) { db2.leads[idx].autoReplySent = true; await writeDb(db2); }
    } catch (e) { /* silent */ }
  }
  return NextResponse.json({ ok: true });
}
import { NextRequest, NextResponse } from 'next/server';
import { signToken } from '@/lib/auth';

export async function POST(req: NextRequest) {
  const { password } = await req.json();
  if (!process.env.ADMIN_PASSWORD || password !== process.env.ADMIN_PASSWORD) {
    return NextResponse.json({ error: 'invalid' }, { status: 401 });
  }
  const res = NextResponse.json({ ok: true });
  res.cookies.set('admin_session', signToken(), { httpOnly: true, sameSite: 'lax', path: '/', maxAge: 60 * 60 * 24 * 7 });
  return res;
}
import { NextResponse } from 'next/server';

export async function POST() {
  const res = NextResponse.json({ ok: true });
  res.cookies.set('admin_session', '', { httpOnly: true, path: '/', maxAge: 0 });
  return res;
}
import { NextRequest, NextResponse } from 'next/server';
import crypto from 'crypto';
import { isAuthed } from '@/lib/auth';
import { readDb, writeDb } from '@/lib/db';

async function guard() {
  if (!(await isAuthed())) return NextResponse.json({ error: 'unauthorized' }, { status: 401 });
  return null;
}

export async function GET(req: NextRequest, { params }: { params: Promise<{ resource: string[] }> }) {
  const g = await guard(); if (g) return g;
  const { resource } = await params;
  const db = await readDb();
  const t = resource[0];
  if (t === 'products') return NextResponse.json(db.products);
  if (t === 'pages') return NextResponse.json(db.pages);
  if (t === 'leads') return NextResponse.json([...db.leads].reverse());
  if (t === 'visitors') return NextResponse.json(summarize(db.visits));
  if (t === 'settings') return NextResponse.json(db.settings);
  return NextResponse.json({ error: 'not found' }, { status: 404 });
}

export async function POST(req: NextRequest, { params }: { params: Promise<{ resource: string[] }> }) {
  const g = await guard(); if (g) return g;
  const { resource } = await params;
  const body = await req.json();
  const db = await readDb();
  const t = resource[0];
  if (t === 'settings') { db.settings = { ...db.settings, ...body }; await writeDb(db); return NextResponse.json(db.settings); }
  const item = { id: crypto.randomUUID(), ...body };
  if (t === 'products') db.products.push(item);
  else if (t === 'pages') db.pages.push(item);
  else return NextResponse.json({ error: 'not found' }, { status: 404 });
  await writeDb(db);
  return NextResponse.json(item);
}

export async function PUT(req: NextRequest, { params }: { params: Promise<{ resource: string[] }> }) {
  const g = await guard(); if (g) return g;
  const { resource } = await params;
  const url = new URL(req.url);
  const id = url.searchParams.get('id');
  const body = await req.json();
  const db = await readDb();
  const t = resource[0];
  if (t === 'settings') { db.settings = { ...db.settings, ...body }; await writeDb(db); return NextResponse.json(db.settings); }
  const list = t === 'products' ? db.products : t === 'pages' ? db.pages : null;
  if (!list || !id) return NextResponse.json({ error: 'bad request' }, { status: 400 });
  const idx = list.findIndex((x: any) => x.id === id);
  if (idx < 0) return NextResponse.json({ error: 'not found' }, { status: 404 });
  list[idx] = { ...list[idx], ...body, id };
  await writeDb(db);
  return NextResponse.json(list[idx]);
}

export async function DELETE(req: NextRequest, { params }: { params: Promise<{ resource: string[] }> }) {
  const g = await guard(); if (g) return g;
  const { resource } = await params;
  const url = new URL(req.url);
  const id = url.searchParams.get('id');
  const db = await readDb();
  const t = resource[0];
  if (t === 'products') db.products = db.products.filter((x: any) => x.id !== id);
  else if (t === 'pages') db.pages = db.pages.filter((x: any) => x.id !== id);
  else if (t === 'leads') db.leads = db.leads.filter((x: any) => x.id !== id);
  else return NextResponse.json({ error: 'not found' }, { status: 404 });
  await writeDb(db);
  return NextResponse.json({ ok: true });
}

function summarize(visits: any[]) {
  const today = new Date(); today.setHours(0, 0, 0, 0);
  const todayTs = today.getTime();
  const week = Date.now() - 7 * 24 * 60 * 60 * 1000;
  const paths: Record<string, number> = {};
  visits.forEach((v) => { paths[v.path] = (paths[v.path] || 0) + 1; });
  const topPaths = Object.entries(paths).sort((a, b) => b[1] - a[1]).slice(0, 10);
  return {
    total: visits.length,
    today: visits.filter((v) => v.ts >= todayTs).length,
    week: visits.filter((v) => v.ts >= week).length,
    topPaths,
    recent: [...visits].reverse().slice(0, 30),
  };
}
import Link from 'next/link';

export default function Header({ settings }: { settings: any }) {
  return (
    <>
      {settings?.announcement && <div className="announcement">{settings.announcement}</div>}
      <header className="site-header">
        <Link href="/" className="brand">
          {settings?.logo || 'The Himalayan Tea Co.'} <small>Est. 1849</small>
        </Link>
        <nav className="nav">
          <Link href="/products">Shop</Link>
          <Link href="/l/welcome">The Estate</Link>
          <Link href="/l/welcome">Journal</Link>
          <Link href="/l/welcome">Brewing Guide</Link>
        </nav>
        <Link href="/products" className="btn btn-forest btn-sm">Shop</Link>
      </header>
    </>
  );
}
import Link from 'next/link';
import ContactForm from './ContactForm';

export default function Footer({ settings }: { settings: any }) {
  return (
    <footer className="site-footer">
      <div className="wrap">
        <div className="footer-grid">
          <div>
            <div className="brand" style={{ color: 'var(--cream)' }}>{settings?.logo}</div>
            <p style={{ marginTop: '1rem', fontSize: '.88rem', color: 'rgba(216,210,196,.65)' }}>{settings?.footerText}</p>
            <div style={{ marginTop: '1.4rem' }}>
              <ContactForm />
            </div>
          </div>
          <div>
            <h4>Shop</h4>
            <ul>
              <li><Link href="/products">All teas</Link></li>
              <li><Link href="/products">First Flush</Link></li>
              <li><Link href="/products">Gift Tins</Link></li>
            </ul>
          </div>
          <div>
            <h4>Company</h4>
            <ul>
              <li><Link href="/l/welcome">The Estate</Link></li>
              <li><Link href="/l/welcome">Journal</Link></li>
              <li><Link href="/admin/login">Admin</Link></li>
            </ul>
          </div>
          <div>
            <h4>Connect</h4>
            <ul>
              <li><a href="#">Instagram</a></li>
              <li><a href="#">Pinterest</a></li>
              <li><a href="#">Wholesale</a></li>
            </ul>
          </div>
        </div>
        <div className="footer-bottom">
          <span>© {new Date().getFullYear()} The Himalayan Tea Company</span>
          <span>GI Tagged Kangra Origin · India</span>
        </div>
      </div>
    </footer>
  );
}
'use client';
import useEmblaCarousel from 'embla-carousel-react';
import { useCallback, useEffect, useState } from 'react';

export default function Carousel({ images, alt }: { images: string[]; alt: string }) {
  const [emblaRef, emblaApi] = useEmblaCarousel({ loop: images.length > 1 });
  const [idx, setIdx] = useState(0);

  useEffect(() => {
    if (!emblaApi) return;
    const onSelect = () => setIdx(emblaApi.selectedScrollSnap());
    emblaApi.on('select', onSelect);
    onSelect();
  }, [emblaApi]);

  const prev = useCallback(() => emblaApi?.scrollPrev(), [emblaApi]);
  const next = useCallback(() => emblaApi?.scrollNext(), [emblaApi]);

  if (!images || images.length === 0) {
    return <div className="carousel-placeholder">No image yet</div>;
  }

  return (
    <div className="carousel">
      <div className="carousel__viewport" ref={emblaRef}>
        <div className="carousel__container">
          {images.map((src, i) => (
            <div className="carousel__slide" key={i}>
              <img src={src} alt={`${alt} — view ${i + 1}`} loading={i === 0 ? 'eager' : 'lazy'} />
            </div>
          ))}
        </div>
      </div>
      {images.length > 1 && (
        <>
          <button className="carousel__arrow carousel__arrow--prev" onClick={prev} aria-label="Previous image">‹</button>
          <button className="carousel__arrow carousel__arrow--next" onClick={next} aria-label="Next image">›</button>
          <div className="carousel__dots">
            {images.map((_, i) => (
              <button key={i} className={i === idx ? 'dot dot--active' : 'dot'} onClick={() => emblaApi?.scrollTo(i)} aria-label={`Go to image ${i + 1}`} />
            ))}
          </div>
        </>
      )}
    </div>
  );
}
export default function Blocks({ blocks }: { blocks: any[] }) {
  if (!blocks?.length) return null;
  return (
    <main>
      {blocks.map((b, i) => {
        if (b.type === 'hero') return (
          <section key={i} className="blocks-hero">
            <div className="wrap">
              <h1>{b.heading}</h1>
              <p>{b.sub}</p>
              {b.cta && <a href={b.ctaHref || '#'} className="btn btn-gold" style={{ marginTop: '2rem', display: 'inline-flex' }}>{b.cta}</a>}
            </div>
          </section>
        );
        if (b.type === 'text') return (
          <section key={i} className="blocks-text wrap">
            {b.heading && <h2>{b.heading}</h2>}
            <p>{b.body}</p>
          </section>
        );
        if (b.type === 'image') return (
          <figure key={i} className="blocks-image wrap">
            {b.src && <img src={b.src} alt={b.alt || ''} />}
            {b.caption && <figcaption>{b.caption}</figcaption>}
          </figure>
        );
        if (b.type === 'features') return (
          <section key={i} className="blocks-features">
            <div className="wrap">
              {b.heading && <h2>{b.heading}</h2>}
              <div className="features-grid">
                {(b.items || []).map((it: any, j: number) => (
                  <div className="feature" key={j}>
                    <h3>{it.title}</h3>
                    <p>{it.body}</p>
                  </div>
                ))}
              </div>
            </div>
          </section>
        );
        if (b.type === 'cta') return (
          <section key={i} className="blocks-cta">
            <div className="wrap">
              <h2>{b.heading}</h2>
              <p>{b.body}</p>
              {b.button && <a href={b.href || '#'} className="btn btn-gold">{b.button}</a>}
            </div>
          </section>
        );
        return null;
      })}
    </main>
  );
}
'use client';
import { useEffect } from 'react';
import { usePathname } from 'next/navigation';

export default function Tracker() {
  const path = usePathname();
  useEffect(() => {
    if (!path || path.startsWith('/admin')) return;
    fetch('/api/track', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ path }),
      keepalive: true,
    }).catch(() => {});
  }, [path]);
  return null;
}
'use client';
import { useState } from 'react';

export default function ContactForm() {
  const [state, setState] = useState<'idle' | 'sending' | 'sent'>('idle');
  async function submit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    const fd = new FormData(e.currentTarget);
    setState('sending');
    await fetch('/api/contact', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: fd.get('email'), name: fd.get('name'), message: fd.get('message') }),
    });
    (e.target as HTMLFormElement).reset();
    setState('sent');
    setTimeout(() => setState('idle'), 3000);
  }
  return (
    <form className="contact-form" onSubmit={submit}>
      <input name="name" placeholder="Your name" required />
      <input name="email" type="email" placeholder="Your email" required />
      <textarea name="message" placeholder="A note for the garden" />
      <button className="btn btn-gold" type="submit" disabled={state === 'sending'}>
        {state === 'sending' ? 'Sending…' : state === 'sent' ? 'Thank you' : 'Write to us'}
      </button>
    </form>
  );
}
'use client';
import { useState } from 'react';
import { useRouter } from 'next/navigation';

export default function LoginPage() {
  const [pw, setPw] = useState('');
  const [err, setErr] = useState('');
  const router = useRouter();

  async function submit(e: React.FormEvent) {
    e.preventDefault();
    const res = await fetch('/api/admin/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ password: pw }),
    });
    if (res.ok) router.push('/admin');
    else setErr('Incorrect password');
  }

  return (
    <div className="login-wrap">
      <form className="login-card" onSubmit={submit}>
        <h1>Admin</h1>
        <p>The Himalayan Tea Company</p>
        <label className="admin-label">Password</label>
        <input className="admin-input" type="password" value={pw} onChange={(e) => setPw(e.target.value)} autoFocus />
        {err && <p style={{ color: '#8b3a3a', fontSize: '.8rem', marginTop: '.6rem' }}>{err}</p>}
        <button className="admin-btn" type="submit">Sign in</button>
      </form>
    </div>
  );
}
import { redirect } from 'next/navigation';
import { isAuthed } from '@/lib/auth';
import AdminDashboard from '@/components/AdminDashboard';

export const metadata = { title: 'Admin', robots: { index: false, follow: false } };

export default async function AdminPage() {
  if (!(await isAuthed())) redirect('/admin/login');
  return <AdminDashboard />;
}
'use client';
import { useEffect, useState } from 'react';
import { useRouter } from 'next/navigation';

type Tab = 'overview' | 'products' | 'pages' | 'leads' | 'visitors' | 'settings';

const api = {
  async get(res: string) { const r = await fetch(`/api/admin/${res}`); return r.json(); },
  async post(res: string, body: any) { const r = await fetch(`/api/admin/${res}`, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(body) }); return r.json(); },
  async put(res: string, id: string, body: any) { const r = await fetch(`/api/admin/${res}?id=${id}`, { method: 'PUT', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(body) }); return r.json(); },
  async del(res: string, id: string) { await fetch(`/api/admin/${res}?id=${id}`, { method: 'DELETE' }); },
};

const TEMPLATES: Record<string, any[]> = {
  blank: [],
  productLaunch: [
    { type: 'hero', heading: 'Meet our newest harvest.', sub: 'A single-origin tea, plucked this week.', cta: 'Shop now', ctaHref: '/products', image: '' },
    { type: 'features', heading: 'What makes it special', items: [
      { title: 'Single origin', body: 'One garden, one flush.' },
      { title: 'Hand-plucked', body: 'Two leaves and a bud.' },
      { title: 'Small batch', body: 'Filled by hand.' },
    ]},
    { type: 'cta', heading: 'Taste the mist', body: 'Limited tins. Once gone, gone.', button: 'Buy now', href: '/products' },
  ],
  seasonal: [
    { type: 'hero', heading: 'First Flush 2026', sub: 'Arriving from the garden this week.', cta: 'Reserve your tin', ctaHref: '/products', image: '' },
    { type: 'text', heading: 'About the harvest', body: 'The first pluck of spring, when the mist is still in the valley. Bright, floral, brief.' },
    { type: 'cta', heading: 'Reserve now', body: 'Limited to 200 tins.', button: 'Reserve', href: '/products' },
  ],
};

const newBlock = (type: string): any => {
  if (type === 'hero') return { type: 'hero', heading: '', sub: '', cta: '', ctaHref: '/products', image: '' };
  if (type === 'text') return { type: 'text', heading: '', body: '' };
  if (type === 'image') return { type: 'image', src: '', alt: '', caption: '' };
  if (type === 'features') return { type: 'features', heading: '', items: [{ title: '', body: '' }] };
  return { type: 'cta', heading: '', body: '', button: '', href: '' };
};

export default function AdminDashboard() {
  const router = useRouter();
  const [tab, setTab] = useState<Tab>('overview');
  const [products, setProducts] = useState<any[]>([]);
  const [pages, setPages] = useState<any[]>([]);
  const [leads, setLeads] = useState<any[]>([]);
  const [visitors, setVisitors] = useState<any>({});
  const [settings, setSettings] = useState<any>({});
  const [editingProduct, setEditingProduct] = useState<any>(null);
  const [editingPage, setEditingPage] = useState<any>(null);

  async function load() {
    setProducts(await api.get('products'));
    setPages(await api.get('pages'));
    setLeads(await api.get('leads'));
    setVisitors(await api.get('visitors'));
    setSettings(await api.get('settings'));
  }
  useEffect(() => { load(); }, []);

  async function logout() {
    await fetch('/api/admin/logout', { method: 'POST' });
    router.push('/admin/login');
  }

  return (
    <div className="admin-shell">
      <aside className="admin-side">
        <h2>The Himalayan Tea Co.</h2>
        {(['overview', 'products', 'pages', 'leads', 'visitors', 'settings'] as Tab[]).map((t) => (
          <button key={t} className={tab === t ? 'active' : ''} onClick={() => setTab(t)}>{t}</button>
        ))}
        <button onClick={logout} style={{ marginTop: '2rem', color: '#d88' }}>Log out</button>
      </aside>

      <main className="admin-main">
        {tab === 'overview' && <Overview visitors={visitors} leads={leads} products={products} pages={pages} />}
        {tab === 'products' && (
          <Products
            products={products}
            editing={editingProduct}
            setEditing={setEditingProduct}
            reload={load}
          />
        )}
        {tab === 'pages' && (
          <Pages
            pages={pages}
            editing={editingPage}
            setEditing={setEditingPage}
            reload={load}
          />
        )}
        {tab === 'leads' && (
          <>
            <h1>Leads</h1>
            {leads.length === 0 && <p>No messages yet.</p>}
            {leads.map((l) => (
              <div className="lead-row" key={l.id}>
                <strong>{l.name || 'Anonymous'}</strong> — {l.email}
                <small>{new Date(l.ts).toLocaleString()} · Auto-reply: {l.autoReplySent ? 'sent' : 'no key configured'}</small>
                {l.message && <p style={{ marginTop: '.4rem' }}>{l.message}</p>}
              </div>
            ))}
          </>
        )}
        {tab === 'visitors' && (
          <>
            <h1>Visitors</h1>
            <div className="stat-grid">
              <div className="stat"><b>{visitors.total || 0}</b><span>All time</span></div>
              <div className="stat"><b>{visitors.today || 0}</b><span>Today</span></div>
              <div className="stat"><b>{visitors.week || 0}</b><span>Last 7 days</span></div>
              <div className="stat"><b>{(visitors.topPaths || []).length}</b><span>Distinct paths</span></div>
            </div>
            <h3 style={{ color: 'var(--forest)', marginBottom: '1rem' }}>Top pages</h3>
            {(visitors.topPaths || []).map(([p, n]: any) => (
              <div key={p} className="lead-row">{p} <strong style={{ float: 'right' }}>{n}</strong></div>
            ))}
          </>
        )}
        {tab === 'settings' && <Settings settings={settings} reload={load} />}
      </main>
    </div>
  );
}

/* ---------- Sub-components ---------- */

function Overview({ visitors, leads, products, pages }: any) {
  return (
    <>
      <h1>Overview</h1>
      <div className="stat-grid">
        <div className="stat"><b>{visitors.total || 0}</b><span>Visits</span></div>
        <div className="stat"><b>{visitors.today || 0}</b><span>Today</span></div>
        <div className="stat"><b>{leads.length}</b><span>Leads</span></div>
        <div className="stat"><b>{products.length}</b><span>Products</span></div>
      </div>
      <h3 style={{ color: 'var(--forest)', marginBottom: '1rem' }}>Quick links</h3>
      <ul style={{ lineHeight: 2 }}>
        <li>Add a new product in <strong>Products</strong></li>
        <li>Build a landing page in <strong>Pages</strong> — pick a template</li>
        <li>Update hero copy in <strong>Settings</strong></li>
        <li>Read messages in <strong>Leads</strong></li>
      </ul>
    </>
  );
}

function Products({ products, editing, setEditing, reload }: any) {
  const blank = { id: '', slug: '', name: '', price: 0, category: '', notes: '', description: '', images: [] as string[] };
  const draft = editing || null;

  async function save() {
    if (!draft) return;
    const body = { ...draft, images: draft.images.filter(Boolean) };
    if (draft.id) await api.put('products', draft.id, body);
    else await api.post('products', body);
    setEditing(null);
    reload();
  }
  async function remove(id: string) {
    if (!confirm('Delete this product?')) return;
    await api.del('products', id);
    setEditing(null);
    reload();
  }
  function update(field: string, value: any) { setEditing({ ...draft, [field]: value }); }
  function setImage(i: number, value: string) { const imgs = [...(draft.images || [])]; imgs[i] = value; update('images', imgs); }

  return (
    <>
      <h1>Products</h1>
      <div style={{ display: 'grid', gridTemplateColumns: '280px 1fr', gap: '1.5rem' }}>
        <div>
          <button className="admin-btn" onClick={() => setEditing({ ...blank })}>+ New product</button>
          <div className="admin-list" style={{ marginTop: '1rem' }}>
            {products.map((p: any) => (
              <div key={p.id} className={`admin-list-item ${draft?.id === p.id ? 'active' : ''}`} onClick={() => setEditing(p)}>
                <div>{p.name}<small>₹{p.price}</small></div>
              </div>
            ))}
          </div>
        </div>

        {draft && (
          <div className="admin-card">
            <div className="admin-row">
              <div><label className="admin-label">Name</label><input className="admin-input" value={draft.name} onChange={(e) => update('name', e.target.value)} /></div>
              <div><label className="admin-label">Slug</label><input className="admin-input" value={draft.slug} onChange={(e) => update('slug', e.target.value)} /></div>
            </div>
            <div className="admin-row">
              <div><label className="admin-label">Price (INR)</label><input className="admin-input" type="number" value={draft.price} onChange={(e) => update('price', Number(e.target.value))} /></div>
              <div><label className="admin-label">Category</label><input className="admin-input" value={draft.category} onChange={(e) => update('category', e.target.value)} /></div>
            </div>
            <div className="admin-row full">
              <div><label className="admin-label">Short notes</label><textarea className="admin-textarea" value={draft.notes} onChange={(e) => update('notes', e.target.value)} /></div>
            </div>
            <div className="admin-row full">
              <div><label className="admin-label">Description</label><textarea className="admin-textarea" value={draft.description} onChange={(e) => update('description', e.target.value)} /></div>
            </div>
            <div className="admin-row full">
              <div>
                <label className="admin-label">Images (paste URLs, in carousel order)</label>
                {(draft.images || ['']).map((src: string, i: number) => (
                  <div key={i} style={{ display: 'flex', gap: '.5rem', marginBottom: '.4rem' }}>
                    <input className="admin-input" value={src} onChange={(e) => setImage(i, e.target.value)} placeholder={`Image ${i + 1} URL`} />
                    <button className="admin-btn ghost" onClick={() => update('images', draft.images.filter((_: any, j: number) => j !== i))}>Remove</button>
                  </div>
                ))}
                <button className="admin-btn ghost" onClick={() => update('images', [...(draft.images || []), ''])}>+ Add image</button>
              </div>
            </div>
            <div className="admin-actions">
              <button className="admin-btn" onClick={save}>Save product</button>
              {draft.id && <button className="admin-btn danger" onClick={() => remove(draft.id)}>Delete</button>}
              <button className="admin-btn ghost" onClick={() => setEditing(null)}>Cancel</button>
            </div>
          </div>
        )}
      </div>
    </>
  );
}

function Pages({ pages, editing, setEditing, reload }: any) {
  const blank = { id: '', slug: '', title: '', blocks: [] as any[] };
  const draft = editing || null;

  async function save() {
    if (!draft) return;
    if (draft.id) await api.put('pages', draft.id, draft);
    else await api.post('pages', draft);
    setEditing(null);
    reload();
  }
  async function remove(id: string) {
    if (!confirm('Delete this page?')) return;
    await api.del('pages', id);
    setEditing(null);
    reload();
  }
  function update(field: string, value: any) { setEditing({ ...draft, [field]: value }); }
  function addBlock(type: string) { update('blocks', [...(draft.blocks || []), newBlock(type)]); }
  function setBlock(i: number, block: any) { const b = [...draft.blocks]; b[i] = block; update('blocks', b); }
  function removeBlock(i: number) { update('blocks', draft.blocks.filter((_: any, j: number) => j !== i)); }
  function moveBlock(i: number, dir: -1 | 1) {
    const b = [...draft.blocks]; const j = i + dir;
    if (j < 0 || j >= b.length) return;
    [b[i], b[j]] = [b[j], b[i]];
    update('blocks', b);
  }
  function applyTemplate(name: string) { update('blocks', JSON.parse(JSON.stringify(TEMPLATES[name]))); }

  return (
    <>
      <h1>Pages</h1>
      <div style={{ display: 'grid', gridTemplateColumns: '280px 1fr', gap: '1.5rem' }}>
        <div>
          <button className="admin-btn" onClick={() => setEditing({ ...blank })}>+ New page</button>
          <div className="admin-list" style={{ marginTop: '1rem' }}>
            {pages.map((p: any) => (
              <div key={p.id} className={`admin-list-item ${draft?.id === p.id ? 'active' : ''}`} onClick={() => setEditing(p)}>
                <div>{p.title}<small>/l/{p.slug}</small></div>
              </div>
            ))}
          </div>
        </div>

        {draft && (
          <div className="admin-card">
            <div className="admin-row">
              <div><label className="admin-label">Title</label><input className="admin-input" value={draft.title} onChange={(e) => update('title', e.target.value)} /></div>
              <div><label className="admin-label">URL slug</label><input className="admin-input" value={draft.slug} onChange={(e) => update('slug', e.target.value)} /></div>
            </div>

            <div style={{ marginTop: '1rem' }}>
              <label className="admin-label">Start from a template</label>
              <div style={{ display: 'flex', gap: '.5rem', flexWrap: 'wrap' }}>
                {Object.keys(TEMPLATES).map((t) => (
                  <button key={t} className="admin-btn ghost" onClick={() => applyTemplate(t)}>{t}</button>
                ))}
              </div>
            </div>

            <div className="block-editor">
              {(draft.blocks || []).map((b: any, i: number) => (
                <div className="block" key={i}>
                  <div className="block-head">
                    <strong>{b.type}</strong>
                    <div className="block-controls">
                      <button onClick={() => moveBlock(i, -1)}>↑</button>
                      <button onClick={() => moveBlock(i, 1)}>↓</button>
                      <button onClick={() => removeBlock(i)}>×</button>
                    </div>
                  </div>
                  {b.type === 'hero' && (
                    <>
                      <div className="admin-row"><div><label className="admin-label">Heading</label><input className="admin-input" value={b.heading} onChange={(e) => setBlock(i, { ...b, heading: e.target.value })} /></div><div><label className="admin-label">Sub</label><input className="admin-input" value={b.sub} onChange={(e) => setBlock(i, { ...b, sub: e.target.value })} /></div></div>
                      <div className="admin-row"><div><label className="admin-label">CTA label</label><input className="admin-input" value={b.cta} onChange={(e) => setBlock(i, { ...b, cta: e.target.value })} /></div><div><label className="admin-label">CTA href</label><input className="admin-input" value={b.ctaHref} onChange={(e) => setBlock(i, { ...b, ctaHref: e.target.value })} /></div></div>
                    </>
                  )}
                  {b.type === 'text' && (
                    <>
                      <div className="admin-row full"><div><label className="admin-label">Heading</label><input className="admin-input" value={b.heading} onChange={(e) => setBlock(i, { ...b, heading: e.target.value })} /></div></div>
                      <div className="admin-row full"><div><label className="admin-label">Body</label><textarea className="admin-textarea" value={b.body} onChange={(e) => setBlock(i, { ...b, body: e.target.value })} /></div></div>
                    </>
                  )}
                  {b.type === 'image' && (
                    <>
                      <div className="admin-row full"><div><label className="admin-label">Image URL</label><input className="admin-input" value={b.src} onChange={(e) => setBlock(i, { ...b, src: e.target.value })} /></div></div>
                      <div className="admin-row"><div><label className="admin-label">Alt</label><input className="admin-input" value={b.alt} onChange={(e) => setBlock(i, { ...b, alt: e.target.value })} /></div><div><label className="admin-label">Caption</label><input className="admin-input" value={b.caption} onChange={(e) => setBlock(i, { ...b, caption: e.target.value })} /></div></div>
                    </>
                  )}
                  {b.type === 'features' && (
                    <>
                      <div className="admin-row full"><div><label className="admin-label">Heading</label><input className="admin-input" value={b.heading} onChange={(e) => setBlock(i, { ...b, heading: e.target.value })} /></div></div>
                      {(b.items || []).map((it: any, j: number) => (
                        <div className="admin-row" key={j}>
                          <div><label className="admin-label">Feature {j + 1} title</label><input className="admin-input" value={it.title} onChange={(e) => { const items = [...b.items]; items[j] = { ...it, title: e.target.value }; setBlock(i, { ...b, items }); }} /></div>
                          <div><label className="admin-label">Body</label><input className="admin-input" value={it.body} onChange={(e) => { const items = [...b.items]; items[j] = { ...it, body: e.target.value }; setBlock(i, { ...b, items }); }} /></div>
                        </div>
                      ))}
                      <button className="admin-btn ghost" onClick={() => setBlock(i, { ...b, items: [...b.items, { title: '', body: '' }] })}>+ Add feature</button>
                    </>
                  )}
                  {b.type === 'cta' && (
                    <>
                      <div className="admin-row"><div><label className="admin-label">Heading</label><input className="admin-input" value={b.heading} onChange={(e) => setBlock(i, { ...b, heading: e.target.value })} /></div><div><label className="admin-label">Body</label><input className="admin-input" value={b.body} onChange={(e) => setBlock(i, { ...b, body: e.target.value })} /></div></div>
                      <div className="admin-row"><div><label className="admin-label">Button label</label><input className="admin-input" value={b.button} onChange={(e) => setBlock(i, { ...b, button: e.target.value })} /></div><div><label className="admin-label">Button href</label><input className="admin-input" value={b.href} onChange={(e) => setBlock(i, { ...b, href: e.target.value })} /></div></div>
                    </>
                  )}
                </div>
              ))}
            </div>

            <div style={{ marginTop: '1rem', display: 'flex', gap: '.5rem', flexWrap: 'wrap' }}>
              <button className="admin-btn ghost" onClick={() => addBlock('hero')}>+ Hero</button>
              <button className="admin-btn ghost" onClick={() => addBlock('text')}>+ Text</button>
              <button className="admin-btn ghost" onClick={() => addBlock('image')}>+ Image</button>
              <button className="admin-btn ghost" onClick={() => addBlock('features')}>+ Features</button>
              <button className="admin-btn ghost" onClick={() => addBlock('cta')}>+ CTA</button>
            </div>

            <div className="admin-actions">
              <button className="admin-btn" onClick={save}>Save page</button>
              {draft.id && <button className="admin-btn danger" onClick={() => remove(draft.id)}>Delete</button>}
              <button className="admin-btn ghost" onClick={() => setEditing(null)}>Cancel</button>
              {draft.slug && <a className="admin-btn gold" href={`/l/${draft.slug}`} target="_blank" rel="noreferrer">Preview</a>}
            </div>
          </div>
        )}
      </div>
    </>
  );
}

function Settings({ settings, reload }: any) {
  const [local, setLocal] = useState<any>(settings);
  useEffect(() => { setLocal(settings); }, [settings]);

  async function save() {
    await api.put('settings', 'x', local);
    reload();
  }

  return (
    <>
      <h1>Settings</h1>
      <div className="admin-card">
        <div className="admin-row"><div><label className="admin-label">Logo text</label><input className="admin-input" value={local.logo || ''} onChange={(e) => setLocal({ ...local, logo: e.target.value })} /></div><div><label className="admin-label">Announcement bar</label><input className="admin-input" value={local.announcement || ''} onChange={(e) => setLocal({ ...local, announcement: e.target.value })} /></div></div>
        <div className="admin-row full"><div><label className="admin-label">Hero heading</label><input className="admin-input" value={local.heroHeading || ''} onChange={(e) => setLocal({ ...local, heroHeading: e.target.value })} /></div></div>
        <div className="admin-row full"><div><label className="admin-label">Hero sub</label><textarea className="admin-textarea" value={local.heroSub || ''} onChange={(e) => setLocal({ ...local, heroSub: e.target.value })} /></div></div>
        <div className="admin-row full"><div><label className="admin-label">Footer text</label><textarea className="admin-textarea" value={local.footerText || ''} onChange={(e) => setLocal({ ...local, footerText: e.target.value })} /></div></div>
        <div className="admin-actions"><button className="admin-btn" onClick={save}>Save settings</button></div>
      </div>
    </>
  );
}
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400">
  <rect width="400" height="400" fill="#EBE8DD"/>
  <rect x="140" y="90" width="120" height="230" rx="6" fill="url(#g1)"/>
  <rect x="135" y="82" width="130" height="18" rx="4" fill="#2b3a32"/>
  <rect x="155" y="150" width="90" height="120" fill="none" stroke="#B8A88A" stroke-width="1.5"/>
  <text x="200" y="195" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">FIRST</text>
  <text x="200" y="215" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">FLUSH</text>
  <text x="200" y="240" text-anchor="middle" font-family="Inter,sans-serif" font-size="8" letter-spacing="3" fill="#F4EFE6">1849</text>
  <defs>
    <linearGradient id="g1" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#26332c"/>
      <stop offset="0.5" stop-color="#546b5e"/>
      <stop offset="1" stop-color="#232f28"/>
    </linearGradient>
  </defs>
</svg>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400">
  <rect width="400" height="400" fill="#EBE8DD"/>
  <rect x="140" y="90" width="120" height="230" rx="6" fill="url(#g2)"/>
  <rect x="135" y="82" width="130" height="18" rx="4" fill="#3a2c20"/>
  <rect x="155" y="150" width="90" height="120" fill="none" stroke="#B8A88A" stroke-width="1.5"/>
  <text x="200" y="195" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">SECOND</text>
  <text x="200" y="215" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">FLUSH</text>
  <text x="200" y="240" text-anchor="middle" font-family="Inter,sans-serif" font-size="8" letter-spacing="3" fill="#F4EFE6">MUSCATEL</text>
  <defs>
    <linearGradient id="g2" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#3a2c20"/>
      <stop offset="0.5" stop-color="#8a6f4d"/>
      <stop offset="1" stop-color="#33281d"/>
    </linearGradient>
  </defs>
</svg>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 400">
  <rect width="400" height="400" fill="#EBE8DD"/>
  <rect x="140" y="90" width="120" height="230" rx="6" fill="url(#g3)"/>
  <rect x="135" y="82" width="130" height="18" rx="4" fill="#3c423d"/>
  <rect x="155" y="150" width="90" height="120" fill="none" stroke="#B8A88A" stroke-width="1.5"/>
  <text x="200" y="195" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">GREEN</text>
  <text x="200" y="215" text-anchor="middle" font-family="Georgia,serif" font-size="14" fill="#B8A88A">LEAF</text>
  <text x="200" y="240" text-anchor="middle" font-family="Inter,sans-serif" font-size="8" letter-spacing="3" fill="#F4EFE6">SNOWMELT</text>
  <defs>
    <linearGradient id="g3" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0" stop-color="#3c423d"/>
      <stop offset="0.5" stop-color="#8b948c"/>
      <stop offset="1" stop-color="#363b37"/>
    </linearGradient>
  </defs>
</svg>
# The Himalayan Tea Company

Next.js 15 app for a single-origin Kangra tea brand.

## Features
- Product pages with multi-image carousel (Embla)
- SEO-perfect: metadata, OpenGraph, JSON-LD, sitemap, robots
- Admin panel at `/admin` — edit products, images, settings, pages
- Landing-page builder with pre-designed templates
- Visitor analytics (first-party, no cookies)
- Auto-reply email on contact form (Resend — optional)

## Run locally
```bash
cp .env.example .env.local
# edit .env.local (set ADMIN_PASSWORD + ADMIN_SECRET)
npm install
npm run dev

---

### ✅ What the admin can do without touching code
| Task | Where |
|---|---|
| Log in with password | `/admin/login` |
| Add/edit/delete products & reorder images | Admin → Products |
| Edit hero copy, footer, logo, announcement | Admin → Settings |
| Build a landing page from 3 templates | Admin → Pages → "productLaunch" / "seasonal" / "blank" |
| Reorder blocks (↑ ↓), add/remove any block | Admin → Pages → block editor |
| See visitor counts by page | Admin → Visitors |
| Read leads + auto-reply status | Admin → Leads |
| Log out | Sidebar |

### 🚀 First run
```bash
mkdir himalayan-tea && cd himalayan-tea
# paste all files above into the paths shown
cp .env.example .env.local
npm install
npm run dev