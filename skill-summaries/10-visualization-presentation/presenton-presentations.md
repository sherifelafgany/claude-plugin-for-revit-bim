# presenton — إنشاء عروض تقديمية وتصديرها PPTX/PDF/PNG

> **الغرض:** إنشاء عروض شرائح جديدة عبر Presenton وتصديرها كـ PPTX قابل للتحرير أو PDF أو صور PNG. **متى تُستخدم:** عند طلب عرض تقديمي أو PowerPoint أو slide/pitch/report deck أو تصدير ملف عرض، حتى دون ذكر Presenton — وليس لملفات PDF/صور عامة، أو مخططات نصية فقط، أو تعديل deck قائم.

## الفكرة الأساسية
يُكتب العرض كمستند HTML واحد كامل بـ Tailwind (كل شريحة 1280×720) في الذاكرة، ثم يُتحقق منه ويُصدَّر عبر أدوات Presenton MCP (endpoint: `https://api.presenton.ai/skills/mcp`)، وتُعاد روابط التحميل ورابط معاينة قابل للمشاركة. القاعدة الحاكمة: **صفر CSS مضمّن** — كل التنسيق بـ Tailwind classes.

## القواعد والأرقام الأساسية

**الأدوات:** `search_designs` · `search_icons` · `import_public_image` · `validate_presentation_html` · `export_html_presentation` · `create_presentation_preview` (قد تكون مسبوقة بـ namespace).
- فحص schema كل أداة وعدم تخمين حقول غير موثقة.
- لا استدعاء REST API مباشرة (Python، Bash، `curl`، WebFetch). إن غاب الـ connector: توضيح ضرورة نشر وربط `https://api.presenton.ai/skills/mcp`، ولا تبديل صامت لمحرك عروض آخر.
- لا إرسال بيانات اعتماد أو أسرار أو بيانات دفع أو مواد سرية؛ الصور الحساسة تحتاج تنقية أو موافقة صريحة على النقل الخارجي.
- كل `message` غير فارغة من أداة تُنقل للمستخدم **حرفياً** في التحديث التالي وفي `Notes` النهائية.

**قاعدة التنسيق غير القابلة للتفاوض:**
- Tailwind-first من البداية؛ **لا** `style` attributes ولا عناصر `<style>` في أي مكان (بما فيها SVG والقوالب وحاويات الرسوم).
- لا توليد أنماط بـ JavaScript (`.style`، `cssText`، `setAttribute("style", ...)`).
- قيم arbitrary عند الحاجة: `w-[320px]`، `bg-[#0f172a]`، `leading-[1.15]`، `shadow-[0_16px_40px_rgba(0,0,0,0.18)]`، `font-['DM_Sans']`، `left-[48px] top-[72px]`.
- صورة خلفية: `<img>` بـ `absolute inset-0 h-full w-full object-cover` لا CSS `background-image`.
- **تدقيق المصدر قبل التحقق** (case-insensitive، يجب صفر تطابقات): `\sstyle\s*=` · `<style(?:\s|>)` · `\.style\b` · `\bcssText\b` · `setAttribute\s*\(\s*['\"]style['\"]`.

**هيكل المستند:**
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>  <!-- عند وجود رسوم -->
  </head>
  <body class="m-0 p-0">
    <main id="presentation-slides-wrapper" class="m-0 w-[1280px] p-0">
      <section class="relative h-[720px] w-[1280px] overflow-hidden" data-speaker-note="Optional note">...</section>
    </main>
  </body>
</html>
```
- **wrapper واحد فقط** `#presentation-slides-wrapper`؛ كل ابن مباشر = شريحة (مجموعة متداخلة = شريحة واحدة).
- كل شريحة **1280×720 px (16:9)** بـ `h-[720px] w-[1280px] overflow-hidden`؛ لا هوامش أو فراغات بين الشرائح.

**الأصول والخطوط:**
- روابط HTTPS مطلقة فقط؛ لا `data:` ولا base64 ولا مسارات محلية/نسبية.
- كل صورة من المستخدم: `import_public_image` **مرة واحدة** وإعادة استخدام الرابط (API: PNG/JPEG/WebP، **حد 10 MB**).
- كل أيقونة: `search_icons` (أنواع `icon_type`: `bold`، `duotone`، `fill`، `light`، `regular`، `thin`؛ الافتراضي `bold`، `limit` افتراضي 20). لا SVG مرسوم أو emoji أو Unicode أو icon fonts كبديل؛ إن لم توجد نتيجة: تحسين البحث أو حذف الأيقونة.
- الخط المسمى في التصميم يُستخدم بالضبط مع `<link>` HTTPS في `<head>`؛ عدم توفره → إبلاغ المستخدم وأخذ موافقته على البديل.
- كل `<img>` له `alt` (معبّر للمحتوى، `alt=""` للزخرفي)، أبعاد صريحة و`object-fit` متعمد. نقص alt = تحذير لا خطأ.

**Chart.js:** لكل رسم بيانات؛ canvas بـ ID فريد و`width`/`height` ثابتة؛ `document.querySelector("#chart-unique-id")` + `new Chart(...)` واحد؛ `responsive: false` و`animation: false`؛ لا timers أو تفاعل. (قد تُلتقط كصورة وتفقد قابلية التحرير في PPTX.)

**توافق PPTX:** النص كعناصر HTML نصية؛ تفضيل fills صلبة، borders، تدرجات بسيطة، flex/grid، `<img>`، SVG بسيط؛ تجنب CSS filters، backdrop filters، masks، blend modes، فيديو، حركة.

**Speaker notes:** عند الطلب فقط، `data-speaker-note` واحد على كل شريحة.

**التصميم:** brief تصميم صريح من المستخدم (palette، typography، layout، brand...) = مصدر الحقيقة؛ لا `search_designs` ولا `design_id`. وإلا `search_designs` (API: `n` من 1 إلى 10) واختيار الأفضل (أو عرض خيارات مع العنوان والوصف والـ ID إن احتاج تفضيلاً بشرياً) وتمرير `design_id` في كل export. ترجمة التصميم إلى نظام صغير: ألوان الخلفية والأسطح، النص والتمييز بتباين مقروء، سلم الخطوط (عنوان/نص/أرقام)، وحدة المسافات وحدود المحتوى الآمنة، لغة الزوايا والحدود والظلال، معالجة الصور وألوان الرسوم. التخطيط يتنوع حسب المحتوى مع الاتساق؛ لا قوالب HTML مرجعية.

**التحقق والتصدير:**
- `validate_presentation_html`: `valid = true` عندما `errors` فارغة (التحذيرات استشارية). **حد 3 محاولات فاشلة** ثم إبلاغ الأخطاء المتبقية.
- `export_html_presentation` مرة واحدة لكل صيغة (`pptx`، `pdf`، `png`) من **نفس HTML**؛ بلا صيغة محددة → الثلاث. النجاح = `id` موجب + `url` HTTP/HTTPS.
- PNG = ملف **ZIP** بصورة لكل شريحة؛ PDF = صفحة 1280×720 لكل ابن مباشر.
- `create_presentation_preview` **مرة واحدة لكل عرض** بأي creation id. **الروابط تنتهي بعد 24 ساعة.**
- أخطاء النقل/الخادم العابرة: إعادة المحاولة **مرتين كحد أقصى**؛ لا تكرار export ناجح.
- رموز أخطاء API: `400` HTML/wrapper/format غير صالح · `401` token معاينة منتهٍ · `404` غير منشور/غير موجود · `422` schema · `502` المُصدِّر غير متاح.

## خطوات العمل (Workflow)
1. تحديد الصيغ، العنوان، الجمهور، الغرض، عدد الشرائح، المحتوى (افتراضات معقولة عند النقص؛ بلا صيغة → PPTX وPDF وPNG).
2. حسم الاتجاه البصري: brief المستخدم أو `search_designs` مع الاحتفاظ بالـ ID.
3. حسم الأصول قبل HTML: `import_public_image` لكل صورة مستخدم، `search_icons` لكل مفهوم أيقونة.
4. كتابة مستند HTML كامل Tailwind-first في الذاكرة (لا حفظ في الـ workspace) وفق `html-authoring-system-prompt.md` و`html-format.md`.
5. تدقيق المصدر بالأنماط الخمسة ثم `validate_presentation_html`؛ تصحيح وإعادة حتى 3 محاولات.
6. `export_html_presentation` لكل صيغة مطلوبة منفصلة (`design_id` فقط للتصميم المبحوث).
7. التقاط `id` موجب و`url` لكل تصدير.
8. `create_presentation_preview` مرة واحدة والتقاط الرابط.
9. جرد الخطوط من HTML المُتحقق منه (العائلات المعلنة وروابط stylesheet) — ينطبق على كل الصيغ.
10. إعادة النتيجة بالصيغة:
```text
Presentation
- Title: <title>
- Formats: <requested formats, or PPTX, PDF, and PNG>
- Slides: <slide count>
- Design/reference used: <user brief, or searched design title and id>
- References/assets used: <sources or "None">
- Notes: <every MCP message verbatim, then other details; or "None">

Shareable preview
- [View presentation](<preview_url>) — expires after 24 hours

Fonts
- <font name> — <source URL when applicable>

Download URLs
- PPTX: [Download](<url>)
- PDF: [Download](<url>)
- PNG: [Download](<url>)
```

## نقاط الفحص / QA Checklist
- [ ] صفر تطابقات للأنماط الخمسة الممنوعة في كامل المصدر (بما فيه SVG وJS).
- [ ] مستند كامل `<html>`/`<head>`/`<body>` مع Tailwind CDN.
- [ ] `#presentation-slides-wrapper` واحد بابن مباشر واحد على الأقل.
- [ ] 1280×720 مطبقة على كل شريحة؛ لا overflow أو قص أو scrollbars أو نص خارج الكانفس.
- [ ] لا روابط أصول محلية/نسبية ولا `data:`/base64.
- [ ] صور المستخدم من `import_public_image`؛ الأيقونات من `search_icons`.
- [ ] كل `<img>` له `alt`.
- [ ] كل خط في التصميم مستخدم بالضبط (أو بديل وافق عليه المستخدم) ومستورد في `<head>`.
- [ ] Chart.js CDN وcanvas IDs فريدة ثابتة الحجم وبلا حركة لكل رسم.
- [ ] نفس HTML النهائي لكل الصيغ؛ تصدير الصيغ المطلوبة فقط.
- [ ] الرد النهائي فيه التصميم/المراجع، التفاصيل، جرد الخطوط الدقيق، رابط تحميل لكل صيغة، رابط المعاينة.

## أخطاء ممنوعة / تحذيرات
- كتابة CSS ثم تحويله لاحقاً؛ أي inline style ولو مؤقتاً.
- استخدام المُحقق كأول كاشف للـ inline CSS.
- اختراع أيقونات أو مراجع أو مصادر خطوط أو IDs أو رسائل أو روابط.
- استبدال الخط المحدد بصمت.
- ادعاء النجاح دون `id` موجب و`url` لكل صيغة ورابط معاينة لكل عرض.
- رسم المخططات يدوياً بـ HTML/CSS/SVG بدل Chart.js.
- تنزيل الملفات المصدّرة محلياً أو الحفظ في الـ workspace.
- إيقاف التصدير بسبب تحذيرات alt.

## مخرجات متوقعة
- روابط تحميل لكل صيغة مطلوبة (PPTX، PDF، PNG-ZIP)، ورابط معاينة قابل للمشاركة (24 ساعة).
- عدد الشرائح، التصميم المختار (العنوان والـ ID) أو brief المستخدم، مصادر الأصول، جرد الخطوط، رسائل الأدوات حرفياً.

## ملفات مرجعية داخل الـ skill
- `references/mcp-contract.md` — العقد الدلالي للأدوات الست، التمييز بين `errors` و`warnings`، وmcp annotations (`search_designs`، `search_icons`، `validate_presentation_html` = readOnly؛ البقية تُنشئ artifacts بلا حذف).
- `references/html-authoring-system-prompt.md` — قيود التوليد كـ system prompt وتحويلات CSS → Tailwind.
- `references/html-format.md` — الهيكل، الأبعاد، Tailwind، الأصول والخطوط، Chart.js، توافق PPTX، speaker notes، تطبيق التصميم، preflight checklist.
- `references/api.md` — Presenton v3 REST (`/api/v3/designs/search`، `/api/v3/images/upload/public`، `/api/v3/icons/search`، `/api/v3/export/html-to-any`، `.../create-preview`، `.../preview`) للمرجعية فقط — الاستخدام عبر MCP لا REST.
- `agents/openai.yaml` — بيانات واجهة ("Presenton — PPTX, PDF & PNG"، `allow_implicit_invocation: true`).

## مهارات مرتبطة
*(استنتاجاً — المصدر لا يسرد مهارات مرتبطة صراحةً)*
pptx (لتعديل/قراءة ملفات .pptx قائمة)، architectural-visualization-director (لصور العرض المعمارية).
