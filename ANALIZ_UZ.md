# SAFAR Sayohat Agentligi — UX/UI tahlili va o‘zbekchaga o‘tkazish hisoboti

Loyiha: sof HTML/CSS (JS yo‘q), 5 sahifa — `index.html`, `pages/destination.html`, `pages/tours.html`, `pages/about.html`, `pages/contact.html`; 6 ta CSS fayl.
Tahlil usuli: barcha HTML/CSS fayllarni to‘liq o‘qish, bosh sahifani Chrome (headless) da 1440px va 390px kenglikda ko‘rish. Boshqa sahifalar kod bo‘yicha tekshirildi.

Holat belgilari: **[Tuzatildi]** — shu ishda kodda tuzatildi · **[Qisman]** — yaxshilandi, lekin to‘liq emas · **[Ochiq]** — tegilmadi (qaror yoki yangi kontent/backend kerak).

---

## 1. Qisqa xulosa

Dizayn tili izchil: to‘q yashil + oltin palitra, Playfair Display + Inter juftligi, aniq bo‘limlar ketma-ketligi. Asosiy muammolar vizual emas, **funksional va foydalanish qulayligi** bilan bog‘liq:

1. Sayt «ishlayotgandek» ko‘rinadi, lekin tugmalar, filtrlar, FAQ va forma haqiqatda deyarli hech narsa qilmaydi (JS yo‘q va muqobil CSS/HTML yechimlari ishlatilmagan).
2. Mobil menyu klaviatura va ekran o‘qish dasturlari uchun yopiq edi.
3. Qat’iy `px` o‘lchamlar (kartalar, bo‘limlar balandligi) kichik ekranlarda matnni kesadi va tarjimadan keyingi uzunroq o‘zbekcha matnga mos kelmasdi.
4. Kontent xatolari: bir xil takrorlangan kartalar, rasmga mos kelmaydigan alt matnlar, noto‘g‘ri havolalar.

---

## 2. Topilmalar (ta’sir darajasi bo‘yicha)

### Blocker

| # | Muammo | Kimga/qanday zarar | Qayerda | Holat |
|---|---|---|---|---|
| B1 | Mobil menyu tugmasi `display:none` checkbox; klaviatura bilan ochib bo‘lmaydi, label’da matn yo‘q, yopiq menyu havolalari Tab’da qoladi | Klaviatura/ekran o‘qish dasturi foydalanuvchisi 868px dan past ekranda saytda yura olmaydi (WCAG 2.1.1, 4.1.2) | `css/style.css` burger bloki | **Tuzatildi**: checkbox ko‘rinmas, lekin fokuslanadi; label’da ekran o‘qish matni; yopiq menyu `visibility:hidden`; fokus halqasi |
| B2 | Deyarli barcha CTA ishlamaydi: bosh sahifada 15 ta `<button>`dan 14 tasi hech narsa qilmaydi; ~30 ta `href="#"`; «Book/Batafsil» tugmalari, «Explore Tours», filtr chiplari | Foydalanuvchi asosiy vazifani (tur tanlash, bog‘lanish) bajara olmaydi; kartalar `cursor:pointer` bilan bosiladigandek ko‘rinadi | barcha sahifalar | **Tuzatildi**: tugmalar haqiqiy havolalarga (`#bo‘lim`, `tours.html`, `contact.html`, `t.me`, `tel:`) aylantirildi |
| B3 | Filtr, qidiruv, select’lar (bitta `<option>`li), «Katalogni filtrlash» — JS’siz ishlamaydi, kartalar soni qotirilgan («8», «6») | Foydalanuvchi aldanadi: filtr bor, lekin natija o‘zgarmaydi | `tours.html`, `destination.html` | **Qisman**: chiplar bo‘limga o‘tuvchi havolalarga aylandi, select’larga label/`disabled` berildi; haqiqiy filtr uchun backend yoki `:has()` + radio yechimi kerak — **Ochiq** |
| B4 | Aloqa formasi: `mailto:` + `method="POST"`, ortiqcha `</div>` maketni buzgan, email maydoni yo‘q, rozilik/maxfiylik yo‘q, muvaffaqiyat/xato holati yo‘q | Pochta ilovasi yo‘q qurilmada yuborish umuman ishlamaydi; javob qaytarish kanali yo‘q | `contact.html`, `index.html` | **Qisman**: struktura tuzatildi, email/autocomplete/maxfiylik qo‘shildi, «tugma pochta ilovasini ochadi» izohi berildi. Haqiqiy yuborish uchun server (Formspree/Netlify) kerak — **Ochiq** |
| B5 | FAQ: faqat tugmalar, javoblar DOM’da yo‘q, `id="faq"` yo‘q (footer havolasi ishlamaydi) | Foydalanuvchi savollarga javob ola olmaydi | `contact.html` | **Tuzatildi**: `<details><summary>`, o‘zbekcha javoblar, `#faq` |
| B6 | Qat’iy o‘lchamlar: `.tour-card 373×688`, `.destinations height:1226`, `.contact height:750`, `.inputs height:800`, FAQ 928px, nowrap flex qatorlar | ≤1280px da kartalar bir-birining ustiga tushadi/kesiladi, gorizontal skroll; tarjimadan keyin matn kesiladi | barcha sahifa CSS’lari | **Tuzatildi** (`min-height`, `auto`, `flex-wrap`, ustunlar soni media-query bilan) |

### Major

| # | Muammo | Qayerda | Holat |
|---|---|---|---|
| M1 | **Kontrast.** Fokus halqasi `#FDCD7E` ochiq fonda ~1.2–1.5:1 (talab ≥3:1); `#717976` 9–13px matn ~4.0–4.5:1; sarlavhadagi aktiv chiziq 1.2:1; forma maydoni chegarasi ~1.1:1; `#C0C8C5` sarlavha ~1.7:1 | `style.css`, barcha sahifa CSS | **Tuzatildi**: qo‘sh rangli fokus halqasi, matn `#5C6361`, maydon chegarasi `#7E8A86`, chiziq oltin-jigarrang |
| M2 | **Kichik shrift:** ~21 joyda 9–10px (yorliqlar, narx, chiplar) | barcha CSS | **Tuzatildi** (11px) — 16px talab emas, lekin yorliq/chip uchun minimum |
| M3 | **Alt matnlar noto‘g‘ri:** tours’da 6 rasmning hammasi bir xil alt; bosh sahifada tur rasmlari/testimonial alt’lari ismga va rasmga mos emas; dekorativ ikonlar `alt="Icon"`, `"Icon-1"` | barcha sahifa | **Tuzatildi**: rasm ko‘rilib, mos o‘zbekcha alt; dekorativlar `alt=""` |
| M4 | **Kontent nusxasi:** destination’da 8 karta bir xil («Samarkand & Bukhara», bir xil tavsif/narx), kesilgan tavsif, «Shengen» xatosi; tours’da 4 ta bir xil «highlights», mos kelmaydigan ikonlar | `destination.html`, `tours.html` | **Tuzatildi**; yangi matn/narxlar taxminiy — **egasi tasdiqlashi kerak** |
| M5 | **CSS xatolari:** `grid .btn-primary:hover` (tours.css), `hover .seasonal-heading p` (destination.css), birliksiz `width:278;`, `.inquiries-head h1` vs h2, o‘lik selektorlar, `url('/assets/…')` ildiz-absolyut yo‘l (fayldan ochilganda hero fon yo‘qoladi) | CSS fayllar | **Tuzatildi** |
| M6 | **Buzilgan/placeholder havolalar:** `href="example@gmail.com"` ×3, `contact.html/#faq`, `#custom-itinerary`, `t.me/...`, Privacy/Terms `href`siz, ijtimoiy ikonlar nomsiz, `fa-regular fa-camera` bo‘sh, CSS’siz `icon-telegram` | HTML | **Tuzatildi** |
| M7 | **Semantika:** `<button>` ichida `<p>`/`<div>`; sarlavha tartibi (footer h4→h3, h5 sakrash); testimonial `<blockquote>` emas; reyting yulduzlarida matn yo‘q; `aria-current` yo‘q; skip-link yo‘q | HTML | **Tuzatildi** (asosiy qismi); yulduzlarga `role="img"` + `aria-label` |
| M8 | **Shrift:** `body`ga shrift berilmagan — menyu, havolalar, iqtiboslar brauzerning Times shriftida chiqardi (Inter/Playfair faqat bir qism elementlarda). `<button>`lar esa tizim shriftida | `style.css` | **Tuzatildi**: `body{font-family:Inter}`, form elementlari `inherit`. *Ko‘zga ko‘rinadigan o‘zgarish — niyat qilingan dizaynga yaqinlashtiradi* |
| M9 | **Bosish zonasi <44px:** chiplar 36px, footer havolalari ~20–24px, burger 28–38px, `.pack-email a` 24px, ijtimoiy ikonlar 34px | CSS | **Tuzatildi** (≥44px) |
| M10 | **Kirish:** mobil sarlavhada telefon/Telegram butunlay yashirilgan | `style.css` | **Qisman**: 520px gacha telefon ko‘rinadi, undan pastda yashirin (menyuga sig‘maydi) |
| M11 | **Dizayn tizimi:** `home.css` `var()` ishlatmaydi (0 marta), ~20 xil shrift o‘lchami, breakpoint’lar har faylda boshqa (1024/868/520, 1100/768/480, 1024/900/600); hover’da `font-weight` o‘zgarib layout siljiydi; `prefers-reduced-motion` yo‘q | CSS | **Qisman**: hover siljishi va reduced-motion tuzatildi; tokenlarga o‘tkazish va breakpoint’larni birlashtirish — **Ochiq** (katta refaktor) |

### Minor

- **SEO/meta:** `<title>` umumiy («Travel Agency»), `meta description`, favicon, OG, hreflang yo‘q; shriftlar `@import` (render-blocking). → **Tuzatildi**: o‘zbekcha title/description, `theme-color`, `preconnect` + `<link>`. Favicon, OG, hreflang — **Ochiq**.
- **Rasmlar:** `width/height` va `loading="lazy"` yo‘q; hero PNG 1.1 MB, `ancient-section.png` 748 KB, `Destination-Pyramids.png` 1920×1281 (373px kartada ko‘rsatiladi); noto‘g‘ri fayl nomlari (`tour-fourthtour-first.png`, `leaders-firstthird.png`). → `width/height` + `lazy` va fayl nomlari (`tour-first.png`, `leaders-third.png`) **Tuzatildi**; siqish, WebP, `srcset` — **Ochiq**.
- **Rasm ichidagi matn:** `tours-fourth.png` (Maldiv) — saytning o‘z skrinshoti qirqilgan: yuqori chapda «AFAR / ravel Agency» va menyu («Home, About, Safari, Contact Us») yozuvlari ko‘rinib turibdi; `salon-third.png`da «SAFAR» yozuvi. Birinchisi xato, rasmni almashtirish kerak — **Ochiq**.
- **Kontent ziddiyatlari:** sarlavha va footer telefonlari har xil (`+998 91 758 50 20` / `+998 71 200 00 20`); «Telegram 24/7» va «Du–Sha 09–20»; eskirgan sanalar (2024/2025); «$50,000» (USD) so‘mdagi narxlar sahifasida; har kartada bir xil «UZ · EN · RU · FR»; about xaritasi Istanbulga qaragan. → vaqt/sana/xarita/til qatori **Tuzatildi**; telefonlar uchun qaysi biri asosiyligini egasi hal qiladi — **Ochiq**.
- **Takrorlash:** header/footer 5 sahifada nusxa (JS/include yo‘qligi sababli qabul qilinadi); sinf nomlari sahifalar bo‘yicha to‘qnashadi (`.hero*`, `.btn-primary`) — **Ochiq**.
- **Ishlatilmagan fayllar:** `SAFAR Logo.png`, `footer-brand1/2.png`, `test_measure.html` — **Tuzatildi** (havola yo‘qligi tekshirilib, o‘chirildi).

### Yaxshi tomonlari

Izchil palitra va shrift juftligi; `:focus-visible` mavjud edi; formada `label for` to‘g‘ri; `viewport`/`lang` bor; `tel:` havolalar; xarita `loading="lazy"`; har bo‘limda aniq vizual ierarxiya (eyebrow → h2 → tavsif).

---

## 3. O‘zbekchaga o‘tkazish

**Qamrov:** barcha 5 sahifa (ko‘rinadigan matn, `alt`, `aria-label`, `placeholder`, `title`, `<option>`, `<title>`, `meta description`), `lang="uz"`.

**Qoidalar:**
- Lotin yozuvi; `oʻ`, `gʻ` — U+02BB (ʻ), tutuq belgisi — U+02BC (ʼ). To‘g‘ri belgi ishlatilgani `grep` bilan tekshirildi (ASCII `'` qolmagan).
- Joy nomlari: Toshkent, Samarqand, Buxoro, Turkiya, BAA, Misr, Maldiv orollari va h.k.
- Narx: `8 400 000 soʻmdan` (qattiq bo‘shliq bilan), davomiylik `5–8 KUN`, `5K / 4T`; kasr `99,4%`.
- Brendlar va identifikatorlar o‘zgarmadi: SAFAR, Telegram, `@safar_travel`, telefonlar, email, aeroport kodlari, mehmonxona brendlari.
- «Concierge/Dossier/Salon» kabi so‘zlar so‘zma-so‘z emas, o‘zbekcha tabiiy ma’noda berildi.

**Tarjima tufayli kerak bo‘lgan CSS o‘zgarishlari:** o‘zbekcha matn inglizchadan ~15–30% uzun, shuning uchun matn qutilaridagi qat’iy `width/height` → `min-height`/`auto` + `flex-wrap`.

---

## 4. Tekshiruv natijalari va cheklovlar

**Tekshirildi (Chrome headless + DevTools):**
- 5 sahifa × 9 kenglik (360, 390, 600, 768, 900, 1024, 1100, 1280, 1440 px): gorizontal toshish **yo‘q** (tuzatishdan oldin: destination 390px da, bosh sahifa 1024px da toshgan).
- Mobil menyu klaviatura bilan: Tab → `nav-toggle` → Space — menyu ochiladi (`visibility: visible`), yopiq holatda Tab’ga tushmaydi.
- Barcha ichki havola, rasm yo‘li va `#anchor` mavjud; takroriy `id` yo‘q.
- Matnda ASCII `'`/`’` yo‘q (faqat ʻ va ʼ), `lang="uz"` barcha sahifada, `href="#"` yo‘q; qolgan inglizcha — brend/dastur nomi («Marriott Luxury», «Premium» opsiyasi).
- Bosh sahifa, about, destination, tours, contact — vizual ko‘rib chiqildi (desktop va mobil).

**Tekshirilmadi / ochiq:**
- Axe/Lighthouse ishga tushirilmadi; kontrast qiymatlari hisob-kitob asosida.
- Safari/Firefox’da ko‘rilmadi (`:user-invalid`, `details[name]` yangi brauzerlarda to‘liq ishlaydi).
- Haqiqiy forma yuborish va filtr — backend kerak (B3, B4).
- Yangi yozilgan kontent (FAQ javoblari, maxfiylik matni, destinatsiya narxlari/tavsiflari, tour highlights) — taxminiy, egasi tasdiqlashi kerak. Destination’dagi 1, 2, 6, 7-karta rasmlari asl alt’iga mos kelmagani uchun almashtirildi.
- Telefonlar: sarlavha/kontakt `+998 91 758 50 20`, footer/tours `+998 71 200 00 20` — qaysi biri asosiyligini egasi hal qiladi.
- `tours-fourth.png` ichida saytning skrinshoti qirqimi (SAFAR menyusi) ko‘rinadi — rasmni almashtirish kerak.
- Tarjima mashina yordamida tayyorlandi: ona tilida so‘zlashuvchi o‘qib chiqishi tavsiya etiladi.
- Oldingi `Loyiha_Kamchiliklari_Tahlili.pdf` o‘qilmadi (PDF o‘qish vositasi yo‘q).
