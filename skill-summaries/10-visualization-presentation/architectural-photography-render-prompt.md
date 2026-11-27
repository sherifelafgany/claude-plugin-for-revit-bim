# architectural-photography-render-prompt — prompt تصوير معماري قبل أي render بالذكاء الاصطناعي

> **الغرض:** تحويل view من BIM (Revit، SketchUp، Rhino، أي view مظلل أو clay) إلى prompt كامل بأسلوب مصور معماري عالمي، واقعي وترويجي ووفي للتصميم. **متى تُستخدم:** قبل أي render بالذكاء الاصطناعي (Canva، Midjourney، إلخ) — كتابة الـ prompt والحصول على اعتماد المستخدم أولاً.

## الفكرة الأساسية
الـ BIM view هو المصدر الوحيد للهندسة؛ الـ AI يحسّن ولا يعيد التصميم. كل عنصر مفتاحي يُوصف صراحة **بعدده وموقعه ومادته** لأن الـ prompt المبهم يجعل الـ AI يبسّط الهياكل المعقدة (الفشل الكلاسيكي: خيمة شدّ تظهر ككتلة ضعيفة مبهمة). يصلح لأي نوع مشروع: فلل، أجنحة، خيام ومظلات، لاندسكيب، داخليات، تجاري.

## القواعد والأرقام الأساسية
1. **لا render بلا prompt معتمد:** عرض الـ prompt الكامل لكل صورة وانتظار اعتماد صريح ("approved"، "go"، "تمام"، "اعتمد"). أي تعديل يُلغي الاعتماد. لا توليد صورة في نفس الدور الذي يُعرض فيه الـ prompt أول مرة.
2. **الدقة المعمارية قبل التفسير الإبداعي.**
3. **كل عنصر مفتاحي موصوف بالعدد** (قمم، صواري، درج، بسطات، فتحات).
4. **prompt واحد لكل صورة** بنفس الهيكل (9 أقسام) في كل مرة؛ يتغير فقط الكاميرا والعناصر الظاهرة والضوء.
5. الرد بلغة المستخدم (العربية)، والـ prompt بالإنجليزية.
6. **حد Canva:** Canva (Dream Lab / أدوات الصور) يحتفظ بأول **~1,500 حرف (~236 كلمة)** فقط. يُسلَّم دائماً بجانب الـ prompt الكامل نسخة Canva مضغوطة **≤ 1,450–1,500 حرف** بنفس الهيكل (role، references، preservation مع الأعداد، materials، camera، light، quality)، وقواعد الهندسة الحرجة أولاً.
7. **بوابة المراجعة بالحجم الكامل:** لا يُعتبر الـ render ناجحاً من معاينة الـ connector الصغيرة (~200 px)؛ يُفحص بالحجم الكامل (يُنزّله المستخدم عند الحاجة).

**المدخلات:** قص الهوامش البيضاء قبل الرفع؛ نفس نسبة Reference 1 في المخرج (عادةً **16:9، مثل 1680x944**)؛ حقائق المشروع من النموذج (ارتفاعات المنصات/الأدوار، البصمة، عدد الدرجات، القمم، الصواري، الفتحات، أرجل المنحدر، البسطات، الأحواض، جدول المواد، الشمال، الموقع، المناخ). لا اختراع لحقيقة مجهولة.

**تدقيق الـ view المرجعي:**
- **Keep exactly:** البصمة، الكتلة، الفتحات، الدرج، الأسقف، الإنشاء، هندسة اللاندسكيب.
- **Fix the look only:** الأسطح المظللة أو شبه الشفافة (مثال: "the grey semi-transparent surface is the tent membrane: render it as an opaque-looking ivory PTFE membrane").
- **Add (context only):** سماء، شمس وظلال، أرض، سياق بعيد، نباتات في الأحواض، أشخاص للمقياس — مع التصريح إن كان المرجع بلا ظلال/سماء/محيط.
- **Forbid:** درابزين، درج إضافي، أدوار، بلكونات، أحواض، كراسي استلقاء، عشب، قمم/صواري إضافية، مسابح.

**مرجعان للعناصر المعقدة:** عند عنصر ذي طابع لا يظهر في Revit (أغشية شدّ، شاشات، مشربيات، برجولات خشب، تكسيات خاصة):
> Reference 1 is the BIM view and is the ONLY source of geometry. Reference 2 is used ONLY to show the character and detailing of [element].
- render قديم كمرجع مزاج فقط: "take nothing physical from Reference N". عند تعديل render قديم لتحديث عنصر واحد: سرد كل جزء آخر تغيّر منذئذ كاستثناء صريح من "change only X".

**دليل الكاميرا (قواعد عامة):** رأسيات رأسية (tilt-shift)، أفق مستوٍ؛ two-point للمباني (one-point فقط للمتماثل المقصود)؛ الأثلاث أو مركز متعمد؛ foreground anchor بلا أرض فارغة كبيرة؛ خطوط موجهة من زاوية الكادر؛ لا قص لعنصر مفتاحي إلا كأداة تأطير؛ الهيكل المميز يُقرأ ضد السماء؛ ضوء جانبي أو 3/4 خلفي، لا ضوء أمامي مسطح.

| اللقطة | الكاميرا | العدسة | التكوين والضوء | النسبة |
|---|---|---|---|---|
| A. Hero aerial (غلاف) | درون 30–50 m، 3/4 من أكثر الزوايا انفتاحاً، ميل 35–40° | 28–35 mm | المشروع كاملاً بسياق حتى الأفق؛ سماء 20–25%؛ آخر العصر/الغروب، ظلال طويلة، غيوم مضاءة من أسفل | 16:9 |
| B. Eye-level 3/4 رئيسي | 1.6–1.8 m طبيعي أو 1.2–1.4 m بطولي؛ 8–20 m من أقرب زاوية | 24–35 mm tilt-shift | المبنى 60–70% من الكادر، واجهتان، المدخل والزجاج والدرج والهيكل المميز؛ golden hour والشمس خلف كتف الكاميرا أو صباح صافٍ | 3:2 أو 16:10 |
| C. Landscape | 5–15 m مرتفع، ميل 30–45° على المسار الرئيسي | 35–50 mm | اللاندسكيب هو الموضوع والمبنى صغير أعلى الكادر؛ 1–2 شخص؛ منتصف الصباح بظلال محددة | 16:9 |
| D. Detail/material/lifestyle | 1.2–1.6 m، على 4–8 m | 35–50 mm (85 mm للتفاصيل الضيقة) | التقاء العناصر (زاوية زجاج، قاعدة إنشاء، باطن الغشاء، فواصل الحجر، موقد)؛ golden/blue hour، sunburst خفيف مسموح | 4:5 أو 3:2 |
| Dusk/blue hour (اختياري) | كـ A أو B | — | 15–20 دقيقة بعد الغروب؛ إنارة موجودة في التصميم فقط أو "subtle warm architectural lighting" | — |

**قالب الـ prompt (الهيكل):**
```
Act as an internationally recognized architectural photographer, architectural visualization artist and professional
CGI rendering specialist. [aerials: + aerial drone cinematographer | landscape: + landscape architecture photographer |
interiors/terraces: + hospitality interiors photographer]

I am providing [one / two] reference images. Reference 1 is a [Revit/BIM] 3D view of my project and is the ONLY source
of geometry. [Reference 2 is an earlier approved render used ONLY for the character of the <element>.] Transform
Reference 1 into an exceptionally realistic, high-resolution [eye-level / aerial / landscape / editorial detail]
architectural photograph ... captured by a professional architectural photographer with a high-end full-frame camera.

The objective is NOT to redesign the architecture. ... improve realism, image quality, materials, lighting, landscape
and camera composition while preserving the original architectural identity.

1. STRICT ARCHITECTURAL PRESERVATION  - exact footprint/dimensions/proportions/geometry/orientation
   - [each key element with count, position, height, shape — e.g. "exactly TWO tall pointed membrane peaks on two
     slender masts; four raked corner struts; ... no single peak, no gable"]
   - [stairs/ramps/landings: number, positions, "no steps on the ramp"] - [reference fixes]
   - Do not add floors, balconies, balustrades, doors, windows, planters, stairs or any structure not in Reference 1.
2. MATERIAL REALISM       - [walls/platform: e.g. warm beige limestone ...] - Glazing: clear low-iron glass, slim [colour]
                            frames ... Avoid dark, opaque or mirror-like glass. - [special: ivory-white PTFE, taut ...]
                            - Paving and stairs: natural stone ...
3. LANDSCAPE AND CONTEXT  - planting true to climate [native arid shrubs ... no lawn; no tropical garden]; no repetitive
                            AI vegetation - transitions soil/gravel/paving - context [desert, fields, palms ...] to horizon
4. CAMERA AND COMPOSITION - [shot type, position, compass direction, height] - [lens FF], tilt-shift/corrected verticals,
                            no exaggerated wide-angle distortion - [composition] - intentionally placed ... not random
5. LIGHTING AND ATMOSPHERE - [time, sun direction vs camera] - soft defined shadows ... warm highlights without
                            excessive orange grading - [if no sky/shadows in reference: add them]
6. PHOTOREALISM AND IMAGE QUALITY - ultra-photorealistic, PBL, GI, AO ... HDR without an HDR look; no noise/blur/artefacts
7. FINAL OUTPUT           - one photograph, [aspect ratio], highest resolution; no text, watermark, labels or borders
FINAL INSTRUCTION         - Prioritize architectural accuracy ... real photograph of a completed, professionally built
                            [project type] in [location], not an AI-generated concept.
```

**الإخفاقات الشائعة وعلاجها:**

| الإخفاق | العلاج في الـ prompt |
|---|---|
| الهيكل المميز غير واضح | عدّ ووصف كل جزء (قمم، صواري، دعامات، كابلات، فتحات قوسية)، مرجع طابع ثانٍ، ضوء جانبي، ضد السماء |
| زوايا عشوائية | ضبط الكاميرا في BIM؛ تحديد النوع والارتفاع والمسافة والعدسة والتكوين |
| الـ AI يخترع درابزين/أحواض/درج | قائمة "do not add" بالأسماء |
| زجاج بلاستيكي أو أسود | clear low-iron glass بانعكاسات ورؤية داخلية |
| زراعة استوائية أو مستنسخة | أنواع وألوان إقليمية؛ "no lawn, no repetitive vegetation" |
| برتقالي زائد / HDR | "warm highlights without excessive orange grading; no HDR look" |
| صورة مسطحة بلا ظلال | التصريح بغياب الظلال/السماء وطلب شمس وظلال فيزيائية |

## خطوات العمل (Workflow)
0. **الوصول للنموذج:** سؤال عن البرنامج (Revit أو SketchUp) والمشروع واللقطات. Revit: Revit MCP على جهاز المستخدم لضبط كاميرات 3D وتصدير الصور (مع revit-api-safety للكتابة). SketchUp: computer use لضبط المشاهد والتصدير. الزوايا الاحترافية تُضبط في البرنامج أولاً.
1. **جمع المدخلات:** الـ views المرجعية ونسبتها، حقائق المشروع، renders معتمدة سابقة أو مراجع مواد، المخرج (اللقطات، النسبة، الاستخدام: غلاف/عرض/سوشال).
2. **تدقيق الـ view:** keep / fix look / add / forbid.
3. **مرجعان للعناصر المعقدة** مع تحديد دور كل مرجع.
4. **اختيار اللقطة والكاميرا:** الزاوية الخاطئة → تغيير الكاميرا في Revit/SketchUp وإعادة التصدير، ثم كتابة قسم الكاميرا مطابقاً.
5. **كتابة الـ prompt** بالقالب.
6. **بوابة الاعتماد:** لكل صورة: غرض في سطر، الـ view المرجعي، الـ prompt الكامل في code block، ثم نسخة Canva (≤ 1,500 حرف مع عدد الأحرف). لعدة صور: قائمة واعتماد كل واحدة.
7. **Render وفحص:** بعد الاعتماد عبر Canva connector على حساب Pro للمستخدم (أو تسليم الصور الأساسية والـ prompts إن كان سيُخرج بنفسه). تسليم الصور عند تقييد المرفقات: رفعها عبر Canva connector إلى Uploads على `https://www.canva.com/M/<mediaId>`. الفحص بالحجم الكامل، والرفض وإعادة التشغيل (أو تعديل الـ prompt وإعادة الاعتماد) عند أي فشل؛ إبلاغ المستخدم بالفحوص التي اجتازتها الصورة.

## نقاط الفحص / QA Checklist
- [ ] كل عنصر معدود موجود بالعدد الصحيح (قمم، صواري، درج، بسطات، فتحات).
- [ ] لا إضافات ممنوعة؛ لا شيء معاد تصميمه أو مبسط أو ذائب.
- [ ] الرأسيات مستقيمة، لا fisheye، الأفق مستوٍ.
- [ ] المواد تُقرأ صحيحاً؛ لا زجاج بلاستيكي ولا زجاج أسود عاكس، لا نباتات منسوخة.
- [ ] لا نص ولا علامة مائية ولا artefacts.
- [ ] الفحص تم على الصورة بالحجم الكامل لا المعاينة ~200 px.
- [ ] نسخة Canva ≤ 1,500 حرف مع عدد الأحرف.

## أخطاء ممنوعة / تحذيرات
- توليد صورة قبل الاعتماد أو في نفس دور عرض الـ prompt.
- محاولة تغيير الزاوية بإحداثيات في الـ prompt (أدوات img2img تتبع كاميرا المرجع).
- أخذ عناصر مادية من render قديم مستخدم كمرجع مزاج.
- prompt مبهم بلا أعداد؛ تجاوز حد 1,500 حرف في Canva (يُقطع ما بعده).
- اعتماد نجاح الصورة من المعاينة الصغيرة.

## مخرجات متوقعة
- لكل صورة: غرض، view مرجعي، prompt إنجليزي كامل (code block)، نسخة Canva مضغوطة مع عدد أحرفها.
- بعد الاعتماد: الصورة المنتجة (أو الصور الأساسية والـ prompts) مع قائمة الفحوص المجتازة.

## مهارات مرتبطة
architectural-visualization-director، canva-night-shot-prompt، revit-api-safety.
