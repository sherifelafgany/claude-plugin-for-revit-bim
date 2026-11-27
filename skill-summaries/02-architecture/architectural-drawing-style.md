# architectural-drawing-style — أسلوب الإخراج المعماري المعتمد (Albarak)

> **الغرض:** توحيد مظهر كل مسقط وقطاع وواجهة وتفصيلة بأسلوب البريزنتيشن الذي اعتمده Sherif في مجموعة Albarak louver detail (2026-11-12). **متى تُستخدم:** كلما رُسم أو أُعدّ أي plan/section/elevation/façade أو construction detail في مشاريع Sherif، سواء من Revit أو كشيت SVG/PDF.

## الفكرة الأساسية
خلفية بيضاء، خطوط سوداء/رمادية رفيعة، مواد بدرجات greige باهتة؛ خط قوي للمقطوع، أخف للمرئي، وأخف (≈ 45% opacity) لما هو أبعد. المواد تُعرف بالدرجة والـ hatch الدقيق لا بالألوان المشبعة. ظل ضبابي ناعم في الواجهات، silhouettes بشرية سوداء وأشجار بخط واحد للمقياس، وكل تعليق بـ leader قائم الزاوية. الأصل: مرجع برازيلي (VISTA 04 / CORTE AA) — تقني، خفيف، بلا مؤثرات. النطاق: plans, sections, elevations, façade/construction details, environmental/sightline diagrams. شيتات الرخص تتبع `riyadh-permit-drawings` في المحتوى، والداخلي/3D يتبع `interior-3d-presentation-style`؛ هذه المهارة تحكم الشكل فقط.

## القواعد والأرقام الأساسية
**الألوان (Hex / RGB)**
| الاستخدام | Hex | RGB |
|---|---|---|
| Ink (خطوط، نص، silhouettes) | #1d1d1b | 29,29,27 |
| Secondary text | #555555 | 85,85,85 |
| Light lines (beyond, ext. lines) | #8a8a86 | 138,138,134 |
| Fin/timber/sand cladding (elevation) | #d8cdbd | 216,205,189 |
| — oblique / darker face | #c7bba9 / #b3a692 | |
| — cut fill (+ fine diagonal hatch) | #c5b6a1 | 197,182,161 |
| — outline of cladding | #857a6a | 133,122,106 |
| Slab/RC cut fill (+hatch) | #d9d9d6 | 217,217,214 |
| Beams/bands in elevation | #e5e4e0 | 229,228,224 |
| Beam cut fill | #d8d7d2 | 216,215,210 |
| Glass | #dbe2e5 (light #edf1f2) | 219,226,229 |
| Glass line plan/section | #5c7a88 | 92,122,136 |
| Walls seen | #f5f4f1 | 245,244,241 |
| Wall poché in plan | #3a3a38 | 58,58,56 |
| Gypsum ceiling zone | #ececea + dotted board | |
| Paving | #d8d6d0 | |
| Planting lines | #8d9b80 / #6f7d63 | |
| Sightlines / warm air out | #c23a5e | 194,58,94 |
| Air (main) | #7f9cad، 30–55% opacity | 127,156,173 |
| Air (small diagrams) | #2a93a3 | |
| Sun ray | #f2d64b، 25–45% (7–12% خلف screen) | 242,214,75 |
| Profiles/mullions | #555 – #6a6a67 | |
| Assumed fire seal | #c9573c | |
| Waterproofing/drainage (assumed) | #2a5d8a dashed | |

**تدرج الخطوط (mm على الشيت عند مقياس الطباعة)**
- Cut outline للإنشائي: 0.35–0.50 (0.6 عند 1:5). Cut slab edge: 0.50. Cut fins/profiles: 0.25–0.40.
- Seen (projection): 0.10–0.15. Beyond: 0.04–0.08 رمادي فاتح، fills بـ 30–45% opacity.
- Dimensions: خط 0.10، ticks 0.22 (slash 45°، **لا أسهم أبدًا**)، extension 0.08 رمادي. Leaders: 0.12.
- Dashed: sightlines 1.6/0.9؛ hidden/below 1.2/0.6؛ section line dash-dot 6/1.2/1/1.2.

**Hatches و Fills**
- Concrete cut: fill رمادي + hatch 45° رفيع (#9c9c98, 0.1)؛ عند 1:20 فأكبر أضف aggregate dots ومثلثات (pattern `concL`). عنصر خرساني ثانٍ/edge beam: نفسه بالرمادي الدافئ #d8d7d2 (`concB` / `concLB`).
- Fin/cladding cut: #c5b6a1 + hatch −45° رفيع (#7d705f).
- Gypsum board: #e8e8e6 بنقاط؛ الـ plenum شريط باهت والنص **داخله** ("SUSPENDED GYPSUM CEILING (assumed)")، hangers خطوط رأسية رفيعة.
- Screed: باهت بنقاط متفرقة. Insulation: أصفر باهت #f6f0d8 بخط موجي.
- Soil: أبيض مع basket-weave (كتل 3 خطوط متبادلة) بـ #9a9a96.
- Wall poché في المساقط: مصمت #3a3a38، والجدران المنحنية قوس ناعم (لا درجات).

**Entourage**
- بشر: silhouettes سوداء مصمتة واقعية **1.75 m** (رجل/امرأة، بعضها معكوس)؛ خلف screen تصبح أشباحًا 14% opacity.
- أشجار: خط واحد متفرع (0.1–0.3)، أوراق بيضاوية مفتوحة، رمادي-أخضر، بلا fills. نباتات الأحواض: ضربات أوراق/عشب لا دوائر. سيارات/أثاث شارع فقط إن شرحت شيئًا.

**التعليقات (Annotation)**
- Tag: نقطة (r ≈ 0.45) على العنصر ← leader رأسي ثم أفقي (لا مائل) ينتهي **1–2 mm قبل** أول حرف؛ اسم bold UPPERCASE + 1–3 أسطر وصف رمادي. الـ leader لا يعبر نصًا. tags الواجهة في عمود يسار والداخلية يمين، محاذاة.
- كل عنصر غير موجود في النموذج المصدر يُكتب "(assumed)" أو "(recommended)"؛ حقائق النموذج "(model)".
- Level heads: ▽ على خط منسوب متقطع، القيمة bold بإشارة (+13.10 / −0.40)، الاسم صغير رمادي فوقها؛ كلها في عمود واحد أقصى يمين الـ view.
- Dimensions: سلاسل بـ slash ticks، النص في المنتصف فوق الخط (الرأسي مدار −90°).
- Section marks: دائرة سوداء بحرف أبيض + dash-dot. North arrow: دائرة بإبرة نصف مصمتة و "N".
- View title: دائرة برقم (01, E1, D3...)، عنوان bold، scale/subtitle رمادي تحت خط 0.35.
- Banner رسالة رئيسية (اختياري، واحد لكل شيت): عرض الشيت، أسود، كلمة bold بيضاء + جملة (مثل "FULLY SCREENED").

**طبقات التحليل (فقط عند طلب دراسة)**
- المخططات العادية بلا sightlines/air/sun/banner (Sherif 2026-11-12).
- Sightlines: حمراء متقطعة من العين (نقطة حمراء عند **1.60 m**) تنتهي بـ ✕ أحمر. Air: أسهم منحنية زرقاء-رمادية عريضة. Sun: شريط أصفر شفاف بزاوية **حقيقية** مع التاريخ/الوقت.
- كل رقم من الهندسة يُوسم "geometric estimate"؛ لا شيء يُسمى simulation. المنطق البيئي حسب `C:\Users\win\.claude\standards\ENV_ARCH_STUDY_STANDARD.md`.

**الواجهات**
- ترتيب العمق: الخلفية مسطحة باهتة ← ظل Gaussian ناعم للـ screen على الجدار (offset ≈ 2–3 mm، blur 1.4، 16–20% أسود) ← العناصر الأمامية ← ظل رفيع تحت كل band أفقي.
- الـ fins/louvres وجهًا وجهًا من الـ footprints الحقيقية: الوجه الأمامي الأفتح والمائل أغمق. الأرض: خط سميك + شريط paving + soil hatch.

**تخطيط الشيت**
- يتبع revit-sheet-methodology (ملء الشيت، أكبر مقياس، محاذاة الـ views ذات المناسيب المشتركة بنفس المقياس الرأسي، المسقط تحت الواجهة على نفس المحور الأفقي، خطوط فاصلة).
- المجموعة النموذجية: 1:50 elevation + section + plan، 1:20 enlarged section مع واجهته، تفاصيل 1:5–1:25؛ legend + ملاحظات مرقمة؛ title block (رقم اللوحة، المقاييس، الشيت، التاريخ).

**وصفة Revit**
- Visual style: **Hidden Line**. الواجهات: Shadows مع Sun intensity ≈ 20 و Shadow intensity ≈ 25، أو Ambient Shadows فقط؛ sections/plans بلا ظلال.
- **Depth Cueing** في الواجهات والقطاعات: fade start ≈ 30%، end ≈ 80%، limit ≈ 45%.
- Line weights: cut = pen 5–6، projection = 1–2، beyond/far clip = 1 رمادي (#8a8a86)؛ tags و dimensions = pen 1.
- VG/filters: خرسانة (floors, framing, concrete walls) cut fill (217,217,214) + hatch قطري (156,156,152)؛ cladding/fins/louvres surface (216,205,189)، cut (197,182,161)، lines (133,122,106)؛ glass surface (219,226,229)، lines (92,122,136)؛ block poché (58,58,56) وجدران التشطيب halftone؛ أسقف القطاع gypsum (232,232,230) بنقاط؛ الأعمدة الإنشائية في المساقط بالأحمر الطوبي (128,0,0) حيث تطلب قواعد Sherif.
- التربة: Filled Region بـ basket-weave (#9a9a96) — **لا عناصر model أبدًا**.
- Levels: عائلة ▽ للمشروع في عمود يمين؛ لا circle heads. Tags بـ "Orthogonal" leaders. Dimension type بـ diagonal tick ونص **2.0–2.3 mm**.
- Sightlines: line style أحمر #c23a5e، dash 1.6/0.9، **0.25 mm** + ✕ generic annotation. Air (127,156,173)؛ sun filled region (242,214,75) بنمط متفرق (لا شفافية في Revit — استخدم draw order).
- بشر/أشجار: 2D detail components سوداء وعائلات أشجار رفيعة (لا RPC في hidden line).
- دفعات صغيرة، بلا modal warnings، التصدير بـ `Document.Export` + `PDFExportOptions` فقط.

**مسار SVG/PDF**
- `assets/lib.js` (Node بلا dependencies): palette `C`؛ `make()` (`line`, `rect`, `poly`, `text`, `leader`, `levelTag`, `dimH`/`dimV`, `flow`, `title`, `human`, `tree`, `plants`, `northArrow`)؛ `defs()` hatches و blur؛ `finFaces`, `loops`, `pocheQuads`, `wallLoops`, `rayHit`؛ `write()` إلى SVG + HTML.
- الهندسة من النموذج (Ruby read-only cuts، footprints) — لا اختراع. أمثلة: `D:\Projects\البراك\DETAIL\_src\d01.js`, `d02.js`, `d03.js`.
- PDF: Chrome headless `--print-to-pdf` (@page بالـ mm)؛ PNG: `--screenshot`. اهرب `<` و `&` في نصوص SVG.

## خطوات العمل (Workflow)
**إعادة رسم views Revit المنتهية كمجموعة عرض خارجية (طلب 2026-11-12):**
1. لا تسأل عمّا أجاب عنه. خذ قائمة الشيتات/الـ views. **Read only** على النموذج؛ أي إعداد يُغيَّر يكون في duplicate view مؤقت.
2. استخراج لكل view بدفعات صغيرة عبر Revit MCP/API: نوع الـ view، المقياس، crop box، cut plane، اتجاهات (`RightDirection`, `UpDirection`, `ViewDirection`)، levels/grids؛ لكل عنصر ظاهر (`FilteredElementCollector(doc, view.Id)`): category, type, materials و `get_Geometry(new Options{ View = view })` مسقطة إلى (u, v) بالـ mm مع وسم cut/projection والعمق؛ rooms, doors/windows, dimensions, tags, text, level heads كبيانات؛ كتابة JSON لكل view في `PRESENTATION\_src`.
3. إعادة الرسم بـ `assets/lib.js`: poché للجدران (`wallLoops` / `pocheQuads`)، concrete hatch للبلاطات، المواد حسب الـ palette، level heads يمينًا، tags قائمة الزاوية، سلاسل الأبعاد من أبعاد Revit، بشر وأشجار، ظل ناعم في الواجهات. بلا طبقات تحليل إلا بطلب.
4. تخطيط الشيتات حسب منهجيته + title block، وتصدير PDF بـ Chrome headless.
5. QA كل شيت كـ PNG مقابل `assets/ref-LS-D0x.png` ومقابل PDF Revit لنفس الـ view؛ الهندسة مطابقة تمامًا بلا عناصر مخترعة، ثم التسليم.

## نقاط الفحص / QA Checklist
- [ ] مقارنة جنبًا إلى جنب مع الشيتات المرجعية قبل الإرسال
- [ ] تصدير PNG وتكبير كل عمود tags وكل سلسلة أبعاد
- [ ] لا leader يعبر نصًا ولا label فوق شكل
- [ ] لا طبقات تحليل في المخططات العادية
- [ ] العناصر غير الموجودة في النموذج موسومة (assumed)/(recommended)
- [ ] Level heads في عمود واحد يمين، ticks مائلة لا أسهم
- [ ] الهندسة مطابقة لـ Revit بلا اختراع

## أخطاء ممنوعة / تحذيرات
- ألوان مشبعة للمواد، أسهم في الأبعاد، leaders مائلة.
- تسمية أي رقم هندسي "simulation".
- نمذجة التربة كعناصر model، أو استخدام RPC في hidden line.
- تعديل نموذج Revit أو views Sherif عند إعادة الرسم الخارجي.
- ملاحظة: Filled regions في Revit بلا شفافية.

## مخرجات متوقعة
- شيتات PDF/PNG بأسلوب Albarak (من Revit أو من SVG/HTML).
- ملفات JSON لكل view في `PRESENTATION\_src` عند إعادة الرسم.
- View templates في Revit تحقق الأسلوب.

## ملفات مرجعية داخل الـ skill
- `assets/ref-LS-D01.png` — مرجع: واجهة/قطاع/مسقط 1:50 وتفاصيل 1:5 / 1:25.
- `assets/ref-LS-D02.png` — مرجع: قطاع مكبر 1:20، واجهة screen، دراسات شمس.
- `assets/ref-LS-D03.png` — مرجع: قطاع وخطة sightline 1:100.
- `assets/lib.js` — مكتبة Node لرسم SVG بهذا الأسلوب (palette، hatches، أدوات رسم، poché، ray test).
- ملاحظة: مجلد `assets/` غير موجود في نسخة الـ skill المتزامنة حاليًا (SKILL.md فقط).

## مهارات مرتبطة
- revit-sheet-methodology (تخطيط الشيت)
- riyadh-permit-drawings
- interior-3d-presentation-style
