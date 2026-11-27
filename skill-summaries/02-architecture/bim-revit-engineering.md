# bim-revit-engineering — مهارة BIM / Revit الهندسية الشاملة (V1.1)

> **الغرض:** العمل كـ Senior BIM Architect / BIM Manager / قائد مكتب فني معماري بخبرة ~20 سنة لإنتاج نموذج ومجموعة لوحات منسقة وقابلة للبناء والتدقيق. **متى تُستخدم:** لإنشاء أو تعديل أو تدقيق أو تنسيق أو تفصيل أو توثيق أو جدولة أو مراجعة نماذج BIM معمارية ولوحات تنفيذية، مع الحكم الهندسي وقابلية البناء وسياق المشاريع السعودية وبوابات QA/QC إلزامية.

## الفكرة الأساسية
الهدف ليس أن «يبدو Revit صحيحًا» بل نموذج رقمي منسق، مُدافَع عنه تقنيًا، قابل للبناء والتدقيق. «Model the building, not a picture». المهارة معيارية: `SKILL.md` = المنطق والتوجيه، `references/` = المعايير، `workflows/` = الإجراءات، `checklists/` = بوابات QA. لا تحل محل وثائق المشروع المعتمدة أو SBC الحالي أو اشتراطات البلدية أو الـ BEP أو وثائق المصنّع، وتتعمد **عدم تثبيت أرقام تنظيمية**. مرجع البرنامج: Revit 2027.

## القواعد والأرقام الأساسية
**ترتيب الأولويات (1 أعلى):** 1) Life safety & statutory compliance 2) Approved project requirements 3) Current authority/code 4) Constructability 5) Multidiscipline coordination 6) Functional performance 7) Maintainability 8) Documentation clarity 9) BIM data quality 10) Cost 11) Aesthetic 12) Modeling convenience. لا اختيار حل لأنه أسهل نمذجة.

**انضباط المصادر:** كل معلومة تُصنّف CONFIRMED / ASSUMED / TO BE VERIFIED / NOT PROVIDED؛ لا تحويل افتراض إلى حقيقة. عند التعارض: حدده، اذكر المتطلبات المتنافسة، حدد الأسبقية من حوكمة المشروع، وصعّد إن مسّ compliance أو cost أو safety أو geometry أو issue.

**تسلسل هرمي للسلطات (Saudi):** 1) SBC/التنظيم الحالي 2) Municipality/authority 3) Planning controls 4) Developer 5) Consultant 6) Office standard 7) Engineering assumption. تحقق من المصدر الرسمي قبل ذكر أي رقم لـ: setbacks, parking, heights, openings, stairs, accessibility, fire resistance, egress, ventilation, daylight, plot coverage, floor-area limits, boundary conditions. حدّد المدينة، التصنيف، الاستخدام، عدد الأدوار، الموقع، مسار الجهة، إصدار الكود. **لا نقل متطلب من الرياض إلى القصيم أو جدة دون تحقق.**

**النمذجة**
- عناصر Revit حقيقية (native categories، families بارامترية معتمدة)؛ لا تزييف بخطوط 2D. Single source of truth: الأبعاد والـ tags والجداول والكميات من النموذج.
- تجنب: in-place families غير ضرورية، CAD مستورد زائد، high-poly، مواد مكررة، nested families منتفخة، تكرار views غير مُدار، parameters بلا معنى.
- Levels للمناسيب الفعلية فقط (لا levels لتنظيم الـ views)؛ Grids مرجع تنسيق حقيقي بأسماء ثابتة. الجدران: type, layers, constraints, offsets, joins, room-bounding, phase, location line, material. الأرضيات: structural vs finish، build-up، slope، shafts، waterproofing، thresholds. الأسطح: slopes, drainage, outlets, parapets, insulation/waterproofing, access. الأبواب/الشبابيك فتحات حقيقية. الغرف محدودة عمدًا ومراجعة المساحة.
- **لا تحريك Internal Origin** كحل للتنسيق؛ وثّق Project Base Point, Survey Point, Project North, True North.
- إعداد المشروع: units, levels, grids, north, origin, base/survey point, shared coordinates, phases, worksets, browser, view/sheet/family/material naming, revision method.

**الـ Families:** ابحث في المعتمد → تكييف موجود → category و hosting → parameters (type/instance/reporting/shared) → types → visibility/detail → متطلبات الجدولة → اختبار في سياق المشروع (placement, constraints, flipping, visibility, detail levels, schedules, tags, materials, performance). تجنب blends زائدة، meshes مستوردة، nesting مفرط، هندسة عرض فقط.

**التصميم التقني:** لكل تجميعة: كيف تُبنى/تُسند/يُتحكم في الماء/الأداء الحراري والصوتي/الاتصال بالمجاور/التشطيب/الصيانة/التنسيق مع الإنشائي و MEP/هل تحتاج تفصيلة. واجهات عالية الخطورة: wall/floor, wall/roof, wall/window, wall/door, wet area/floor, wet area/wall, parapet/roof, stair/floor, façade/slab, ceiling/MEP, shaft/wall/floor, penetrations, movement joints.

**التنسيق:** أنواع التصادم HARD / CLEARANCE / WORKFLOW / DESIGN / DOCUMENTATION CLASH. الخطورة **P1** safety/compliance، **P2** constructability/major، **P3** functional/documentation، **P4** cosmetic. سجل المشكلة: ID, location, disciplines, category, severity, description, evidence, proposed resolution, owner, status, verification result. المشكلة CLOSED فقط بعد إعادة الفحص. عدم التقاطع الفيزيائي لا يكفي — راجع clearance الصيانة والتركيب. clash detection ليس كل التنسيق.

**التوثيق والإنتاج**
- التسلسل الإلزامي: **MODEL → VIEWS → ANNOTATION → DETAILS → SCHEDULES → SHEETS → PDF → QA**.
- قبل إنشاء أي view: Purpose, Audience, Scale, Required information, Sheet destination. لا مقياس «صحيح عالميًا»؛ معيار المشروع يتقدم.
- هرمية الرسم: setting-out → plans → elevations → sections → enlarged → schedules → details → notes/legends. هرمية الجرافيك: cut → primary projection → secondary → overhead → annotations → references.
- View Templates و Filters و Object Styles بدل overrides يدوية؛ لا detail lines لإخفاء أخطاء النمذجة؛ لا temporary hide/isolate.
- تسمية الشيت: `DISCIPLINE - SERIES - NUMBER - DESCRIPTION` (مثال `A-201 - GROUND FLOOR PLAN`)؛ تسمية PDF: `PROJECT_DISCIPLINE_ISSUE_DATE.pdf` (معيار المشروع يتقدم).
- الشيت يجيب عن سؤال معلوماتي واحد متماسك؛ **لا تحميل الشيت لتقليل عدد الشيتات**؛ margins ثابتة، viewports محاذاة، **balanced whitespace**.
- الفهرس يطابق الشيتات الفعلية: لا ghost sheets ولا شيتات صادرة غير مدرجة.
- هرمية الأبعاد: overall → grids/setting-out → structural → walls → openings → rooms → critical clearances → (level) → detail-specific. لا تكرار أو سلاسل متعارضة أو أبعاد لهندسة غير مستقرة.
- هرمية التعليق: Primary (dimensions, levels, room ID, tags, references) > Secondary (material tags, keynotes, notes) > Tertiary. لا tag تلقائي لكل عنصر. كل marker يشير لـ view حقيقي؛ كل callout له مرجع متبادل detail ↔ source. تحرك التعليق لا الهندسة لحل التداخل. لا كتابة مناسيب يدويًا حين يمكن الإشارة إلى datum.
- الجداول: Door (mark, type, width, height, operation, fire/acoustic إن كانت موثقة، hardware)، Window (+ sill, glazing)، Room (number, name, level, area)، Finish (floor, wall, skirting, ceiling). **لا اختراع finish codes.** فحص: missing, duplicates, duplicate marks, unused types, stale values.
- حالات الإصدار: WIP → INTERNAL QA → COORDINATION → CLIENT REVIEW → AUTHORITY REVIEW → FOR CONSTRUCTION → AS-BUILT. لا تاريخ revisions مزيف؛ clouding where required.
- PDF هدف QA مستقل: راجع full page / reading zoom / detail zoom. أعطال شائعة: إزاحة كاملة (page setup/print region)، نص صغير (annotation scale)، تفاصيل مختفية (lineweights/detail level/vector-raster)، صفحات فارغة (print set)، جدول مقطوع (عرض الجدول)، logo مفقود.

**التفاصيل و Shop drawings:** التفصيلة مطلوبة حين لا تكفي الهندسة؛ تجيب WHAT / WITH WHAT / WHERE / HOW connect / HOW installed / WHAT coordinate. **لا اختراع:** product data, manufacturer systems, fire ratings, acoustic ratings, structural capacities, proprietary fixings — تُعلَّم TO BE VERIFIED. التمييز بين design requirement / typical practice / manufacturer requirement / contractor proposal / item requiring approval / site measurement. المراجع ثنائية الاتجاه plan ↔ detail.

**الهوية المحلية (Riyadh):** تُقيّم كنظام (massing, proportions, openings, solid/void, shading, materials, colours, parapets, roof, rhythm, privacy, street relationship, climate) لا كزخارف؛ تحقق من الدليل المحلي عند ادعاء الامتثال. لا فرض افتراضات ثقافية دون سند من البرنامج.

**الفلل:** مراجعة العلاقة بين المدخل، الضيوف، العائلة، النوم، الخدمات، المطبخ، الحمامات، الدرج، المواقف، الخارجي، السطح. مخاطر: column conflicts, stair geometry, wet-stack discontinuity, shaft undersizing, ceiling congestion, roof drainage, waterproofing continuity, door swings, maintenance access, openings vs structure.

**LOD:** الفصل بين geometric detail و information maturity و model use؛ لا over/under-modeling. **البيانات:** parameters بلا تكرار/غموض، shared parameters فقط عند الحاجة للجدولة/التبادل؛ لا بيانات بلا استخدام محدد؛ COBie حسب قالب المشروع المعتمد فقط ولا اختراع asset data.

**الأتمتة (Dynamo/API):** مناسبة لـ views, sheets, naming, parameters, tagging, audits, room validation, index, QA reports؛ غير مناسبة دون مراجعة لـ compliance, structural design, fire engineering, final planning, manufacturer design, authority interpretation. عقد كل script: inputs, assumptions, operation, output, validation, error handling, rollback. دفعات صغيرة؛ بعد كل دفعة: عدّ، فحص failures، QA، تسجيل الاستثناءات.

**نتيجة QA:** فقط PASS / FAIL / PASS WITH COMMENTS / TO BE VERIFIED — **ممنوع "looks fine"**، وممنوع "complete" مع بنود حرجة مفتوحة.

## خطوات العمل (Workflow)
**المسار الافتراضي لمشروع جديد (25 مرحلة):** brief → input audit → code applicability → setup → units → levels → grids → coordinates → model organization → planning → primary model → openings & vertical circulation → roof/site → rooms → structural coord. → MEP coord. → technical assemblies → documentation views → dimensions/tags/notes → schedules → details → sheets → QA/QC → plot/PDF QA → final issue. لا تخطٍ صامت؛ استخدم NOT APPLICABLE. اقرأ ملف الـ workflow قبل كل مرحلة رئيسية.

**قاعدة السبب الجذري:** أوقف المسار المتأثر → السبب الجذري → الأثر اللاحق → إصلاح المصدر → فحص التابع → إعادة بوابة QA. لا ترقيع بـ overrides.

**المراجعة المستقلة (3 تمريرات):** Spatial (layout, circulation, zoning, furniture, openings, stairs) → Technical (structure, MEP, wet areas, shafts, roof, waterproofing, access) → Documentation (dims, tags, schedules, details, sheets, revisions, print). أسئلة التحدي: ماذا سيسأل المقاول/يرفض الإنشائي/يرصد منسق MEP/يسأل مراجع الجهة/يسيء فهمه مهندس الموقع/يصعب صيانته؟

**صيغة التواصل الإلزامية عند الغموض:** CONFIRMED / ASSUMPTIONS / ISSUES / ACTION / QA RESULT.

**مراجعة ذاتية قبل التسليم:** Geometry, Engineering, Constructability, Coordination, Documentation, BIM, Compliance, Maintainability, Consistency.

## نقاط الفحص / QA Checklist
**بوابات SKILL.md:** A Model Integrity — B Design Integrity — C Coordination — D Documentation — E Deliverable. (ملف 06 يقسمها 6: Model, Architecture, Technical, Coordination, Documentation, Issue.)
- [ ] Model: units, levels, grids, coordinates, north, walls/floors/roof/doors/windows، stairs، rooms valid، families approved، materials، links current، warnings، duplicates، CAD imports
- [ ] Coordination: arch/structure، arch/MEP، shafts، openings، ceiling zones، duct/pipe routes، electrical، fire، access، maintenance clearance، installation sequence، resolved issues rechecked
- [ ] Documentation: index، plans، dims، tags، sections، elevations، details، schedules، legends، sheet numbers، view titles، scales، north arrows، revisions، no clipping، no conflicts، PDF reviewed
- [ ] Annotation: dimension hierarchy، no conflicting/duplicate dims، room/door/window/material tags، levels، section/elevation markers، callouts، keynotes، no text/tag collisions، no clipping، readable at print scale
- [ ] Sheet: number، title، title block، revision، issue state، viewports aligned، titles visible، scales، legends، schedules readable، no overlap، no accidental blank area، index agrees
- [ ] PDF: page size، orientation، sheet count، no blank pages، no clipping، lineweights، text/dims/symbols/hatches/logos visible، title blocks، revision، filenames، opens correctly
- [ ] Final issue: all gates passed، outstanding issues + assumptions + TO BE VERIFIED listed، revisions، filenames، PDF complete & legible، model saved، exports checked، package organized

## أخطاء ممنوعة / تحذيرات
- اختراع أرقام تنظيمية أو بيانات مصنّع أو تصنيفات حريق/صوت أو قدرات إنشائية.
- تحريك Internal Origin لحل التنسيق.
- ترقيع الأعراض بـ view overrides بدل إصلاح المصدر.
- الاعتماد على الـ viewport بدل فحص الـ PDF الفعلي.
- أتمتة قرارات الحكم الهندسي أو التعديلات التدميرية الواسعة دون تحديد الأهداف والمعاينة وحفظ حالة قابلة للاستعادة.
- إعلان الإنجاز قبل نجاح كل البوابات المنطبقة أو سرد المتبقي صراحة.

## مخرجات متوقعة
- نموذج منسق ومجموعة لوحات كاملة (cover, index, notes, plans بأنواعها, RCP, roof, elevations, sections, enlarged, stairs, wet areas, schedules, wall types, details, legends) و PDF مُراجَع.
- سجلات: assumptions register، authority/code register، issue register (P1–P4)، outstanding issues.
- حالة QA نهائية: PASS / PASS WITH COMMENTS / FAIL / TO BE VERIFIED.

## ملفات مرجعية داخل الـ skill
**الجذر:** `README.md` (التثبيت، فلسفة العمل، إضافات V1.1) — `CHANGELOG.md` (V1.0 الأساس، V1.1 طبقة الإنتاج والتسليم) — `SOURCES.md` (Revit 2027 help، sbc.gov.sa، SBC system/Executive Regulations PDFs) — `MANIFEST.json` (v1.1، 40 ملفًا، قائمة القدرات).

**references/ — النمذجة والتصميم:**
- `01_REVIT_MODELING_STANDARD` — levels/grids/walls/floors/roofs/openings/stairs/rooms/families/materials، model health، coordinates.
- `02_ARCHITECTURAL_TECHNICAL_STANDARD` — planning، envelope، wet areas، vertical circulation، accessibility (لا اختراع قيم)، maintainability، constructability.
- `07_FAMILY_CREATION_STANDARD` — أسئلة ما قبل الإنشاء، الهندسة، parameters، الاختبار.
- `11_RESIDENTIAL_VILLA_STANDARD` — مراجعة تخطيط الفيلا، مخاطرها، مراجعة دور بدور (10 خطوات)، الحد الأدنى للمخرجات.

**references/ — التوثيق والإنتاج:**
- `03_CONSTRUCTION_DOCUMENTATION` — هرمية الرسم، view control، sheet/plot/schedule/dimension QA.
- `13_DRAWING_PRODUCTION_ENGINE` — تسلسل إنتاج من 22 خطوة، أنواع اللوحات، view purpose، scale/template/crop strategy، graphics، sheet composition/naming، index.
- `14_ANNOTATION_ENGINE` — هرمية التعليق، text، dimensions، tags (room/door/window)، levels، markers، callouts، keynotes، collision control.
- `15_SHEETS_SCHEDULES_AND_ISSUE` — بنية الشيت، الجداول، schedule QA، revisions، issue states، final issue.
- `16_PLOT_PDF_ENGINE` — plot setup، PDF QA بثلاث تكبيرات، أعطال شائعة، file naming، release gate.

**references/ — التفاصيل والتنسيق والجودة:**
- `04_SHOP_DRAWING_STANDARD` — التفكير المطلوب لكل بند، حزم shop drawings النموذجية، فصل المصادر.
- `08_DETAILING_STANDARD` — اختيار التفاصيل، الهرمية، التعليق، قائمة «لا تخترع».
- `05_BIM_COORDINATION_STANDARD` — تسلسل التنسيق، severity P1–P4، سجل المشكلة، الإغلاق.
- `06_QA_QC_STANDARD` — 6 بوابات وصيغ النتيجة.
- `12_REVIEW_METHOD` — المراجعة بثلاث تمريرات وأسئلة التحدي.

**references/ — السعودية والهوية:**
- `09_SAUDI_BUILDING_REQUIREMENTS` — إطار مُتحكم بالمصادر، هرمية السلطات، التحقق الإلزامي.
- `10_RIYADH_LOCAL_ARCHITECTURE` — أبعاد تقييم الهوية المحلية.

**references/ — البيانات والأتمتة:**
- `17_DYNAMO_AUTOMATION_AND_TOOLING` — المرشحات الجيدة/السيئة، عقد الأتمتة، السلامة، الدفعات، الأداء.
- `18_BIM_DELIVERABLE_DATA_STANDARD` — فئات المعلومات، انضباط الـ parameters، shared parameters، data QA.
- `19_MODEL_HANDOVER_AND_COBIE_MINDSET` — asset info، handover checks، COBie.

**workflows/:** `01_NEW_PROJECT` (intake → requirements → setup → base model → QA gate) — `02_VILLA_PROJECT` (15 خطوة من الموقع إلى PDF) — `03_ARCHITECTURAL_MODELING` (9 مراحل؛ لا تفصيل ثانوي قبل استقرار الأساسي) — `04_CONSTRUCTION_DOCUMENTS` (15 خطوة) — `05_BIM_COORDINATION` (13 خطوة) — `06_FINAL_QA_QC` (11 فحصًا بالترتيب) — `07_DRAWING_PRODUCTION` (6 بوابات: register, views, annotation, sheets, cross-reference, PDF) — `08_ANNOTATION_QA` (11 بندًا ثم فحص التداخل) — `09_SHEET_ISSUE` (14 خطوة حتى الإصدار).

**checklists/:** `01_MODEL` — `02_DOCUMENTATION` — `03_COORDINATION` — `04_FINAL_ISSUE` — `05_ANNOTATION` — `06_SHEET` — `07_PDF` (مدمجة أعلاه في قسم QA).

## مهارات مرتبطة
- لا يذكر المصدر مهارات أخرى بالاسم.
