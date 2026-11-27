# mohammed-ballaswad-revit-to-cad-professional-export — تصدير احترافي من Revit إلى DWG

> **الغرض:** إنتاج ملفات DWG نظيفة ومنظمة الطبقات وقابلة للتحرير وصحيحة المقياس من Revit عبر سير عمل ذي بوابات. **متى تُستخدم:** عند طلب تصدير CAD من Revit، مع التحضير والتصدير المضبوط والفحص/التنظيف الفعلي والمقارنة البصرية وحالة إصدار مبنية على الأدلة.

## الفكرة الأساسية
نجاح أمر Export ≠ جاهزية CAD احترافية (المؤلف: Mohammed Ballaswad | محمد بلسود، الإصدار 2.0.0). الملف لا يكون `CAD-READY` إلا بعد اجتياز بوابات المصدر والتصدير وسلامة CAD والتهشير/الخطوط والتعليقات والمقارنة البصرية. إذا تعذّر الفحص → `EXPORT COMPLETE — QA PARTIAL`. المهارة لا تمنح قدرات AutoCAD غير متصلة.

## القواعد والأرقام الأساسية

**مبادئ التشغيل:**
1. اكتشاف القدرات أولاً: هل يمكن التصدير، فحص كيانات DWG، تعديلها، فتح/حفظ في AutoCAD، المقارنة البصرية. لا ادعاء أوامر AutoCAD عبر MCP بلا دليل.
2. حماية المصدر: قراءة فقط افتراضياً؛ views تصدير ونسخ DWG جديدة بموافقة؛ لا كتابة فوق النموذج أو DWG المصدر.
3. فصل الدقة البصرية عن قابلية التحرير (قد تتعارضان) — الاختيار حسب المتطلب واختبار عينة.
4. معالج CAD حقيقي للوصل/إزالة التكرار/إصلاح التهشير/تنظيف الطبقات/audit.
5. لا تنظيف تدميري شامل (explode/حذف خطوط قصيرة/flatten/duplicate cleanup) دون معاينة وتسامح وموافقة ونسخة قابلة للاسترجاع.
6. حفظ نية التصميم: لا تغيير أبعاد أو حدود أو تسلسل خطوط أو معنى تهشير أو إحداثيات.
7. كل فحص بدليل (نتيجة أداة، إعادة فتح، مقارنة قابلة للقياس، تقرير كيانات/طبقات، معاينة) وإلا `NOT CHECKED`.
8. profile معتمد لكل غرض؛ لا تعديل الإعدادات العامة للمستخدم دون موافقة.
9. تسليم مسارات مخرجات متحقق منها + ملخص profile + تقرير QA + استثناءات + حالة.
10. لا اختراع معايير: معيار CAD للعميل إن وُجد، وإلا baseline المكتب كمقترح معلّم.

**الأوضاع (التسلسل الافتراضي):** `AUDIT-ONLY → PREPARE-EXPORT → EXPORT-APPROVED → CAD-QA → CAD-CLEANUP-APPROVED (فقط عند الحاجة والدعم) → VERIFY-EXPORT → PACKAGE-DELIVERY`.

**تحديد الهدف قبل الإعدادات:** IDs وأسماء views/sheets والإصدار؛ النوع (plan/section/elevation/detail/sheet/3D)؛ model space / paper space؛ DWG لكل لوحة أم مدمج؛ إصدار AutoCAD المستهدف؛ الوحدات وبُعد مرجعي؛ أساس الإحداثيات (internal vs shared) والأصل؛ معيار الطبقات، CTB/STB، lineweights، linetypes؛ سياسة xref؛ مكتبة التهشير والخطوط؛ النص العربي RTL؛ الغرض (permit، tender، construction، shop drawing، coordination، reference).

**تحضير view المصدر:** view template، scale، detail level، discipline، phase، design options، crop/annotation crop؛ view range/underlay؛ far clip/depth؛ visibility/filters/overrides/halftone؛ الأبعاد والتاغات والنصوص وfilled/masking regions وrevision clouds؛ links/imports وnested families؛ المحتوى المخفي/التحليلي/المرجعي. view مخصص بالاسم: `CAD-EXPORT | <sheet/view number> | <description>` — بموافقة فقط؛ لا تعديل view الإنتاج للتصدير.

**Export profiles (توصيات تسمية — لا تُدّعى موجودة حتى تُنشأ):**

| Profile | الغرض | أبرز الإعدادات |
|---|---|---|
| `MB-CAD-SHEET` | تخطيط اللوحة/title block/viewports | تحقق viewport scale وpaper space، سياسة links كـ xrefs، أولوية الدقة البصرية |
| `MB-CAD-MODELSPACE` | تحرير/تنسيق في model space | تصدير views منفردة، وحدات وأصل وبُعد مرجعي، layer mapping حسب category/subcategory، تقليل التعليقات |
| `MB-CAD-DETAIL` | مقاطع/واجهات/تفاصيل | تسلسل cut/projection، اختبار التهشير، lineweight/font/hidden-line، تصدير عينة أولاً للـ view الكثيف |

سجل كل profile: target DWG version، output type، units، coordinate basis، layer standard، line mapping، pattern/PAT file، font policy، link policy، views/sheets، المعتمِد.

**ربط الطبقات المقترح (layer-mapping.csv):**

| Revit | Layer | | Revit | Layer |
|---|---|---|---|---|
| Walls | A-WALL | | Grids | A-GRID (Confirm) |
| Doors | A-DOOR | | Levels | A-LEVEL (Confirm) |
| Windows | A-WIND | | Structural Columns | S-COLS |
| Curtain Panels | A-GLAZ | | Structural Framing | S-BEAM |
| Floors | A-FLOR | | Structural Floors | S-SLAB |
| Ceilings | A-CEIL | | Ducts | M-DUCT |
| Roofs | A-ROOF | | Pipes | M-PIPE |
| Stairs | A-STAIR | | Lighting Fixtures | E-LITE |
| Furniture | A-FURN | | Electrical Fixtures | E-POWR |
| Dimensions | A-DIMS | | Filled Regions/Hatches | A-HATCH |
| Text | A-TEXT (تحقق RTL) | | Tags | A-ANNO |

- العناصر غير المربوطة على `A-UNMAPPED-QA` (Plot: No until resolved) — لا تُسلّم دون إفصاح.

**الخطوط والتهشير والنصوص والوحدات:**
- Revit قد يدمج خطوطاً متداخلة/على استقامة واحدة؛ لا افتراض أن كل تجزئة خطأ تصدير.
- التهشير: تحقق من مسار PAT وتوفره، Drafting vs Model pattern، اختبار على مقياس تمثيلي؛ لا رفع حدود التهشير العامة أو استبدال أنماط عشوائياً؛ hatch صالح يحتاج حدوداً مغلقة وislands صحيحة.
- الخطوط: map بعد تأكيد توفرها لدى المستلم ودعم العربية/RTL. لا تحويل كل النصوص/الأبعاد إلى خطوط مفككة إلا بطلب المستلم.
- الوحدات: ضبط صريح لوحدات DWG والإحداثيات؛ تحقق من بُعد مرجعي معروف؛ shared coordinates فقط للتنسيق مع نماذج أخرى؛ فحص insertion scale، extents، origin، UCS/WCS، قيم Z غير المقصودة.

**استراتيجية التصدير:** Sheet DWG (title block وviewports) · View/model-space DWG (للتحرير) · ملفات منفصلة حسب التخصص/النوع · 3D DWG فقط بطلب صريح.
- تسمية الملف: `<ProjectCode>_<SheetOrView>_<Description>_DWG_<Revision>.dwg` — والتحقق من وجود المسار.

**المعالجة اللاحقة في CAD (بقدرة تحرير فعلية فقط، على نسخة، مع log):**
- سلامة الملف: audit/repair، الوحدات، extents، UCS/WCS، Z، xrefs، proxy objects، خطوط SHX/TTF، linetypes، أعداد الكيانات قبل/بعد.
- التكرار والوصل: التكرار التام أولاً، ثم near-duplicates بتسامح مبرر بوحدات الرسم؛ مقارنة layer/linetype/elevation/normal/width/topology؛ لا وصل عبر door swings أو الأبعاد أو centerlines أو حدود التهشير أو cut/projection أو طبقات مختلفة.
- لا `OVERKILL` أو `PEDIT JOIN` أو `EXPLODE` أو حذف المقاطع القصيرة كعمليات شاملة غير مراقبة.
- التهشير: فحص pattern، scale، angle، origin، الحدود، islands؛ حفظ الحد الأصلي على طبقة QA؛ إعادة البناء فقط لحد واضح وبموافقة؛ لا استبدال أنماط الخرسانة/التربة/العزل/التشطيب الحرجة بنمط عام.
- Blocks/xrefs: الحفاظ على الـ blocks والسمات؛ لا bind/unbind أو explode إلا بمتطلب التسليم؛ الحواف المضلعة من 3D تُعالج من المصدر/الإعدادات لا بـ join شامل.

**بوابات QA الإلزامية (كل فحص `PASS`/`FAIL`/`NOT CHECKED` بدليل؛ أي FAIL حرج يمنع CAD-READY):**
- **A — Source:** view/sheet، إصدار، نطاق، crop، visibility، scale، تعليقات.
- **B — Export:** الملفات، إصدار DWG، الوحدات، الإحداثيات، الطبقات، النص/الخط، ربط الخطوط والتهشير.
- **C — CAD integrity:** يفتح، لا فساد حرج، الاعتماديات معروفة، المقياس مقابل بُعد مرجعي.
- **D — Linework & hatch:** مخاطر التكرار/التجزئة، التهشير والحدود، لا تنظيف تدميري.
- **E — Visual & semantic fidelity:** مقارنة DWG/plot preview بـ Revit على نفس المقياس.
- **F — Editability & handoff:** طبقات ذات معنى، كيانات قابلة للتحرير، اعتماديات موثقة.

**حالات الإصدار:**
- `DRAFT` — إعداد أو تصدير تجريبي غير مفحوص بالكامل.
- `EXPORT COMPLETE — QA PARTIAL` — صُدّر لكن فحص مطلوب أو أكثر لم يُنفذ.
- `CAD-QA VERIFIED` — فُتح وفُحص وكل الفحوص المنطبقة نجحت، وقد يبقى اعتماد المستلم.
- `CAD-READY FOR USE` — كل البوابات ناجحة، أُعيد فتحه بعد آخر تعديل، ومقارنة بصرية تمت. ممنوع دون فحص كيانات ومقارنة بصرية.
- تسجيل الدقة البصرية وقابلية التحرير كلٌّ على حدة.

## خطوات العمل (Workflow)
1. اكتشاف النموذج النشط وإصدار Revit والأدوات الفعلية؛ تعبئة `capability-ledger.csv`.
2. تحديد هل يمكن فحص/تحرير DWG لا تصديره فقط.
3. تعريف نوع المخرج، الوحدات، الإحداثيات، معيار الطبقات، الخطوط، مكتبة التهشير، غرض المستلم (سؤال واحد موجز فقط إذا غيّر القرار المخرج جوهرياً).
4. فحص view/sheet المصدر قبل التصدير.
5. تحضير view/profile مخصص بموافقة.
6. تصدير عينة تمثيلية أولاً للـ views الكثيفة والتهشير والمقاطع والنص العربي.
7. مراجعة العينة في برنامج CAD (مقياس، خطوط، تهشير، تعليقات، طبقات).
8. ضبط الـ profile أو الـ view بناءً على الأدلة.
9. تصدير نهائي إلى مجموعة ملفات جديدة.
10. CAD QA وتنظيف معتمد على نسخ فقط.
11. إعادة الفتح والفحص والمقارنة بـ Revit.
12. تقرير QA، الاستثناءات، حالة إصدار صادقة.

**تشخيص التهشير/الخطوط:** فحص المصدر على المقياس ← تصدير view صغير ← فحص DWG ← المقارنة: عيب في المصدر → إصلاح view/material/family؛ عيب من التصدير → اختبار export setup؛ عيب بعد تفسير CAD → فحص font/PAT/layer/dependency.
- Missing hatch: mapping ومسار/إصدار PAT وتحذيرات التصدير. Overdense: scale/units ومقياس الرسم. Solid fill: mapping والخلفية. حد مفتوح: gaps، overlaps، arcs، مقاطع صغيرة. زاوية/أصل خاطئ: نوع النمط والإحداثيات.

**ترتيب التنظيف الآمن:** فتح وaudit ← حل الاعتماديات دون مس الهندسة ← الوحدات والإحداثيات ← تحليل التكرار (تقرير قبل الحذف) ← مرشحات الوصل ← حدود التهشير ← الطبقات ← حفظ كإصدار جديد ← إعادة الفتح ومقارنة baseline. (قبلها: نسخة عمل، الإبقاء على DWG الخام، baseline بحجم الملف وأعداد الكيانات/الطبقات والـ extents.)

**إذا لم يكن AutoCAD متصلاً:** تحضير المصدر والإعدادات ضمن القدرة؛ لا ادعاء إصلاح/وصل/إزالة تكرار/audit؛ إصدار `EXPORT COMPLETE — QA PARTIAL`؛ تقديم checklist تسليم لـ AutoCAD أو مقترح أتمتة ينفذه المستخدم بموافقة. **السكربت لا يُعد منفذاً لمجرد توليده.**

## نقاط الفحص / QA Checklist
- [ ] النموذج والـ view/sheet والـ phase والإصدار صحيحة.
- [ ] crop وview range/depth وdetail level وvisibility والتعليقات فُحصت.
- [ ] إصدار DWG ونوع التصدير صحيحان؛ الوحدات والإحداثيات صحيحة.
- [ ] بُعد مرجعي معروف يطابق المقياس؛ المسار موجود.
- [ ] DWG يفتح في أداة CAD حقيقية؛ extents وorigin وUCS/WCS وZ فُحصت.
- [ ] الطبقات ذات معنى وكل طبقة غير متوقعة مفسّرة.
- [ ] xrefs والخطوط وlinetypes والتهشير وproxy objects فُحصت؛ أعداد الكيانات/الطبقات ملتقطة.
- [ ] التكرار والتجزئة فُحصا بتسامح محدد؛ لا explode/join/delete شامل غير مراجع.
- [ ] تعريفات الأنماط ومسار PAT مؤكدة؛ scale/angle/origin والكثافة مناسبة؛ الحدود مغلقة؛ لا solid fills غير متوقعة؛ أي hatch معاد بناؤه معتمد وقابل للاسترجاع.
- [ ] النص العربي RTL واللاتيني سليمان؛ بدائل الخطوط موثقة.
- [ ] الأبعاد والأسهم والـ leaders والدقة والوحدات فُحصت؛ lineweights/linetypes/colors/CTB-STB.
- [ ] مقارنة PDF/plot preview مع Revit على نفس المقياس عند اللزوم.
- [ ] كل الإخفاقات الحرجة محلولة، الاستثناءات مُفصح عنها، الحالة وفق التعريفات.

## أخطاء ممنوعة / تحذيرات
- اعتبار نجاح أمر Export دليلاً على الجاهزية الاحترافية.
- الكتابة فوق نموذج Revit أو DWG المصدر؛ تعديل view الإنتاج أو الإعدادات العامة دون موافقة.
- عمليات شاملة غير مراقبة: explode كل blocks/hatches، حذف كل الخطوط القصيرة، join كل خطوط طبقة، flatten/Z=0 دون إثبات أن الرسم 2D، duplicate removal بتسامح غير مراجع، استبدال كل تهشير بـ solid أو نمط عام، bind/unbind كل xrefs، تحويل النصوص/الأبعاد لخطوط.
- رفع حدود التهشير العامة دون دليل؛ join شامل لحواف 3D مضلعة.
- اختراع مسار ملف أو اسم أداة أو معيار CAD.
- اعتبار وثائق Autodesk دليلاً على اجتياز مخرج معين للـ QA.

## مخرجات متوقعة
1. DWG(s) متحقق منها (DXF اختياري عند الطلب فقط).
2. نموذج/view/sheet المصدر وإصداره.
3. اسم export profile وإعداداته الأساسية.
4. ربط الطبقات.
5. تقرير QA بالأدلة.
6. الاستثناءات والفحوص غير المتحقق منها.
7. PDF preview اختياري (فقط إذا وُلّد وقورن).
8. حالة الإصدار والإجراء المتبقي. هوية المالك في التقارير: `Mohammed Ballaswad | محمد بلسود`.

## ملفات مرجعية داخل الـ skill
- `references/export-profile.md` — profiles الثلاثة والسجل المطلوب.
- `references/layer-mapping.csv` — baseline الطبقات المقترح.
- `references/qa-checklist.md` — قائمة فحص CAD-ready.
- `references/release-status.md` — تعريفات الحالات.
- `references/autodesk-reference-notes.md` — روابط Autodesk الرسمية (Export to DWG/DXF 2026، Modify Export Setup، Patterns، Text and Fonts، Units and Coordinates، دعم missing hatch patterns 2026-08-04) كإرشاد فقط.
- `references/capability-ledger.csv` — سجل الأدوات المتحقق منها.
- `workflows/export-workflow.md` (12 خطوة)، `workflows/hatch-and-linework-repair.md` (تشخيص + سجل العيب: view، layer، entity types، defect، probable source، evidence، test، approved action، before/after، remaining issue)، `workflows/cad-cleanup-safety.md`.
- `templates/` — `export-register.csv`، `export-profile-record.csv`، `layer-audit.csv`، `qa-report.csv` (check_id، gate، result، evidence، impact، action_taken، residual_issue، reviewer، date).
- `tests/acceptance-tests.md` — 12 اختبار قبول (capability honesty، source readiness، export setup، scale/coordinates، hatch، layers، fragmentation، Arabic text، post-processing safety، visual fidelity، reopen/delivery، release status).
- `README_AR.md` (أمر تشغيل عربي موصى به)، `CHANGELOG.md`، `MANIFEST.json` (live tests غير منفذة بعد: `live_revit_mcp_test: false`).

## مهارات مرتبطة
*(استنتاجاً — المصدر لا يسرد مهارات مرتبطة صراحةً)*
revit-api-safety، revit-boq-engineering، revit-sheet-methodology، architectural-drawing-style، output-quality-control.
