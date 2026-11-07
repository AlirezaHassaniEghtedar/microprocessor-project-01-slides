---
theme: default
title: رقص نور حلقه‌ای و انباشت خطی LED با ESP32
titleTemplate: '%s'
info: |
  ## رقص نور حلقه‌ای (۸ LED) و انباشت خطی (۴ LED) با ESP32
  گزارش کار گروهی اول — درس ریزپردازنده — استاد: دکتر پورمند
class: text-[#CFE5D9]
colorSchema: dark
background: 'radial-gradient(ellipse at 50% -10%, #0E2A1F 0%, #061510 55%, #020806 100%)'
transition: fade
mdc: true
fonts:
  sans: 'Vazirmatn:400,500,700,800'
  serif: 'Vazirmatn:700,800'
  mono: 'JetBrains Mono'
---

<!-- ============ امضای بصری: «ریل LED» ============
یک ستون از نقطه‌های LED در حاشیهٔ راست (جهت RTL) که یک نور روی آن می‌دود؛
همان حرکتی که خود پروژه می‌سازد. استایل‌ها در style.css هستند (کنار همین فایل). -->

<div class="relative flex flex-col items-center justify-center h-full text-center px-20">

  <div class="mono text-xs tracking-[0.4em] text-[#39FF88] mb-6">MICROPROCESSOR · GROUP WORK 01</div>

  <h1 class="font-serif font-800 text-5xl leading-[1.5] text-[#F2FBF6]">
    رقص نور حلقه‌ای و انباشت خطی LED
  </h1>

  <div class="text-lg text-[#8FB3A2] mt-4">
    کنترل خروجی‌های دیجیتال با <span class="mono text-[#39FF88]">ESP32</span> و Arduino IDE
  </div>

  <div class="flex gap-3 my-9 ltr">
    <div v-for="i in 8" :key="i" class="led chase" :style="{ '--c': '#39FF88', width: '14px', height: '14px' }">
      <div class="base"></div>
      <div class="glow" :style="{ '--s': i - 1 }"></div>
    </div>
  </div>

  <div class="text-sm text-[#8FB3A2] leading-loose">
    علیرضا حسنی اقتدار &nbsp;·&nbsp; ارشیا جعفری &nbsp;·&nbsp; شیدا مردانلو<br>
    درس ریزپردازنده &nbsp;·&nbsp; استاد درس: دکتر پورمند
  </div>
</div>

<div @click="$slidev.nav.next" class="absolute bottom-8 left-14 mono text-xs text-[#39FF88] flex items-center gap-2 cursor-pointer opacity-70 hover:opacity-100">
  <carbon:arrow-left /> PRESS SPACE
</div>

<!--
سلام. ما علیرضا حسنی اقتدار، ارشیا جعفری و شیدا مردانلو هستیم و این گزارش کار گروهی اول درس ریزپردازنده، استاد دکتر پورمند است. در این کار با یک برد ESP32 دو الگوی نورپردازی ساختیم: رقص نور حلقه‌ای با هشت LED و انباشت خطی با چهار LED. نوری که پایین عنوان می‌دود همان الگوی اول است.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۱ — مقدمه</div>
<h1 class="ttl">صورت مسئله</h1>

<div class="grid grid-cols-2 gap-6">

<v-click>
<div class="card">
  <div class="mono text-xs text-[#39FF88] mb-1">EXPERIMENT 01</div>
  <div class="font-serif font-700 text-xl text-[#F2FBF6] mb-2">رقص نور حلقه‌ای</div>
  <div class="text-[#CFE5D9] leading-loose">
    <b class="text-[#39FF88]">۸ عدد LED</b> به‌شکل حلقه. در هر لحظه <b>یک یا دو LED</b> روشن باشند. مقدار تأخیر دلخواه است.
  </div>
</div>
</v-click>

<v-click>
<div class="card">
  <div class="mono text-xs text-[#39FF88] mb-1">EXPERIMENT 02</div>
  <div class="font-serif font-700 text-xl text-[#F2FBF6] mb-2">انباشت خطی</div>
  <div class="text-[#CFE5D9] leading-loose">
    <b class="text-[#39FF88]">۴ عدد LED</b> در یک خط. نورها یکی‌یکی به انتهای خط می‌رسند و روی هم <b>انباشته</b> می‌شوند. تأخیر دلخواه است.
  </div>
</div>
</v-click>

</div>

<v-click>
<div class="callout mt-8 max-w-3xl">
  <span class="text-sm text-[#39FF88]">هدف</span>
  <div class="text-[#F2FBF6] mt-1 leading-loose">آشنایی عملی با نحوهٔ کنترل دنیای بیرون توسط یک میکروکنترلر؛ با روشن و خاموش کردن برنامه‌ریزی‌شدهٔ چند LED از طریق پایه‌های ورودی/خروجی عمومی (GPIO).</div>
</div>
</v-click>

</div>

<!--
هدف این کار گروهی آشنایی عملی با کنترل دنیای بیرون توسط میکروکنترلر است. صورت تکلیف دو بخش داشت: یک، رقص نور حلقه‌ای با هشت LED که در هر لحظه یک یا دو LED روشن باشند. دو، انباشت خطی با چهار LED. مقدار تأخیر در هر دو دلخواه بود.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۲ — مبانی</div>
<h1 class="ttl">میکروکنترلر چطور LED را کنترل می‌کند؟</h1>

<div class="grid grid-cols-[1fr_1.05fr] gap-10 items-center">

<div class="text-lg leading-relaxed">

<v-clicks>

- پایهٔ **GPIO** را با `pinMode(pin, OUTPUT)` خروجی تعریف می‌کنیم.
- با `HIGH`، ولتاژ پایه حدود **۳٫۳ ولت** می‌شود؛ جریان از LED و مقاومت می‌گذرد و LED **روشن** می‌شود.
- با `LOW`، ولتاژ صفر است؛ جریانی برقرار نمی‌شود و LED **خاموش** می‌ماند.
- پس هر الگوی نوری یعنی: **روشن، مکث، خاموش** به ترتیبی مشخص.

</v-clicks>

</div>

<div class="card">
<svg viewBox="0 0 440 190" class="w-full ltr">
  <text x="54" y="30" fill="#39FF88" style="font-size:12px;text-anchor:middle;font-family:JetBrains Mono,monospace" class="hi">HIGH = 3.3 V</text>
  <text x="54" y="30" fill="#8FB3A2" style="font-size:12px;text-anchor:middle;font-family:JetBrains Mono,monospace" class="lo">LOW = 0 V</text>
  <rect x="6" y="45" width="96" height="100" rx="8" fill="#08201A" stroke="#14382A" stroke-width="1.5"/>
  <text x="54" y="92" fill="#CFE5D9" style="font-size:16px;text-anchor:middle;font-family:JetBrains Mono,monospace">ESP32</text>
  <text x="54" y="114" fill="#8FB3A2" style="font-size:11px;text-anchor:middle;font-family:JetBrains Mono,monospace">GPIO pin</text>
  <circle class="sglow" cx="170" cy="95" r="34" fill="#FF5C5C"/>
  <path class="wire" d="M102 95 H152 M188 95 H238 M298 95 H356"/>
  <path d="M152 75 V115 L188 95 Z" fill="#08201A" stroke="#FF5C5C" stroke-width="2"/>
  <path d="M188 75 V115" stroke="#FF5C5C" stroke-width="3"/>
  <text x="170" y="142" fill="#FF5C5C" style="font-size:12px;text-anchor:middle;font-family:JetBrains Mono,monospace">LED</text>
  <rect x="238" y="84" width="60" height="22" rx="3" fill="#08201A" stroke="#FFC24B" stroke-width="2"/>
  <text x="268" y="74" fill="#FFC24B" style="font-size:12px;text-anchor:middle;font-family:JetBrains Mono,monospace">220 Ω</text>
  <path d="M356 95 V140 M338 140 H374 M345 149 H367 M352 158 H360" fill="none" stroke="#8FB3A2" stroke-width="2"/>
  <text x="356" y="178" fill="#8FB3A2" style="font-size:12px;text-anchor:middle;font-family:JetBrains Mono,monospace">GND</text>
</svg>
</div>

</div>

</div>

<!--
هر LED مسیر مستقل خودش را دارد: پایهٔ خروجی برد به پایهٔ بلندتر LED که آند است وصل می‌شود، پایهٔ کوتاه‌تر که کاتد است از طریق یک مقاومت به زمین می‌رسد. وقتی برنامه روی پایه HIGH بگذارد حدود سه و سه دهم ولت روی پایه می‌افتد، جریان برقرار می‌شود و LED روشن می‌شود. با LOW جریانی نیست و LED خاموش است. در شماتیک سمت چپ این دو حالت را می‌بینید.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۲ — مبانی</div>
<h1 class="ttl">قطعات و ساختار مدار</h1>

<div class="grid grid-cols-2 gap-8">

<div>
<div class="text-sm text-[#8FB3A2] mb-2">قطعات استفاده‌شده</div>

<v-clicks>

- برد **ESP32**
- بردبورد و سیم جامپر (از جمله سیم مشکی برای GND)
- ۸ عدد **LED** (قرمز، زرد و سبز)
- مقاومت محدودکنندهٔ جریان برای هر LED
- کابل USB (دارای خطوط داده)
- نرم‌افزار Arduino IDE همراه با پکیج برد ESP32

</v-clicks>
</div>

<div class="space-y-5">

<v-click>
<div class="card">
  <div class="text-sm text-[#8FB3A2] mb-3">مسیر هر LED (۸ مسیر مستقل)</div>
  <div class="flex items-center gap-2 text-sm">
    <span class="card !py-1 !px-3 mono text-[#39FF88]">GPIO</span>
    <carbon:arrow-left class="text-[#5F8272]" />
    <span class="card !py-1 !px-3">آند LED</span>
    <carbon:arrow-left class="text-[#5F8272]" />
    <span class="card !py-1 !px-3">کاتد</span>
    <carbon:arrow-left class="text-[#5F8272]" />
    <span class="card !py-1 !px-3 text-[#FFC24B]">مقاومت</span>
    <carbon:arrow-left class="text-[#5F8272]" />
    <span class="card !py-1 !px-3 mono">GND</span>
  </div>
  <div class="text-xs text-[#8FB3A2] mt-3">همهٔ مسیرها به یک GND مشترک روی ریل بردبورد می‌رسند.</div>
</div>
</v-click>

<v-click>
<div class="card">
  <div class="text-sm text-[#FFC24B] mb-2">چرا مقاومت؟</div>
  <div class="mono text-[#F2FBF6] text-lg my-2">I = (3.3 − 2) / 220 ≈ 6 mA</div>
  <div class="text-sm text-[#CFE5D9] leading-loose">با افت ولتاژ حدود ۲ ولت روی LED، جریان حدود ۶ میلی‌آمپر می‌شود که برای LED و پایهٔ برد امن است. بدون مقاومت ممکن است LED یا پایه آسیب ببیند.</div>
</div>
</v-click>

</div>

</div>

</div>

<!--
قطعات: برد ESP32، بردبورد، هشت LED، مقاومت برای هر LED، سیم جامپر و کابل USB که حتماً خط داده داشته باشد. هر مسیر از پایهٔ GPIO به آند LED، از کاتد به مقاومت و از آنجا به GND می‌رود. مقاومت نقش محدود کردن جریان را دارد: با ولتاژ سه و سه دهم و افت حدود دو ولت روی LED، یک مقاومت دویست و بیست اهمی حدود شش میلی‌آمپر عبور می‌دهد که امن است.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۲ — مبانی</div>
<h1 class="ttl">ESP32 و نگاشت پایه‌ها</h1>

<div class="grid grid-cols-3 gap-4 mb-6">
  <div class="card text-center"><div class="mono text-[#39FF88] text-lg">Espressif</div><div class="text-xs text-[#8FB3A2] mt-1">سازندهٔ تراشه</div></div>
  <div class="card text-center"><div class="mono text-[#39FF88] text-lg">3.3 V</div><div class="text-xs text-[#8FB3A2] mt-1">منطق پایه‌ها</div></div>
  <div class="card text-center"><div class="mono text-[#39FF88] text-lg">Arduino IDE</div><div class="text-xs text-[#8FB3A2] mt-1">زبان C/C++</div></div>
</div>

<v-click>
<div class="ltr">
  <div class="grid grid-cols-8 gap-2">
    <div v-for="(g, i) in [16, 17, 18, 19, 21, 22, 23, 25]" :key="g" class="card text-center !py-3 !px-1" :class="i < 4 ? '!border-[#39FF88]/60' : ''">
      <div class="mono text-[11px] text-[#8FB3A2]">LED {{ i + 1 }}</div>
      <div class="mono text-xl text-[#39FF88]">{{ g }}</div>
      <div class="mono text-[10px] text-[#5F8272]">GPIO</div>
    </div>
  </div>
  <div class="flex mt-2">
    <div class="w-1/2 border-t-2 border-[#39FF88] pt-1 text-center text-xs text-[#39FF88]">Exp 2 — 4 pins</div>
    <div class="w-1/2 border-t-2 border-[#14382A] pt-1 text-center text-xs text-[#8FB3A2]">Exp 1 — 8 pins</div>
  </div>
</div>
</v-click>

<v-click>
<div class="callout warn mt-6 max-w-4xl">
  <span class="text-sm text-[#FFC24B]">چرا این پایه‌ها؟</span>
  <div class="text-[#CFE5D9] mt-1 text-sm leading-loose">پایه‌های <code>0, 2, 5, 12, 15</code> هنگام بوت نقش دارند، <code>6–11</code> به حافظهٔ فلش وصل‌اند و <code>34–39</code> فقط ورودی‌اند؛ پس پایه‌های آزادتر انتخاب کردیم.</div>
</div>
</v-click>

</div>

<!--
تراشهٔ ESP32 محصول شرکت Espressif است، منطق سه و سه دهم ولتی دارد و با Arduino IDE و زبان C و C++ برنامه‌ریزی می‌شود. در ESP32 باید شمارهٔ GPIO را مستقیم در کد بنویسیم. ما هشت پایه را برای آزمایش اول انتخاب کردیم و در آزمایش دوم فقط چهار پایهٔ اول را استفاده می‌کنیم. پایه‌هایی را انتخاب کردیم که در بوت نقش خاصی ندارند.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۳ — برنامه‌نویسی</div>
<h1 class="ttl">ساختار برنامهٔ Arduino</h1>

<div class="grid grid-cols-2 gap-6 mb-6">

<v-click>
<div class="card">
  <div class="mono text-[#39FF88] text-lg mb-1">setup()</div>
  <div class="text-[#CFE5D9] leading-loose">فقط <b>یک بار</b> در ابتدای کار اجرا می‌شود. همهٔ پایه‌های مورد استفاده را با <code>pinMode(pin, OUTPUT)</code> خروجی تعریف می‌کنیم.</div>
</div>
</v-click>

<v-click>
<div class="card">
  <div class="mono text-[#39FF88] text-lg mb-1">loop()</div>
  <div class="text-[#CFE5D9] leading-loose">بعد از <code>setup()</code> <b>بی‌نهایت تکرار</b> می‌شود و الگوی نوری در آن نوشته شده است.</div>
</div>
</v-click>

</div>

<div class="grid grid-cols-2 gap-x-8 text-[#CFE5D9]">

<v-clicks>

- <code>digitalWrite(pin, HIGH)</code> روشن کردن LED
- <code>digitalWrite(pin, LOW)</code> خاموش کردن LED
- <code>delay(d)</code> توقف برنامه به مدت d میلی‌ثانیه
- متغیر <code>d</code> مقدار تأخیر را نگه می‌دارد؛ برای تغییر سرعت فقط یک جا ویرایش می‌شود

</v-clicks>

</div>

<v-click>
<div class="callout mt-4 max-w-3xl">
  <span class="text-sm text-[#39FF88]">نکتهٔ کلیدی</span>
  <div class="text-[#F2FBF6] mt-1 leading-loose">پایه‌ای که <code>HIGH</code> شده تا زمانی که <code>LOW</code> نشود روشن می‌ماند. برای «ماندن» LED کافی است آن را خاموش نکنیم.</div>
</div>
</v-click>

</div>

<!--
هر برنامهٔ Arduino دو تابع اصلی دارد. setup فقط یک بار در شروع اجرا می‌شود و ما در آن پایه‌ها را خروجی تعریف می‌کنیم. loop بعد از آن بی‌نهایت تکرار می‌شود و الگوی نوری داخل آن است. سه دستور اصلی: digitalWrite با HIGH برای روشن، با LOW برای خاموش و delay برای مکث. نکتهٔ کلیدی این است که پایه تا وقتی LOW نشود روشن می‌ماند؛ همین، پایهٔ آزمایش دوم یعنی «ماندن» نورها را ممکن می‌کند.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۴ — آزمایش اول</div>
<h1 class="ttl !mb-4">کد رقص نور (نمونهٔ فشرده)</h1>

```cpp {1-2|4-6|9-14|15-23|all}
const int leds[8] = {16, 17, 18, 19, 21, 22, 23, 25};
const int d = 200;                       // delay per step (ms)

void setup() {
  for (int i = 0; i < 8; i++) pinMode(leds[i], OUTPUT);
}

void loop() {
  // phase 1: one LED at a time
  for (int i = 0; i < 8; i++) {
    digitalWrite(leds[i], HIGH);
    delay(d);
    digitalWrite(leds[i], LOW);
  }
  // phase 2: two adjacent LEDs, last pair closes the ring
  for (int i = 0; i < 8; i++) {
    int j = (i + 1) % 8;
    digitalWrite(leds[i], HIGH);
    digitalWrite(leds[j], HIGH);
    delay(d);
    digitalWrite(leds[i], LOW);
    digitalWrite(leds[j], LOW);
  }
}
```

<div class="text-xs text-[#5F8272] mt-2">در کد اصلی هر دستور خط‌به‌خط نوشته شده؛ این نسخه با آرایه و for همان رفتار را کوتاه‌تر می‌نویسد.</div>

</div>

<!--
اینجا ساختار کد آزمایش اول را می‌بینید. آرایهٔ leds شمارهٔ پایه‌ها را نگه می‌دارد و d تأخیر هر گام است. در setup پایه‌ها خروجی می‌شوند. در loop فاز اول هر LED را روشن می‌کند، d میلی‌ثانیه نگه می‌دارد و خاموش می‌کند. فاز دوم دو LED مجاور را با هم روشن و خاموش می‌کند. ما در کد اصلی همهٔ دستورها را خط‌به‌خط نوشتیم؛ این نسخه همان رفتار را با for کوتاه‌تر نشان می‌دهد.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۴ — آزمایش اول</div>
<h1 class="ttl !mb-6">رقص نور حلقه‌ای با ۸ LED</h1>

<div class="grid grid-cols-[1fr_auto] gap-10 items-center">

<div class="max-w-xl">

<v-click>
<div class="callout mb-4">
  <div class="font-serif font-700 text-[#F2FBF6]">فاز ۱ — یک LED در هر لحظه</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">LED 1 تا LED 8 به‌ترتیب روشن می‌شوند، ۲۰۰ms می‌مانند و خاموش می‌شوند؛ یک نقطهٔ نور روی حلقه می‌چرخد.</div>
</div>
</v-click>

<v-click>
<div class="callout mb-6">
  <div class="font-serif font-700 text-[#F2FBF6]">فاز ۲ — دو LED مجاور</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">هر بار دو LED کنار هم هم‌زمان روشن و پس از ۲۰۰ms هم‌زمان خاموش می‌شوند.</div>
</div>
</v-click>

<v-click>
<div class="grid grid-cols-3 gap-3">
  <div class="card text-center !p-3"><div class="mono text-2xl text-[#39FF88]">200ms</div><div class="text-xs text-[#8FB3A2] mt-1">تأخیر هر گام</div></div>
  <div class="card text-center !p-3"><div class="mono text-2xl text-[#39FF88]">16</div><div class="text-xs text-[#8FB3A2] mt-1">گام در هر دور</div></div>
  <div class="card text-center !p-3"><div class="mono text-2xl text-[#39FF88]">3.2s</div><div class="text-xs text-[#8FB3A2] mt-1">یک دور کامل</div></div>
</div>
</v-click>

</div>

<div class="relative ltr" style="width: 300px; height: 300px">
  <div class="absolute rounded-full border border-dashed border-[#14382A]" style="width: 210px; height: 210px; left: 45px; top: 45px"></div>

  <div v-for="i in 8" :key="i" class="led lring"
    :style="{
      position: 'absolute',
      left: (150 + 105 * Math.sin((i - 1) * Math.PI / 4) - 14) + 'px',
      top: (150 - 105 * Math.cos((i - 1) * Math.PI / 4) - 14) + 'px',
      '--c': ['#FF5C5C', '#FFC24B', '#39FF88'][(i - 1) % 3]
    }">
    <div class="base"></div>
    <div v-for="s in [i - 1, 8 + (i - 1), 8 + ((i + 6) % 8)]" :key="s" class="glow" :style="{ '--s': s }"></div>
  </div>

  <div v-for="i in 8" :key="'t' + i" class="absolute mono text-[11px] text-[#5F8272]"
    :style="{
      left: (150 + 138 * Math.sin((i - 1) * Math.PI / 4)) + 'px',
      top: (150 - 138 * Math.cos((i - 1) * Math.PI / 4)) + 'px',
      transform: 'translate(-50%, -50%)'
    }">{{ i }}</div>

  <div class="absolute inset-0 flex flex-col items-center justify-center gap-1 text-xs">
    <span class="ph1 text-[#39FF88]">فاز ۱ · تک LED</span>
    <span class="ph2 text-[#FFC24B]">فاز ۲ · جفت LED</span>
  </div>
</div>

</div>

</div>

<!--
در آزمایش اول هشت LED به هشت پایهٔ برد وصل است و d برابر دویست میلی‌ثانیه. فاز اول: هر LED روشن می‌شود، دویست میلی‌ثانیه می‌ماند و خاموش می‌شود؛ نتیجه یک نقطهٔ نور است که دور حلقه می‌چرخد. فاز دوم: دو LED مجاور با هم روشن می‌شوند. روی هم هشت گام فاز اول و هشت گام فاز دوم داریم، یعنی شانزده گام؛ با دویست میلی‌ثانیه برای هر گام یک دور حدود سه و دو دهم ثانیه طول می‌کشد. انیمیشن سمت چپ همین الگو را زنده نشان می‌دهد.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۴ — آزمایش اول</div>
<h1 class="ttl">فاز ۲ — چطور حلقه بسته می‌شود؟</h1>

<div class="text-[#8FB3A2] text-sm mb-3">جفت‌های روشن، به ترتیب (شمارهٔ LED)</div>

<div class="ltr flex flex-wrap gap-3 mb-8 max-w-3xl">
  <v-click v-for="i in 8" :key="i" :at="i">
    <div class="card text-center !py-2 !px-4" :class="i === 8 ? '!border-[#FFC24B] !bg-[#FFC24B]/10' : ''">
      <div class="mono text-lg" :class="i === 8 ? 'text-[#FFC24B]' : 'text-[#39FF88]'">{{ i }} + {{ i === 8 ? 1 : i + 1 }}</div>
    </div>
  </v-click>
</div>

<v-click at="9">
<div class="grid grid-cols-2 gap-6 max-w-4xl">
  <div class="callout warn">
    <div class="text-sm text-[#FFC24B]">جفت آخر</div>
    <div class="text-[#CFE5D9] mt-1 leading-loose">جفت <span class="mono">(8, 1)</span> است که LED هشتم را کنار LED اول می‌گذارد؛ پس حلقه‌ای <b>واقعی</b> داریم.</div>
  </div>
  <div class="card">
    <div class="text-sm text-[#39FF88]">ایدهٔ برنامه</div>
    <div class="mono text-[#F2FBF6] text-lg my-2">j = (i + 1) % 8</div>
    <div class="text-xs text-[#8FB3A2]">باقی‌مانده، شمارهٔ بعد از آخر را به اول برمی‌گرداند.</div>
  </div>
</div>
</v-click>

</div>

<!--
در فاز دوم جفت‌ها این‌طور می‌آیند: یک و دو، دو و سه، و همین‌طور تا هفت و هشت. جفت آخر هشت و یک است که حلقه را می‌بندد و LED هشتم را کنار LED اول قرار می‌دهد. در برنامه این کار با باقی‌ماندهٔ تقسیم بر هشت انجام می‌شود. بعد از فاز دوم loop تمام می‌شود و خودش از ابتدا شروع می‌کند.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۵ — آزمایش دوم</div>
<h1 class="ttl !mb-6">انباشت خطی با ۴ LED</h1>

<div class="grid grid-cols-[1fr_1.1fr] gap-10 items-center">

<div>

<div class="text-[#CFE5D9] leading-loose mb-4">یک نور از ابتدای خط تا انتهای آن می‌رود و <b>در انتها می‌ماند</b>؛ نور بعدی یک خانه زودتر می‌ایستد، و همین‌طور ادامه تا همهٔ LEDها روشن شوند. شبیه <b>چیدن آجر</b> روی هم.</div>

<v-clicks>

- دور ۱: توقف در LED 4
- دور ۲: توقف در LED 3
- دور ۳: توقف در LED 2
- دور ۴: توقف در LED 1
- چشمک پایانی: همه خاموش، روشن، خاموش

</v-clicks>

</div>

<div class="card">
  <div class="ltr flex justify-center gap-8 py-8">
    <div v-for="(slots, k) in [[0, 4, 7, 9, 11], [1, 5, 8, 9, 11], [2, 6, 7, 8, 9, 11], [3, 4, 5, 6, 7, 8, 9, 11]]" :key="k" class="flex flex-col items-center gap-3">
      <div class="led bline" :style="{ '--c': ['#FF5C5C', '#FFC24B', '#39FF88', '#FF5C5C'][k], width: '44px', height: '44px' }">
        <div class="base"></div>
        <div v-for="s in slots" :key="s" class="glow" :style="{ '--s': s }"></div>
      </div>
      <div class="mono text-xs text-[#8FB3A2]">LED {{ k + 1 }}</div>
    </div>
  </div>
  <div class="text-center text-xs text-[#8FB3A2]">تأخیر هر گام ۳۰۰ میلی‌ثانیه</div>
</div>

</div>

</div>

<!--
در آزمایش دوم چهار LED روی چهار پایهٔ اول داریم و d برابر سیصد میلی‌ثانیه است. یک نور از LED اول شروع می‌کند، یکی‌یکی جلو می‌رود و در LED چهارم می‌ماند. دور بعد دوباره از اول می‌رود ولی این بار در LED سوم می‌ایستد، بعد دوم و در نهایت اول. به این ترتیب مثل چیدن آجر روی هم نورها انباشته می‌شوند. وقتی هر چهار LED روشن شدند یک چشمک پایانی می‌زنیم و الگو از اول تکرار می‌شود.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۵ — آزمایش دوم</div>
<h1 class="ttl !mb-5">نمودار زمانی هر چرخه (۱۳ گام)</h1>

<div class="ltr flex gap-3 items-stretch mb-6">
<div class="flex flex-col font-mono text-xs text-[#8FB3A2]" style="width: 48px">
<div style="height: 24px"></div>
<div v-for="k in 4" :key="k" class="flex items-center" style="height: 36px">LED {{ k }}</div>
</div>
<div class="relative flex-1">
<div class="tcursor"></div>
<div class="flex">
<div v-for="s in 13" :key="s" class="mono text-[11px] text-center text-[#5F8272]" style="flex: 1 1 0; height: 20px; margin: 2px">{{ s }}</div>
</div>
<div v-for="(slots, k) in [[0, 4, 7, 9, 11], [1, 5, 8, 9, 11], [2, 6, 7, 8, 9, 11], [3, 4, 5, 6, 7, 8, 9, 11]]" :key="k" class="flex">
<div v-for="s in 13" :key="s" class="rounded-sm" :class="slots.includes(s - 1) ? 'bg-[#39FF88]' : 'bg-[#0C2A21]'" style="flex: 1 1 0; height: 32px; margin: 2px"></div>
</div>
</div>
</div>

<div class="grid grid-cols-3 gap-4 max-w-4xl">

<v-click>
<div class="card !p-3"><div class="text-[#39FF88] text-sm mb-1">گام‌های ۱ تا ۱۰</div><div class="text-xs text-[#CFE5D9] leading-loose">انباشت: ۴ + ۳ + ۲ + ۱ گام برای چهار دور</div></div>
</v-click>

<v-click>
<div class="card !p-3"><div class="text-[#FFC24B] text-sm mb-1">گام‌های ۱۱ تا ۱۳</div><div class="text-xs text-[#CFE5D9] leading-loose">چشمک پایانی: همه خاموش، همه روشن، همه خاموش</div></div>
</v-click>

<v-click>
<div class="card !p-3"><div class="mono text-[#39FF88] text-sm mb-1">13 × 300ms ≈ 3.9s</div><div class="text-xs text-[#CFE5D9] leading-loose">طول هر چرخه، سپس دوباره از ابتدا</div></div>
</v-click>

</div>

</div>

<!--
این نمودار وضعیت هر LED را در هر گام نشان می‌دهد؛ مربع سبز یعنی روشن. چهار گام اول نور اول تا LED چهارم می‌رود و می‌ماند. سه گام بعدی نور تا LED سوم، دو گام بعدی تا LED دوم و یک گام آخر LED اول روشن می‌شود و می‌ماند: روی هم ده گام. بعد سه گام چشمک پایانی: همه خاموش، همه روشن، همه خاموش. در مجموع سیزده گام و حدود سه و نه دهم ثانیه برای هر چرخه. چند دستور LOW هم ابتدای loop هست تا مطمئن شویم از حالت خاموش شروع می‌کنیم.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۵ — آزمایش دوم</div>
<h1 class="ttl !mb-4">منطق کد انباشت</h1>

```cpp {1-2|4-9|11-14|all}
// round r: walk one light from LED 1 to LED (4 - r) and leave it ON
for (int r = 0; r < 4; r++) {
  for (int i = 0; i < 4 - r; i++) {
    digitalWrite(leds[i], HIGH);
    delay(d);
    if (i < 3 - r) digitalWrite(leds[i], LOW);   // the last one stays on
  }
}
// final blink: all off, all on, all off
allWrite(LOW);  delay(d);
allWrite(HIGH); delay(d);
allWrite(LOW);  delay(d);
```

<div class="text-xs text-[#5F8272] mt-2">allWrite() تابع کمکی ساده‌ای است که همهٔ ۴ پایه را با یک مقدار می‌نویسد.</div>

</div>

<!--
هستهٔ منطق انباشت: دور اول نور تا LED چهارم می‌رود، دور دوم تا سوم و همین‌طور. شرط داخل حلقه می‌گوید همهٔ LEDها بعد از مکث خاموش شوند به جز آخرین LED هر دور که روشن می‌ماند. بعد چشمک پایانی را اجرا می‌کنیم.
-->

---
transition: fade
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec">بخش ۶ — اجرا روی برد</div>
<h1 class="ttl">بارگذاری برنامه روی ESP32</h1>

<div class="space-y-3 max-w-3xl">

<v-clicks>

<div class="flex gap-4 items-center card !py-3"><span class="mono text-[#39FF88] text-xl w-8">1</span><span>نصب <b>Arduino IDE</b></span></div>
<div class="flex gap-4 items-center card !py-3"><span class="mono text-[#39FF88] text-xl w-8">2</span><span>افزودن پشتیبانی ESP32: نصب بستهٔ <code>esp32</code> (ساختهٔ Espressif) از <span class="mono">Tools › Board › Boards Manager</span></span></div>
<div class="flex gap-4 items-center card !py-3"><span class="mono text-[#39FF88] text-xl w-8">3</span><span>انتخاب برد از منوی <span class="mono">Tools › Board</span> (مثلاً <code>ESP32 Dev Module</code>)</span></div>
<div class="flex gap-4 items-center card !py-3"><span class="mono text-[#39FF88] text-xl w-8">4</span><span>انتخاب پورت درست از <span class="mono">Tools › Port</span> (در صورت نیاز نصب درایور USB)؛ کابل باید خط داده داشته باشد</span></div>
<div class="flex gap-4 items-center card !py-3"><span class="mono text-[#39FF88] text-xl w-8">5</span><span>زدن <b>Upload</b>؛ برنامه در حافظهٔ برد می‌ماند و با وصل بودن برق مدام اجرا می‌شود</span></div>

</v-clicks>

</div>

</div>

<!--
مراحل بارگذاری: Arduino IDE را نصب می‌کنیم، بستهٔ پشتیبانی ESP32 را از Boards Manager اضافه می‌کنیم، برد را از منوی Tools انتخاب می‌کنیم، پورت درست را برمی‌داریم و در صورت نیاز درایور USB نصب می‌کنیم؛ دقت کنید کابل USB باید قابلیت انتقال داده داشته باشد. بعد Upload را می‌زنیم و برنامه روی حافظهٔ برد می‌ماند و با وصل بودن برق مداوم اجرا می‌شود.
-->

---
transition: slide-up
---

<div class="rail"></div>

<div class="h-full flex flex-col justify-center pr-28 pl-16">

<div class="sec warn">بخش ۷ — نکات فنی</div>
<h1 class="ttl">مشکلات احتمالی و محدودیت‌ها</h1>

<div class="grid grid-cols-2 gap-4 max-w-4xl">

<v-click>
<div class="callout warn">
  <div class="font-serif font-700 text-[#F2FBF6]">پایه‌های ویژه هنگام بوت</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">در ESP32 پایه‌های <code>0, 2, 5, 12, 15</code> حالت بوت را تعیین می‌کنند. اگر موقع Upload مشکل پیش آمد، LED روی پایهٔ مشکوک را موقتاً جدا کنید.</div>
</div>
</v-click>

<v-click>
<div class="callout warn">
  <div class="font-serif font-700 text-[#F2FBF6]">جریان محدود پایه‌ها</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">هر پایه جریان محدودی می‌دهد؛ استفاده از مقاومت برای هر LED ضروری است.</div>
</div>
</v-click>

<v-click>
<div class="callout warn">
  <div class="font-serif font-700 text-[#F2FBF6]">delay() برنامه را متوقف می‌کند</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">در مدت مکث، کار دیگری (مثل خواندن دکمه) ممکن نیست. برای این پروژه مشکلی ندارد.</div>
</div>
</v-click>

<v-click>
<div class="callout warn">
  <div class="font-serif font-700 text-[#F2FBF6]">خوانایی در برابر کوتاهی</div>
  <div class="text-sm text-[#CFE5D9] mt-1 leading-loose">کد را خط‌به‌خط نوشتیم تا ساده باشد؛ با آرایه‌ای از پایه‌ها و حلقهٔ for بسیار کوتاه‌تر می‌شد.</div>
</div>
</v-click>

</div>

</div>

<!--
چند نکتهٔ فنی. یک: بعضی پایه‌ها موقع روشن شدن یا ریست نقش خاص دارند و در انتخاب حالت بوت دخیل‌اند؛ اگر هنگام آپلود مشکلی پیش آمد، جدا کردن موقت LED آن پایه کمک می‌کند. دو: هر پایه جریان محدودی می‌دهد و مقاومت ضروری است. سه: دستور delay برنامه را کاملاً متوقف می‌کند و در این بازه کار دیگری ممکن نیست؛ برای این پروژه مشکلی ایجاد نمی‌کند. چهار: کد را برای خوانایی خط‌به‌خط نوشتیم و می‌شد با آرایه و حلقهٔ for کوتاه‌ترش کرد.
-->

---
layout: center
transition: fade
---

<div class="max-w-3xl mx-auto">

<div class="sec">بخش ۸ — نتیجه‌گیری</div>
<h1 class="ttl">نتیجه‌گیری</h1>

<div class="text-lg">

<v-clicks>

- هر دو الگو را روی **ESP32** با موفقیت پیاده کردیم: رقص نور حلقه‌ای با ۸ LED و انباشت خطی با ۴ LED.
- هر دو الگو فقط با **`digitalWrite`** و **`delay`** روی خروجی‌های دیجیتال ساخته شد.
- میکروکنترلر با تغییر ولتاژ پایه‌ها در **زمان‌ها و ترتیب مشخص** دنیای فیزیکی را کنترل می‌کند.
- با کنترل **ترتیب و زمان‌بندی** دستورها می‌توان الگوهای نوری متنوعی ساخت.

</v-clicks>

</div>

</div>

<!--
جمع‌بندی: هر دو الگوی خواسته‌شده را روی ESP32 پیاده کردیم. یاد گرفتیم که میکروکنترلر با تغییر ولتاژ پایه‌های خروجی، در زمان‌ها و ترتیب مشخص، می‌تواند دنیای فیزیکی را کنترل کند و فقط با کنترل ترتیب و زمان‌بندی دستورها الگوهای نوری متنوعی می‌شود ساخت.
-->

---
layout: center
class: text-center
---

<div class="absolute inset-0 flex items-center justify-center pointer-events-none">
  <div class="relative" style="width: 820px; height: 380px">
    <div class="absolute top-0 left-0 w-14 h-14 border-t-2 border-l-2 border-[#39FF88]/60"></div>
    <div class="absolute top-0 right-0 w-14 h-14 border-t-2 border-r-2 border-[#39FF88]/60"></div>
    <div class="absolute bottom-0 left-0 w-14 h-14 border-b-2 border-l-2 border-[#39FF88]/60"></div>
    <div class="absolute bottom-0 right-0 w-14 h-14 border-b-2 border-r-2 border-[#39FF88]/60"></div>
  </div>
</div>

<div class="relative text-center">
  <h1 class="font-serif font-800 text-5xl text-[#F2FBF6] leading-[1.5]">ممنون از توجه شما</h1>
  <div class="text-[#8FB3A2] mt-4">پرسش و پیشنهاد شما را می‌شنویم</div>

  <div class="flex justify-center gap-3 my-8 ltr">
    <div v-for="i in 8" :key="i" class="led chase" :style="{ '--c': '#39FF88', width: '14px', height: '14px' }">
      <div class="base"></div>
      <div class="glow" :style="{ '--s': i - 1 }"></div>
    </div>
  </div>

  <div class="text-sm text-[#8FB3A2] leading-loose">
    علیرضا حسنی اقتدار &nbsp;·&nbsp; ارشیا جعفری &nbsp;·&nbsp; شیدا مردانلو<br>
    درس ریزپردازنده &nbsp;·&nbsp; استاد: دکتر پورمند
  </div>
</div>

<style>
/* استایل‌های سراسری اسلایدها (با :global تا روی همهٔ اسلایدها اعمال شود) */
:global(:root) { --g: #39ff88; --amber: #ffc24b; --red: #ff5c5c; --ink: #f2fbf6; --text: #cfe5d9; --muted: #8fb3a2; --faint: #5f8272; --line: #14382a; --panel: #08201a; }
:global(.slidev-layout) { direction: rtl; text-align: right; color: var(--text); background: radial-gradient(ellipse at 50% -10%, #0e2a1f 0%, #061510 55%, #020806 100%); }
:global(.slidev-layout pre),
:global(.slidev-layout .slidev-code),
:global(.slidev-layout .slidev-code-wrapper),
:global(.ltr) { direction: ltr !important; text-align: left !important; unicode-bidi: isolate; }
:global(.slidev-layout :not(pre) > code) { direction: ltr; unicode-bidi: isolate; color: var(--g); background: rgba(57, 255, 136, 0.08); padding: 0.05em 0.4em; border-radius: 4px; font-size: 0.85em; white-space: nowrap; }
:global(.slidev-layout strong) { color: var(--ink); font-weight: 700; }
:global(.slidev-layout ul) { list-style: none; padding: 0; margin: 0; }
:global(.slidev-layout li) { position: relative; padding-inline-start: 1.5rem; margin: 0.7rem 0; }
:global(.slidev-layout li::marker) { content: ''; }
:global(.slidev-layout li::before) { content: ''; position: absolute; inset-inline-start: 0; top: 0.75em; width: 7px; height: 7px; background: var(--g); transform: rotate(45deg); }
:global(.sec) { font-size: 0.95rem; font-weight: 500; color: var(--g); margin-bottom: 0.4rem; }
:global(.sec::before) { content: '▍'; margin-inline-end: 0.5rem; }
:global(.sec.warn) { color: var(--amber); }
:global(.slidev-layout h1.ttl) { font-size: 2.3rem; font-weight: 800; line-height: 1.4; color: var(--ink); margin: 0 0 1.6rem 0; padding: 0; border: 0; opacity: 1; }
:global(.mono) { font-family: 'JetBrains Mono', monospace; direction: ltr; unicode-bidi: isolate; }
:global(.card) { border: 1px solid var(--line); background: var(--panel); padding: 1rem 1.25rem; border-radius: 6px; }
:global(.callout) { border-inline-start: 3px solid var(--g); background: rgba(57, 255, 136, 0.06); padding: 0.7rem 1.2rem; }
:global(.callout.warn) { border-color: var(--amber); background: rgba(255, 194, 75, 0.06); }
:global(.rail) { position: absolute; right: 0; top: 0; bottom: 0; width: 56px; overflow: hidden; pointer-events: none; opacity: 0.75; }
:global(.rail::before) { content: ''; position: absolute; inset: 0; background-image: radial-gradient(circle at 28px 20px, rgba(57, 255, 136, 0.25) 0, rgba(57, 255, 136, 0.25) 3px, transparent 4px); background-size: 56px 40px; }
:global(.rail::after) { content: ''; position: absolute; right: 0; top: 0; width: 56px; height: 40px; background: radial-gradient(circle at 28px 20px, #39ff88 0, #39ff88 3px, rgba(57, 255, 136, 0.35) 4px, transparent 13px); animation: railchase 4s steps(14) infinite; }
@keyframes -global-railchase { from { transform: translateY(0); } to { transform: translateY(560px); } }
:global(.led) { position: relative; width: 28px; height: 28px; }
:global(.led .base) { position: absolute; inset: 0; border-radius: 50%; border: 2px solid var(--c); background: rgba(255, 255, 255, 0.03); opacity: 0.4; }
:global(.led .glow) { position: absolute; inset: 0; border-radius: 50%; background: var(--c); box-shadow: 0 0 16px 5px var(--c); opacity: 0; }
:global(.lring .glow) { animation: slot16 3.2s linear infinite; animation-delay: calc(var(--s) * 0.2s); }
:global(.chase .glow) { animation: slot8 1.6s linear infinite; animation-delay: calc(var(--s) * 0.2s); box-shadow: 0 0 10px 3px var(--c); }
:global(.bline .glow) { animation: slot13 3.9s linear infinite; animation-delay: calc(var(--s) * 0.3s); box-shadow: 0 0 22px 7px var(--c); }
@keyframes -global-slot16 { 0%, 6.25% { opacity: 1; } 6.26%, 100% { opacity: 0; } }
@keyframes -global-slot8 { 0%, 12.5% { opacity: 1; } 12.6%, 100% { opacity: 0; } }
@keyframes -global-slot13 { 0%, 7.69% { opacity: 1; } 7.7%, 100% { opacity: 0; } }
:global(.ph1) { animation: ph1 3.2s steps(1, end) infinite; }
:global(.ph2) { animation: ph2 3.2s steps(1, end) infinite; }
@keyframes -global-ph1 { 0% { opacity: 1; } 50% { opacity: 0.25; } 100% { opacity: 0.25; } }
@keyframes -global-ph2 { 0% { opacity: 0.25; } 50% { opacity: 1; } 100% { opacity: 1; } }
:global(.wire) { fill: none; stroke: #39ff88; stroke-width: 2.5; stroke-dasharray: 6 5; animation: flow 0.6s linear infinite, dim 2s step-end infinite; }
@keyframes -global-flow { to { stroke-dashoffset: -11; } }
@keyframes -global-dim { 0% { opacity: 1; } 50% { opacity: 0.12; } 100% { opacity: 0.12; } }
:global(.sglow) { filter: blur(10px); opacity: 0; animation: vis 2s step-end infinite; }
:global(.hi) { animation: vis 2s step-end infinite; }
:global(.lo) { animation: invis 2s step-end infinite; }
@keyframes -global-vis { 0% { opacity: 1; } 50% { opacity: 0; } 100% { opacity: 0; } }
@keyframes -global-invis { 0% { opacity: 0; } 50% { opacity: 1; } 100% { opacity: 1; } }
:global(.tcursor) { position: absolute; top: 0; bottom: 0; left: 0; width: calc(100% / 13); border: 1.5px solid var(--amber); border-radius: 4px; background: rgba(255, 194, 75, 0.08); animation: tcur 3.9s steps(13, end) infinite; pointer-events: none; }
@keyframes -global-tcur { from { transform: translateX(0); } to { transform: translateX(1300%); } }
</style>