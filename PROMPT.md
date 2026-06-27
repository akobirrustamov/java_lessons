# Dasturlash kursi roadmap — Texnik talab (Prompt)

> Bu hujjat **boshqa dasturlash tili uchun roadmap + prezentatsiyalar yaratishda**
> Claude'ga texnik talab sifatida foydalanish uchun yozilgan.
>
> Keyingi safar quyidagi promptni nusxalab, qaysi til uchun ekanini yozing —
> Claude xuddi shu strukturada loyiha yaratib beradi.

---

## 🎯 Loyihaning maqsadi

Dasturlash tilini noldan boshlab professional darajagacha o'rgatuvchi
**interaktiv o'quv platforma** yaratish:
- Bitta `index.html` — barcha mavzular bilan **roadmap (yo'l xaritasi)**
- Har bir mavzu uchun alohida fayl: `{topic-name}.html` — **slayd-prezentatsiya**
- Til: **O'zbek tilida**, tushunarli va batafsil
- Muallif: **Rustamov Akobir** (himoyalangan watermark)

---

## 📁 Fayl tuzilmasi

```
{language}-lessons/
├── index.html          # Roadmap (asosiy sahifa)
├── {topic-1}.html      # 1-mavzu prezentatsiyasi
├── {topic-2}.html      # 2-mavzu prezentatsiyasi (kelajakda)
├── PROMPT.md           # Bu fayl (texnik talab)
└── ...
```

Har bir mavzu fayli alohida brauzer oynasida ochilishi kerak (`target="_blank"`).

---

## 🎨 Dizayn tizimi (Design System)

### Asosiy ranglar (CSS variables)

```css
:root {
  --bg: #0a0a14;                              /* asosiy fon — qora-binafsha */
  --bg-2: #0f0f1e;
  --surface: rgba(255, 255, 255, 0.04);       /* card fon */
  --surface-hover: rgba(255, 255, 255, 0.07);
  --border: rgba(255, 255, 255, 0.08);
  --border-hover: rgba(139, 92, 246, 0.4);
  --text: #f8fafc;
  --text-dim: #94a3b8;
  --text-muted: #64748b;
  --primary: #8b5cf6;                         /* binafsha */
  --primary-2: #6366f1;                       /* ko'k-binafsha */
  --accent: #f472b6;                          /* pushti */
  --success: #10b981;                         /* yashil */
  --warning: #f59e0b;                         /* sariq */

  /* Slayd uchun (oq fon) */
  --slide-bg: #ffffff;
  --slide-bg-2: #fafafe;
  --code-bg: #0f172a;
  --code-text: #e2e8f0;
}
```

### Shriftlar (Google Fonts)
- **Inter** (400, 500, 600, 700, 800, 900) — asosiy matn
- **JetBrains Mono** (400, 500, 600) — kod, raqamlar, monospace elementlar

### Background effektlari
- **Animatsiyali blob'lar** (3 ta blur dog'i — binafsha, pushti, ko'k)
- **Grid pattern** (yengil to'r) — radial mask bilan
- `backdrop-filter: blur(20px) saturate(180%)` — nav va controls uchun

---

## 📑 1. `index.html` — Roadmap

### Tuzilishi (sequence)

1. **Bg-effects** (3 ta animated blobs)
2. **Sticky nav** (logo + nav links) — backdrop blur bilan
3. **Hero header**:
   - Pulse dot bilan badge: `● {Language} Developer Roadmap 2026`
   - Katta gradient sarlavha (oq → binafsha → pushti)
   - Tavsif (1-2 jumla)
   - 3 ta statistika (mavzu soni, misollar, daraja)
4. **Roadmap section**:
   - Section title + subtitle
   - **Grid** (auto-fill, minmax(340px, 1fr), 20px gap)
   - Har bir card:
     - Card number badge (`01`, `02`...) — JetBrains Mono, gradient
     - Mavzu nomi
     - Status badge (`● Tayyor` / `● Tez orada`)
     - Mavzu ichidagi mavzular listi (• punktlari bilan)
     - CTA button: "Prezentatsiyani ochish →" (faqat tayyor bo'lganda)
5. **Footer** — kichik credit
6. **Watermark himoya skripti** (eng pastda)

### Card behavior
- Hover'da: 6px tepaga, gradient border + radial glow
- Disabled state: opacity 0.5, gray button "Tez orada"
- Faqat tayyor mavzular `target="_blank"` bilan ochilishi
- Status indicator yashil (Tayyor) — pulse animatsiyasi bilan

---

## 🎬 2. `{topic}.html` — Slayd prezentatsiya

### Sahifa tuzilmasi

```
┌─ Top bar (fixed) ─────────────────────────┐
│ [← Roadmap]  [● N-Mavzu · Topic]   3 / 32 │
├───────────────────────────────────────────┤
│                                           │
│         [Active slide here]               │
│                                           │
├───────────────────────────────────────────┤
│ ▬▬▬▬▬▬▬▬▬▬ progress line ▬▬▬▬▬▬▬▬▬▬▬▬▬   │
│ [◀] [01 02 03 04 05 06 07 ...] [▶]        │
└───────────────────────────────────────────┘
```

### Slayd turlari

**A. Title slide (mavzu sarlavhasi)**
- `class="slide slide-title"` (markazga tekislangan)
- Topic pill ("⚡ 1-Mavzu")
- Katta gradient h1
- Tavsif paragrafi
- Note box

**B. Oddiy mavzu slaydi**
- `<h2>` (binafsha vertical bar bilan)
- Paragraf
- `<ul>` — punkt belgisi gradient nuqta + glow
- Kod bloki (macOS window — keyin auto-wrap)
- Note box (sariq, 💡 emoji bilan)

**C. Definition grid slaydi**
- `def-grid` — 2-3 ta card grid (masalan: JDK / JRE / JVM)
- Har bir `def-card` da: katta sarlavha (monospace, binafsha) + qisqa tavsif

**D. LeetCode-style problem slaydi**
- `<div class="problem-header">`:
  - `problem-num` badge (⚡ Masala N)
  - `difficulty` badge (Easy / Medium / Hard — rang bilan)
- `<h2>` masala sarlavhasi
- `<p>` qisqa tavsif
- `<ul>` shartlar (ixtiyoriy)
- `<div class="example-block">` — kulrang fon, "MISOL" sarlavha bilan
- `<div class="constraints">` — ko'k dashed, "Maslahat", "G'oya", "Lifehack" bilan (ixtiyoriy)
- `<pre data-filename="...">` — yechim kodi (auto-wrap'da macOS oynasi va **yashirin toggle** bilan o'raladi)

### Slayd CSS
- Background: oq → kulrang gradient
- Border-radius: 28px
- Box shadow: katta + 1px inset glow
- Pseudo elementlar: ::before va ::after — radial gradient dog'lari
- Animatsiya: slideIn (opacity + translateY + scale)
- Max-width: 1180px, padding: 56px 64px

---

## 💻 3. macOS-style kod oynasi (auto-wrap)

JavaScript orqali sahifa yuklanganda **har bir `<pre>` bloki avtomatik**
quyidagi strukturaga o'raladi:

```
┌────────────────────────────────────┐
│ 🔴 🟡 🟢   ☕ Main.java   [↺] [📋] │  ← header (gradient)
├────────────────────────────────────┤
│                                    │
│  public class Main {               │  ← editable
│      public static void main(...) {│
│          ...                       │
│      }                             │
│  }                                 │
│                                    │
└────────────────────────────────────┘
```

### Xususiyatlar
- **Traffic lights** (chap yuqorida): qizil, sariq, yashil
- **Filename** (markazda): ☕ emoji (tilga mos) + `data-filename` qiymati
- **Reset button** (asl holatga qaytaradi)
- **Copy button**: bosilganda yashil ranga o'tadi, "✓ Nusxalandi" deydi, toast chiqaradi
- **Editable** (`contenteditable="true"`): binafsha caret bilan
- **Focus state**: yengil binafsha tus
- **Custom scrollbar**: binafsha-pushti gradient, 10px, webkit + firefox
- **Max-height**: 50vh (juda uzun kod uchun vertical scroll)
- **HTML escape**: `<`, `>`, `&` → `&lt;`, `&gt;`, `&amp;` (MAJBURIY!)

### Code header style
- Gradient fon (`#2d3548` → `#1e2536`)
- Traffic lights: 12px doiralar, `#ff5f57`, `#febc2e`, `#28c840`
- Action button: rgba(255,255,255,0.06) fon, hover'da binafsha

### Qoida
HTML faylda yozish: `<pre data-filename="Example.java"><code>... kod ...</code></pre>`
JavaScript qolgan barchasini avtomatik qiladi.

---

## 🔒 4. Yechimni yashirish (Solution Toggle)

**MUHIM:** Faqat `<div class="problem-header">` bo'lgan slaydlardagi
kod bloklari avtomatik yashirin holatga o'tadi.

### Mexanizm
- JavaScript `.problem-header` bo'lgan har bir slaydni topadi
- Ichidagi `.code-window` ni `.solution-content` ichiga o'raydi
- Yuqorisida **"Yechimni ko'rsatish"** tugmasi paydo bo'ladi
- Bosilganda: smooth animatsiya bilan ochiladi (max-height + opacity)
- Tugma pushti-qizilga o'tadi: "Yechimni yashirish"

### CSS tafsilotlar
```css
.solution-content {
  max-height: 0;
  opacity: 0;
  transition: max-height 0.5s, opacity 0.3s;
}
.solution-content.shown {
  max-height: 2000px;
  opacity: 1;
}
```

---

## 🧭 5. Navigatsiya — Pagination

### Bottom controls
- **Yupqa progress chizig'i** (top, 3px, gradient, glow effekti)
- **Prev arrow** (38x38 icon button)
- **Pagination dots** — **barcha slaydlarni raqam ko'rinishida** ko'rsatadi:
  - Har biri 34x34px, monospace `01`, `02`, ...
  - **Active**: gradient fon, `scale(1.08)`, katta soya
  - **Visited (o'tilgan)**: yengil binafsha tus
  - **Default**: surface fon, dim color
  - Klikda — to'g'ridan-to'g'ri o'sha slaydga o'tish
  - Active dot — avtomatik markazga scroll qilinadi
- **Next arrow** (38x38 icon button)
- Mobile: pagination horizontal scroll qilinadigan

### Keyboard navigation
- `→`, `Space`, `PageDown` → keyingi
- `←`, `PageUp` → oldingi
- `Home` → birinchi slayd
- `End` → oxirgi slayd
- **ContentEditable element'da focus bo'lsa** — keyboard nav o'chiriladi
  (foydalanuvchi kodga yozayotganda strelka tugmalari slayd almashtirmasligi uchun)

---

## 🛡️ 6. Watermark himoyasi (MUHIM!)

Barcha sahifalarda **"© Muallif: Rustamov Akobir"** belgisi bo'lishi shart,
lekin **kodda hech qanday plain matn ko'rinishida bo'lmasligi kerak**
(`Ctrl+F` topilmasligi uchun).

### Texnik talablar
1. **Char-code encoding**: Matn `String.fromCharCode([169, 32, 77, 117, ...])` orqali yig'iladi
2. **Hech qachon** "Rustamov", "Muallif", "Akobir" so'zlari kod ichida plain matn bo'lib yozilmasin
3. **Visible badge**: pastki o'ng burchakda kichik chiroyli badge (binafsha glow bilan)
4. **MutationObserver**: agar watermark DOM'dan o'chirilsa, **darhol qayta tiklanadi**
5. **setInterval (1.5s)**: zaxira tekshiruv — Observer chetlab o'tilsa
6. **5 ta yashirin nusxa**: `display: none`, `left: -99999px` joylarda
7. **Console signature**: F12'da gradient bilan chiqsin
8. **Object.defineProperty lock**: `window` obyektida o'zgartirib bo'lmas property
9. **Obfuskatsiyalangan o'zgaruvchilar**: `_a`, `_b`, `_t`, `_n`, `_x94k7t9r5_sig` kabi

### Watermark stilizatsiyasi (visible)
```css
position: fixed;
bottom: 12px; right: 12px;
background: rgba(15,15,25,0.75);
backdrop-filter: blur(10px);
border: 1px solid rgba(139,92,246,0.3);
padding: 6px 14px;
border-radius: 20px;
font-size: 11px;
color: rgba(255,255,255,0.85);
```

### Joylashish
- `index.html`: pastki o'ng (`bottom: 12px`)
- `{topic}.html`: pastki o'ng, **controls'ning ustida** (`bottom: 96px`)

---

## 🌍 7. Til va ohang

- **O'zbek tili** — barcha matnlarda (lotin alifbosi)
- ASCII apostrof (`'`) — qiyshiq apostrof emas
- "Sen" emas, "Siz" — hurmat ko'rinishida
- Texnik terminlarni **inglizcha qoldirish va izoh berish**:
  - "Variables — o'zgaruvchilar"
  - "Methods — metodlar"
  - "Loops — sikllar"
- Kod izohlarida **o'zbekcha kommentlar** — tushuntirish uchun
- Sonlarni o'qish uchun bo'shliq bilan: `1 000 000` (vergul emas)

---

## 📊 8. Mavzu kontentining tuzilmasi (MUHIM!)

Java Core misolida ishlatilgan **aniq slayd ketma-ketligi** —
boshqa mavzular ham xuddi shu sxemada bo'lishi kerak:

### Standart ketma-ketlik

1. **Title slide** — mavzu nomi + tavsif + dars maqsadi (note bilan)
2. **Asosiy tushuncha** — "{Til} nima?" (4-5 ta foyda nuqtasi)
3. **Atamalar** — `def-grid` (3 ta card) — masalan JDK/JRE/JVM, Compiler/Interpreter/Runtime
4. **Birinchi dastur** — Hello World (asosiy struktura tushuntirilgan)
5. **Variables / Identifiers** — o'zgaruvchilar
6. **Data Types** — **to'liq diapazonlari bilan**, har bir tur uchun:
   - Hajm (bayt)
   - Diapazon (aniq raqamlar bilan)
   - Maxsus eslatma (suffix kerak bo'lsa)
7. **Arifmetik operatorlar** — kod misol bilan
8. **Taqqoslash operatorlari** — kod misol bilan
9. **Mantiqiy operatorlar** — kod misol bilan
10. **🎯 4 ta masala — Mantiqiy operatorlar uchun** (LeetCode style):
    - Difficulty: 3 Easy + 1 Hard (yoki shunga yaqin)
    - Yechim YASHIRIN — tugma bilan ochiladi
11. **Qiymat berish operatorlari** (+=, -=, va h.k.)
12. **Inkrement / Dekrement** — post va pre farqi bilan
13. **Ternary operator** (?: yoki ekvivalent)
14. **🎯 1 ta masala — Ternary uchun** (Medium darajada — nested ternary)
15. **If / Else** — kod misol bilan
16. **🎯 4 ta masala — If/Else uchun**:
    - FizzBuzz (Easy) yoki shunga o'xshash
    - Grade letter (Easy)
    - Klassifikatsiya (Medium)
    - Murakkab klassifikatsiya (Hard — bosqichli soliq kabi)
17. **Switch / Pattern matching** (tilga qarab)
18. **Loops — sikllar** (asosan `for`)
19. **🎯 3 ta masala — Loops uchun**:
    - Faktorial (Easy)
    - Sonning raqamlari yig'indisi (Easy)
    - Tub son (Medium)
20. **While / Do-While**
21. **Arrays / Lists** (tilga qarab)
22. **Array bilan sikl** (for + for-each)
23. **Functions / Methods**
24. **📝 Amaliy topshiriqlar — 10 ta murakkab vazifa**:
    - Yechimsiz, faqat tavsif va input/output misol
    - Mavzularni birlashtiradi
    - Klassik algoritmlar: Two Sum, Fibonacci, Bubble Sort, Palindrome, va h.k.
25. **Title slide — Dars yakuni** + keyingi mavzu

### Qoidalar
- **Bir slayd = bir g'oya**
- **Operator/tushunchadan keyin darrov masala** — talaba o'rganib chiqib mashq qiladi
- **Difficulty progression**: Easy → Easy → Medium → Hard
- **Misol qiymatlar real bo'lsin**: "Akobir", "Tashkent", real raqamlar
- **Kod izohlari (kommentlar) o'zbekcha**
- Jami slaydlar soni: ~32-40 (mavzuga qarab)

---

## ⚡ 9. Masala formatining qat'iy strukturasi

Har bir LeetCode-style masala slaydi **aynan shu strukturada** bo'lishi shart:

```html
<section class="slide">
  <div class="problem-header">
    <span class="problem-num">⚡ Masala N</span>
    <span class="difficulty easy"><span class="dot"></span>Easy</span>
    <!-- yoki: medium / hard -->
  </div>
  <h2>Masala sarlavhasi</h2>
  <p>Qisqa tavsif. Nima yechilishi kerakligi.</p>
  <ul>
    <li>Shart 1</li>
    <li>Shart 2</li>
  </ul>
  <div class="example-block">
    <div class="example-title">Misol</div>
    Input: ... → Output: <b>natija</b><br>
    Input: ... → Output: <b>natija</b><br>
    Input: ... → Output: <b>natija</b>
  </div>
  <div class="constraints">
    <b>Maslahat / G'oya / Lifehack / Eslatma:</b> qisqa tushuntirish.
  </div>
  <pre data-filename="ProblemName.java"><code>// To'liq yechim kodi
// Bir nechta test holatlari bilan</code></pre>
</section>
```

### Misollar soni
- Har masalada **3-5 ta misol** (Input/Output)
- Misollar **edge case'larni** qamrab oladi (0, manfiy, eng katta va h.k.)

### Yechim kodi
- **Toza, ishlaydigan** kod
- O'zbekcha kommentlar (kerakli joyda)
- **Test holatlar** — har biri natijasi bilan kommentlangan
- HTML belgilar **escape qilingan**: `<`, `>`, `&`

---

## 📝 10. Amaliy topshiriqlar slaydi (Mustaqil bajarish)

Mavzuning oxiriga yaqin (Methods'dan keyin):

```html
<section class="slide">
  <div class="problem-header">
    <span class="problem-num">📝 Amaliy topshiriqlar</span>
    <span class="difficulty hard"><span class="dot"></span>Mustaqil</span>
  </div>
  <h2>10 ta murakkab vazifa</h2>
  <p>Quyidagi 10 ta vazifani <b>o'zingiz mustaqil</b> yechib ko'ring...</p>
  <ul style="font-size: 16px;">
    <li>
      <b>1. Vazifa nomi.</b> Qisqa tavsif.<br>
      <code>misol input → misol output</code>
    </li>
    <!-- ... 10 ta jami ... -->
  </ul>
  <div class="note">
    Har bir vazifa uchun: alohida metod yozing, kamida 3 ta test holati...
  </div>
</section>
```

### Vazifalar tanlash
- 10 ta vazifa — **darslar yakunidagi mavzularni birlashtiruvchi**
- **Yechimlar yo'q** — talaba mustaqil yechishi uchun
- Klassik algoritmlar:
  - Massiv ustida amallar (reverse, second largest)
  - Matematik (Armstrong, GCD, Fibonacci)
  - String (palindrome)
  - Algoritm (Bubble Sort, Two Sum)
  - Murakkab (Pascal triangle)

---

## 🔧 11. Texnik talablar

- **No build process** — sof HTML/CSS/JS (Vanilla)
- **No external CSS framework** — barchasi inline `<style>` ichida
- **No external JS library** — sof JavaScript
- **Faqat Google Fonts** — tashqi CDN sifatida
- **Mobile responsive** — `@media (max-width: 768px)` bilan
- **Print friendly** — `@media print` bilan PDF ga chop etish mumkin
- **Accessible** — semantic HTML, contrast nisbati yaxshi
- **Single file** — har bir mavzu uchun bitta `.html` fayl

---

## 📋 Tayyor prompt (nusxa olish uchun)

```
Salom! Men {LANGUAGE} dasturlash tili bo'yicha o'quv platforma yaratmoqchiman.
Iltimos, `PROMPT.md` fayli ichidagi texnik talab bo'yicha aniq xuddi shunday
roadmap + prezentatsiya tizimini yarating.

Til: {LANGUAGE} (masalan: Python / JavaScript / Go / C# / Rust / Kotlin)
Loyiha papka: {language}-lessons/
Muallif: Rustamov Akobir (watermark himoyasi bilan)

Birinchi navbatda:
1. `index.html` — roadmap (10-15 mavzu)
2. `{first-topic}.html` — birinchi mavzuning to'liq prezentatsiyasi
   (kamida 32 slayd, "Mavzu kontentining tuzilmasi" bo'limidagi
   aynan shu ketma-ketlikda)

Aniq qoidalar:
- Mantiqiy operatorlardan keyin — 4 ta masala (3 Easy + 1 Hard)
- Ternary'dan keyin — 1 ta masala (Medium, nested ternary)
- If/Else'dan keyin — 4 ta masala (Easy → Hard)
- Loops'dan keyin — 3 ta masala (Easy → Medium)
- Oxirida — "10 ta murakkab vazifa" slayd (yechimsiz)
- Har bir masala yechimi YASHIRIN — tugma bilan ochiladi
- Kodlarda `<`, `>`, `&` — `&lt;`, `&gt;`, `&amp;` ga escape qilingan
- Har bir kod blokida `data-filename="..."` atributi
- Watermark — char-code encoding bilan obfuskatsiyalangan

Dizayn, ranglar, kod oynasi, pagination, watermark — barchasi
xuddi `PROMPT.md` da ko'rsatilgandek bo'lsin.
```

---

## ✅ Tekshirish ro'yxati (Checklist)

Yangi til uchun yaratilgan loyihada quyidagilar bo'lishi kerak:

### `index.html`
- [ ] Animatsiyali 3 ta blob fon
- [ ] Sticky nav + logo (gradient kvadrat ichida)
- [ ] Hero header — gradient sarlavha, badge, statistika
- [ ] Mavzular roadmap — card grid (status badge bilan)
- [ ] Hover effektlari (transform, glow, gradient border)
- [ ] Birinchi mavzu `target="_blank"` bilan ochiladi
- [ ] Watermark himoyasi (obfuskatsiyalangan inline JS)

### `{topic}.html`
- [ ] Title slide + dars maqsadi
- [ ] Mavzu kontentining tuzilmasi (8-bo'limga muvofiq)
- [ ] **Mantiqiy operatorlardan keyin 4 ta masala** (3E + 1H)
- [ ] **Ternary'dan keyin 1 ta masala** (M)
- [ ] **If/Else'dan keyin 4 ta masala** (E, E, M, H)
- [ ] **Loops'dan keyin 3 ta masala** (E, E, M)
- [ ] **10 ta amaliy topshiriq** slayd (yechimsiz)
- [ ] Yakun title slayd
- [ ] macOS-style kod oynasi (auto-wrap)
- [ ] Yechimni yashirish toggle (problem-header bo'lganida)
- [ ] Pagination (barcha slaydlar raqam bilan)
- [ ] Keyboard navigation (← → Space Home End)
- [ ] HTML escape (`&lt;`, `&gt;`, `&amp;`)
- [ ] Custom scrollbar (binafsha-pushti gradient)
- [ ] Watermark himoyasi (char-code, MutationObserver, hidden copies)
- [ ] Mobile responsive
- [ ] Print mode (PDF ga chop etish)
- [ ] Toast notification (Copy/Reset uchun)
- [ ] Console signature (F12 da chiqsin)
- [ ] Har bir kod blokida `data-filename`

### Kontent
- [ ] Barcha matnlar **o'zbek tilida** (lotin)
- [ ] Kod izohlari **o'zbekcha**
- [ ] Real misollar (Akobir, Tashkent, real raqamlar)
- [ ] Difficulty progression: oson → qiyin
- [ ] Edge case'lar misollarda qamrab olingan

---

**Muallif:** Rustamov Akobir
**Sana:** 2026
**Versiya:** 2.0 — Java Core dars yakunidan keyin yangilangan
