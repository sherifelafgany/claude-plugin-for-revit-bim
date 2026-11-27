# revit-api-safety — أمان التعامل مع Revit API

> **الغرض:** تجنب انهيار Revit والنوافذ المنبثقة (modal dialogs) والكتابة على مستند خاطئ. **متى تُستخدم:** قبل إرسال أي كتابة أو قراءة ثقيلة إلى Revit حي عبر Revit MCP / API.

## الفكرة الأساسية
كل كتابة تمر عبر غلاف (wrapper) موحّد داخل Idling-job بـ Transaction ومعالج أخطاء ومعالج نوافذ، مع dry run أولًا، دفعات صغيرة، حراسة بعنوان المستند، وإدارة ذاكرة صارمة. الدروس من انهيارين (2026-10-19) ومن مشروع WEDAD.

## القواعد والأرقام الأساسية
**أساسيات**
- كل كتابة = Idling-job: Transaction خاصة + IFailuresPreprocessor + SetForcedModalHandling(false). Dry run ثم commit (Assimilate للمجموعات). سجّل كل job في ملف على الجهاز.
- لا popups أبدًا (modal + استدعاءات إضافية = انهيار مرتين).
- **ممنوع** استدعاء say_hello / اختبار الاتصال (فتح "HELLO MCP" وعطّل Revit ساعة، 2026-10-20). افحص الاتصال بقراءة صامتة (doc.Title).
- بداية كل جلسة: سجّل مرة واحدة (idempotent) معالج UIApplication.DialogBoxShowing يغلق النوافذ بالزر الآمن/الافتراضي ويسجل id + النص، ومعالج FailuresProcessing يحذف التحذيرات ويعمل rollback للأخطاء فقط.
- نافذة حاجبة: جلسة الجهاز تغلق نوافذ Revit بنفسها (بإذن شريف)؛ لا تغلق النافذة الرئيسية ولا تتجاهل عملًا غير متزامن.
- عائلة واحدة / نوع واحد لكل استدعاء؛ دفعات صغيرة.
- بعد timeout: لا استدعاء جديد؛ انتظر وافحص الـ log/ملف النتيجة.
- احرس كل كتابة بـ doc.Title (حدّثه عند انتقال النموذج، مثل ACC).
- تعديل العائلات: preprocessor داخل مستند العائلة + IFamilyLoadOptions؛ لا rollback لمجموعة بعد LoadFamily.
- حدود API: لا نقل لرؤوس الـ callout، لا تعديل داخل المجموعات، لا إنشاء family labels.
- Workshared/ACC: Reload Latest قبل كل دفعة، sync بعدها، كل تخصص على workset خاص؛ محادثة واحدة تقود Revit في كل وقت.
- عدّ العناصر والتعليقات قبل/بعد؛ لا حذف بدون سرد. لا خطوات يدوية لشريف.

**الغلاف والذاكرة (WEDAD)**
- الغلاف الموحّد: FailuresPreprocessor، SetClearAfterRollback(true)، معالج نوافذ (dismiss + log + stop)، stopwatch 45 s. الـ DRY يمر بنفس المسار عبر أخطاء وقت الـ commit ويطبعها. ارفض أي job بها Transaction خارج الغلاف أو استدعاء dialog.
- كل compile يترك 0.2-0.5 GB: أنهِ كل job/loader بـ `D = null; GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();`.
- ذاكرة حرة ≥ 2.5 GB قبل الإرسال (استدعاء GC-only أولًا إن أقل؛ توقف إن < 1.6 بعد GC). PNG شيت واحد لكل إرسال؛ PDF 1-5 شيتات عبر Document.Export (لا PrintManager)؛ الـ sync إرسال مستقل. أعد تشغيل Revit فقط إن ذاكرته الخاصة > 6 GB بعد GC.
- لا job في الطابور يعمل دون مراقبة وشريف على الجهاز.
- لا تُعِد تسمية أو تمسح STOP / guard log لتمرير إعادة إرسال؛ إعادة الإرسال = ملف مختوم جديد باسم log جديد.
- device timeout (relay 60-80 s): الاستدعاء قد يكون جاريًا أو انتهى أو لم يبدأ ← اقرأ الـ log / IsModified / عدد الحفظ أولًا؛ تحقق من بطاقة موافقة معلقة أو popup.
- بعد إعادة كتابة ملف على network mount: sha256 لنسخة الجهاز قبل compile/send.

**Worksets**: العناصر المنشأة عبر API تذهب للـ workset النشط (84 عنصر ARCH ذهبت مرة لـ PLUMBING). كل job يضبط الـ workset النشط لتخصصه ويستعيده، ويضبط ELEM_PARTITION_PARAM لكل عنصر جديد (بما فيه Create.NewOpening، CopyElement، والعنصر المخفي من WallType.Duplicate). المراجع يتحقق بالـ id. المواسير تُحكم بـ SYSTEM NAME (condensate / refrigerant = HVAC).

**Bridge / compile (Revit 2026، قالب .NET 8 MCP)**
- C# 5 فقط (لا `$""`، `?.`، `is T x`)؛ `ElementId.Value`.
- اكتب `Autodesk.Revit.DB.Plumbing.PipeSystemType` و`Autodesk.Revit.DB.Mechanical.MechanicalFitting` بالاسم الكامل، أو قارن `c.PipeSystemType.ToString()`.
- لا compiler في الحاوية: ابحث (grep) في القالب عن أسماء مشابهة قبل إضافة متغير محلي (local `G` حجب `G()`). "target of an invocation" يخفي الأخطاء: افحص InnerException.
- قالب send_code يحمل Transaction أصلًا: الـ probes بـ Transaction خاصة تفشل؛ لفّها في Idling. القراءة المباشرة أثناء Idling job تنتهي بـ timeout: راقب ملف الـ log.
- `new FilteredElementCollector(doc)` بلا فلتر يرمي استثناء؛ استخدم LogicalOrFilter(ElementIsElementTypeFilter(false), ElementIsElementTypeFilter(true)).

**وضع MEP**
- NewTeeFitting لا يصنع wye 45°: ضعه بـ NewFamilyInstance ووصّل يدويًا؛ الفرع بزاوية 45° بالضبط على الـ main المائل. Sanitary tees: main على استقامة (kink < 0.01°). NewTransitionFitting(pipeConnector, fittingConnector): الـ pipe أولًا. مسافة = take-off ← "Line is too short".
- الأكواع fitting-to-fitting: NewFamilyInstance، اضبط Angle، طابق الإطار، انقل المنفذ على المنفذ، ConnectTo.
- Revit قد يضيف fittings عند الـ COMMIT لم يظهرها الـ DRY: ضع الـ transitions صراحة واقرأ كل id جديد بعد آخر id للـ job.
- العائلات المعتمدة على work-plane (roof drains) والـ floor drains: ضعها على SketchPlane عند z المطلوب (انتبه لإزاحة الـ connector). تحريك host ref plane يسحب الشبكة: افصل أولًا.
- تدوير جهاز يسحب أطراف المواسير: افصل، دوّر حول أصل connector الصرف، أعد الوصل، قارن الأطراف.
- البوابات على SOLID extents لا bboxes.

**تعديل الهندسة**
- Edit Profile مسموح (Sherif 2026-11-25): احذف المقاطع < 1 cm، افحص كل sketch هندسيًا، لا خطوط متقاطعة (سببت انهيار).
- تقصير جدار متصل: WallUtils.DisallowWallJoinAtEnd أولًا. بعد SketchEditScope.Commit لا Regenerate خارج Transaction. درابزين الدرج: Railing.SetPath لا يعمل ← أعد الإنشاء بـ Railing.Create + HostId = stair.
- نسخ CopyElement تحتفظ بـ Type Mark المصدر: عيّن واحدًا جديدًا.

## خطوات العمل (Workflow)
1. تحقق من الاتصال بقراءة صامتة (doc.Title)؛ سجّل معالجات النوافذ/الأخطاء مرة.
2. افحص الذاكرة الحرة (≥ 2.5 GB) وReload Latest.
3. جرد العناصر قبل الكتابة.
4. أرسل DRY عبر الغلاف، راجع الأخطاء، ثم commit.
5. اضبط/استعد الـ workset؛ اقرأ الـ ids الجديدة وتحقق منها.
6. GC، sync كإرسال مستقل، جرد بعد الكتابة، سجّل في الـ log.

## نقاط الفحص / QA Checklist
- [ ] لا say_hello ولا dialogs
- [ ] doc.Title guard صحيح
- [ ] الذاكرة ≥ 2.5 GB قبل الإرسال
- [ ] DRY عبر نفس مسار الغلاف قبل الـ commit
- [ ] كل عنصر جديد على workset تخصصه (ELEM_PARTITION_PARAM)
- [ ] العدد قبل/بعد مسجل؛ لا حذف بدون سرد
- [ ] بعد timeout: قراءة الـ log قبل أي استدعاء

## أخطاء ممنوعة / تحذيرات
- استدعاء جديد بعد timeout.
- Transaction خارج الغلاف.
- مسح/إعادة تسمية STOP log.
- خطوط sketch متقاطعة أو مقاطع < 1 cm.
- PrintManager للتصدير.
- إغلاق نافذة Revit الرئيسية أو التخلي عن عمل غير متزامن.
- مطالبة شريف بإغلاق النوافذ أو بخطوات يدوية.

## مخرجات متوقعة
- jobs آمنة موثقة بـ log على الجهاز، جرد قبل/بعد، عناصر على worksets صحيحة، نموذج متزامن.

