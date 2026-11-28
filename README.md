# Amazon 2.0 (amazora) — Multi-category marketplace

React + Vite (client) aur Express + MongoDB (server) wala full-stack e-commerce project.

## Kya chahiye
- Node.js 18+
- MongoDB (local ya Atlas)
- Razorpay test keys (sirf online payment ke liye; COD bina keys ke chalta hai)

## Chalane ka tarika (Windows PowerShell)
1. Project ke main folder (`Amazon`) mein PowerShell kholo.
2. Ye commands chalao:
   ```powershell
   Copy-Item .env.example server/.env
   npm run install:all
   npm run dev
   ```
3. Browser mein kholo: http://localhost:5173  (backend: http://localhost:8000/api)

`server/.env` mein apni MongoDB, email, Razorpay, **SERPAPI_KEY** aur **ADMIN_EMAIL / ADMIN_PASSWORD** bharo (`.env.example` mein sab likha hai).
Pehli baar website kholte hi SerpAPI se products aa jate hain (neeche "Products SerpAPI se" dekho). Customer website: http://localhost:5173 · Admin panel (alag app): http://localhost:5174

## Folder structure
```
Amazon/
├─ package.json            (saare commands yahin se chalte hain)
├─ .env.example            (server/.env ka sample)
├─ client/                 (React website)
│  ├─ index.html
│  ├─ vite.config.js
│  └─ src/
│     ├─ main.jsx          (app shuru)
│     ├─ App.jsx           (saare pages aur routes)
│     ├─ Avatar.jsx        (profile photo)
│     ├─ ProductGallery.jsx(product photo zoom + thumbnails)
│     ├─ Stars.jsx         (asli rating ke stars)
│     ├─ Breadcrumbs.jsx   (Home › Category › Product)
│     ├─ Mini.jsx          (chhota product card)
│     ├─ OfferStrip.jsx    (upar ki offer patti)
│     ├─ BannerSlider.jsx  (Home ki sliding banner)
│     ├─ DealOfDay.jsx     (Today's deals)
│     ├─ HomeExtras.jsx    (Shop by brand)
│     ├─ RecentlyViewed.jsx(Recently viewed row)
│     ├─ CardExtras.jsx    (discount tag, laal dil, Quick view)
│     ├─ ProductExtras.jsx (Buy Now, reviews, specs, similar products, mobile Add to Cart bar)
│     ├─ ListingExtras.jsx (breadcrumbs + mobile Filters button)
│     ├─ CartExtras.jsx    (free delivery bar, checkout steps, order timeline, Buy again)
│     ├─ Chrome.jsx        (search suggestions, mobile bottom bar, newsletter, trust badges)
│     ├─ InfoPages.jsx     (About, Contact, Help, Returns, Privacy, Terms)
│     ├─ shopData.js       (common helpers)
│     ├─ state.jsx         (user / cart / wishlist ka common data)
│     ├─ api.js            (backend se baat karne wala axios)
│     ├─ alerts.js         (toast notifications)
│     ├─ OtpModal.jsx      (OTP popup)
│     └─ style.css         (poora design)
└─ server/                 (Express backend)
   ├─ server.js            (saare API routes)
   ├─ models.js            (MongoDB models)
   ├─ utils.js             (login guard, token, mail)
   ├─ catalog.js           (categories / sub-categories)
   ├─ serpapi.js           (NAYA: products SerpAPI / Google Shopping se laata hai, cache ke saath)
   ├─ admin.js             (NAYA: admin panel ke saare API routes + admin account banana)
   ├─ extras.js            (newsletter route)
   ├─ testMail.js          (email / OTP check karne wali script)
   (products.json, importProducts.js, fillImages.js aur fillRealImages.js hata di gayi hain)
```

Admin panel ab website se **alag app** hai: `admin/` folder (port 5174). Usme `Admin.jsx` (dhancha + Dashboard), `AdminLogin.jsx`, `AdminProducts.jsx`
(Products + Add / Edit form), `AdminOrders.jsx`, `AdminPeople.jsx` (Customers, Coupons, Support), `adminState.jsx`, `api.js`, `admin.css`.
Website ka `client/` folder bilkul pehle jaisa hai (usme admin ka koi code nahi).

## Commands
- `npm run install:all`  -> root, server aur client ke packages install
- `npm run dev`          -> backend + frontend dono chalao
- `npm run test:mail`    -> test email bhejta hai (OTP kyun nahi aa raha, ye check karne ke liye)
- `npm run build`        -> client ka production build

## Features
- Home, category/search listing, product detail, login/register (email OTP), cart, wishlist, checkout, orders, account
- Search, category / sub-category / price / rating filters, sorting, pagination
- Cart / wishlist, address management, COD aur Razorpay payment (signature verification + webhook)
- Online payment se pehle email OTP, order / payment ke email, sweet alerts (toast)
- Wishlist se "Add to Cart" karne par item wishlist se hat jata hai
- UI/UX: offer patti, auto-sliding banner, Today's deals, Shop by brand, Recently viewed
- Product card: asli rating stars, discount / Top rated tag, wishlist ka laal dil, Quick view popup
- Product page: breadcrumbs, Buy Now, low-stock note, specifications, reviews (dekhna + likhna), similar products
- Cart: free delivery progress bar, khali cart par suggestions | Checkout: step indicator | Orders: tracking timeline + Buy it again
- Mobile: neeche bottom navigation + Categories drawer, Filters button, product page par chipka Add to Cart bar
- Search box mein suggestions, footer ke upar trust badges + newsletter
- Product photo preview: click par zoom (react-medium-image-zoom package), kai photos ho to thumbnails
- Profile photo (multer se server par save): Account page par upload / change / remove, header mein gol photo, photo na ho to naam ke initials

## Razorpay webhook
Webhook URL: `https://YOUR_PUBLIC_DOMAIN/api/payments/webhook`, events: `payment.captured` aur `payment.failed`. Secret kabhi client mein mat daalo.

## Email (OTP)
OTP na aaye to pehle `npm run test:mail` chalao, wo bata dega ki kya galat hai.
`server/.env` mein `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS`, `MAIL_FROM` bharo (Gmail ke liye App Password). SMTP khali ho to OTP server ke terminal mein print hota hai.


## Naya code - sirf aapke 33 concepts
Maine jo naya code jorda hai (client + server), wo sirf aapki 33 cheezon (try, catch, if, else, .then, json,
populate, params, req.body, .end, props, Math.ceil, useEffect, useState, async/await, createContext/useContext,
useParams, formData, Model.create, updateOne, countDocuments, module.exports ...) aur un cheezon se bana hai
jo aapke purane code mein pehle se thi (.map, axios, JSX, useNavigate etc.). Purana code waisa hi hai.

## Session (cookie nahi)
Login token aur "Recently viewed" ab cookie mein nahi, browser ke session (sessionStorage) mein rehte hain.
Tab ya browser band karte hi login aur recently viewed khatam ho jate hain; dobara login karna padega.

## Products SerpAPI se (Google Shopping)
- Website ke saare products ab SerpAPI se aate hain. Purani products.json, import aur image scripts hata di gayi hain.
- `server/.env` mein `SERPAPI_KEY` zaroori hai (https://serpapi.com/manage-api-key).
- Jab koi category / sub-category kholta hai ya search karta hai, server SerpAPI se products laakar MongoDB mein save kar leta hai.
  MongoDB mein isliye rakhte hain ki cart, wishlist, orders, reviews pehle jaise chalein.
- **Credit bachane ke liye**: ek search (1 credit) `SERPAPI_CACHE_HOURS` (default 72 ghante) tak yaad rehti hai, us beech dobara SerpAPI ko nahi jati.
  Category page par ek baar mein maximum `SERPAPI_MAX_PER_REQUEST` (default 3) nayi searches hoti hain.
- Website bilkul khali ho to Home kholte hi har category ki pehli sub-category ki search (8 credits) apne aap chal jati hai.
- Admin panel mein SerpAPI ka koi zikr ya control nahi hai. Website apne aap SerpAPI se products laati hai, admin apne products alag se add karta hai.
- Price asli (Google Shopping ka) hota hai. Purana price (MRP) tabhi dikhta hai jab SerpAPI deta hai, warna discount nahi dikhta.
- SerpAPI stock nahi batata, isliye har naye product ka stock 50 se shuru hota hai. Admin panel se badal sakte ho.

## Admin panel
- Address: http://localhost:5174 (alag app, apna login page; website ke header mein admin ka koi link nahi hai)
- Admin banane ke liye `server/.env` mein `ADMIN_EMAIL` aur `ADMIN_PASSWORD` likho, server chalao (account apne aap ban jata hai),
  phir http://localhost:5174 par usi email-password se login karo. Agar wo email pehle se customer hai to uska role admin ho jata hai.
- Pages: Dashboard (kamai, orders, low stock), Products, Orders (status badlo: customer ko mail + notification jati hai),
  Customers, Coupons (naya banao, on / off), Support messages.
- **Product add karna** (Products > "+ Add product"): title, slug (title se apne aap banta hai), meta title (70 akshar tak),
  meta description (160 akshar tak), category, sub-category, brand, price, MRP, stock, description, photos, visible / featured.
  Photos computer se upload hoti hain (server/uploads/products mein save, PNG / JPG / WEBP / GIF, 3 MB tak, max 8) ya photo ka link paste kar sakte ho.
  Pehli photo main photo hoti hai. Website par product `/product/<slug>` address par khulta hai aur uska meta title / meta description page par lagta hai.
- Website ke products bhi isi list mein dikhte hain: unka price, stock, slug, meta, photos edit kar sakte ho ya **Hide** kar sakte ho.
  Edit kiya hua product waisa hi rehta hai, website ka refresh use nahi badalta.
- Products delete nahi hote, **Hide** hote hain (taaki purane orders na bigdein).
- Order cancel karne par stock wapas jud jata hai. Online (Razorpay) payment wale order ka refund Razorpay dashboard se karna hoga.
