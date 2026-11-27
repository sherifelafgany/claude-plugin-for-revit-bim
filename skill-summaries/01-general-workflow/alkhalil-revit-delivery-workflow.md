# alkhalil-revit-delivery-workflow — سير عمل تسليم مشاريع الخليل على Revit

> **الغرض:** قواعد العمل لمشاريع ALKHALIL عبر Revit MCP: ردود عربية، title block معتمد، خط إنتاج الشيتات/PDF، المراجعات، التنسيق والجودة. **متى تُستخدم:** لأي عمل إنتاج لوحات في مشروع ALKHALIL على Revit (معماري، إنشائي، MEP، عرض).

## الفكرة الأساسية
هوية مكتب ثابتة + title block معتمد (معيار HOUSE 93) + مجموعة شيتات قياسية، مع ممارسات تقنية لتجاوز حدود Revit API/MCP. يُستخدم مع bim-revit-engineering وalkhalil-presentation-deck.

## القواعد والأرقام الأساسية
**التواصل**: عربي قصير مباشر؛ ما تم أولًا ثم ما يحتاج قراره. عند تعليم موضع: ابحث عن السبب الجذري وصححه في كل مكان واذكر السبب. بعد تعديل النموذج ذكّر بـ Ctrl+S؛ لا تدّعِ الحفظ.

**هوية المكتب**: الخليل للهندسة المعمارية / ALKHALIL Architectural Engineering، خط Cairo. رسم واحد لكل شيت؛ الجداول داخل الإطار؛ أبعاد على الواجهات والقطاعات؛ شيتات setting-out بأبعاد تنفيذية؛ مقاسات الغرف length x width.

**Title block المعتمد — `ALKHALIL - A1 Title Block.rfa`**
- نسخ في `Desktop\BIM\` و`_claude_revit\titleblock\`. A1 841x594، الإطار 9..831 x 9..584، الشريط الأيمن x 724-831.
- الشريط من أعلى: الشعار، شريط أسود لاسم المشروع (نص أبيض 254,254,254 — الأبيض النقي يُطبع أسود)، المالك/المشروع، صورة key plan، عنوان الرسم، رقم الشيت، المقياس، التاريخ، جدول المراجعات، خانات الاستشاري/الختم.
- البناء: عدّل "A1 metric"؛ الـ API لا ينشئ labels ← انقل/أعد تنسيق الموجودة (VerticalAlignment Top). Labels فوق الشريط الأسود تُنسخ بعد إنشاء الـ filled region (لا draw order في عائلات 2D).
- FilledRegion لا يُنشأ على الشيتات: الـ key plan = PNG بـ System.Drawing يوضع كـ ImageInstance.
- إعادة تحميل العائلة: IFamilyLoadOptions عبر System.Reflection.Emit (OnFamilyFound/OnSharedFamilyFound = true، overwrite).

**مجموعة الشيتات**: A-000 غلاف · A-100 موقع · A-101 أثاث · A-102 setting out · A-103 سطح · A-104 RCP · A-201..204 واجهات · A-301/302 قطاعات · A-601.. جداول (صحي، أثاث، مطبخ، إنارة) · M-101.. HVAC (أرضي، سطح، تنسيق، 3D) · PR-xx عرض. الدمج بـ pypdf إلى `"<Project> - Working Drawings - RevNN.pdf"`.

**ممارسات Revit MCP**
- مهلة 60 s: قسّم العمل الثقيل، استدعاءات idempotent، وسم نصوص الإرجاع، أعد فحص الحالة بعد الـ timeout.
- الوحدات F=1/0.3048، MM=304.8؛ أكّد الأصل من عناصر قائمة.
- تدوير crop المسقط: اعثر على عنصر الـ crop بمقارنة عناصر الـ view مع CropBoxVisible true/false، RotateElement بـ +π/2، CropBox عبر Transform.Inverse.
- ViewSection.CreateSection: ينظر باتجاه +BasisZ من Min.Z؛ BasisX = Z × d، BasisY = Z، BasisZ = d؛ مدى X معكوس (Min.X=-x2، Max.X=-x1)، Z من 0..depth.
- سلاسل الأبعاد: خطوط detail قصيرة في `<Invisible lines>` + NewDimension، النوع "AK Linear m 2dp".
- نقل الجدران عبر location curve ثم إعادة الأبواب/الشبابيك المستضافة؛ تحديث النصوص الثابتة وwitness lines بعد تغيّر الهندسة.
- الملفات: device_stage_files / device_commit_files (force عند الاستبدال)؛ التصدير بـ PDFExportOptions وExportImage.

**قواعد مراجعة HVAC**: لا هواء إمداد مباشر على الأسرّة/الجلسات؛ linear slot diffusers في gypsum tray upstands تقذف على السقف؛ returns مُدكّتة في غرف النوم مع access panels؛ لغة grilles واحدة لكل فراغ مفتوح (لا خلط رأسي وأفقي)؛ شفاطات لكل الغرف الرطبة بما فيها الغسيل؛ كل وحدة داخلية لها وحدة خارجية على سطح.

## خطوات العمل (Workflow)
1. اعمل ضمن الـ title block المعتمد ومجموعة الشيتات القياسية.
2. نفّذ عبر MCP بدفعات صغيرة idempotent مع فحص الحالة.
3. عند تغيّر المحتوى بعد الإصدار: ارفع رقم المراجعة في الـ title block والجدول والتذييل واسم الملف.
4. صدّر الأجزاء وادمجها بـ pypdf إلى ملف RevNN واحد.
5. حوّل كل شيت معدّل إلى صورة وانظر إليه قبل التسليم.

## نقاط الفحص / QA Checklist
- [ ] رقم المراجعة محدث في كل الأماكن الأربعة
- [ ] المعماري/الإنشائي/MEP متفقة، وأعيد التصدير بعد التغييرات
- [ ] كل شيت معدّل فُحص كصورة
- [ ] الافتراضات المفتوحة مذكورة بصدق (TBC للمالك/الرقم، الشمال، تفاصيل ناقصة)

## أخطاء ممنوعة / تحذيرات
- ادعاء الحفظ (ذكّر بـ Ctrl+S فقط).
- نص أبيض نقي 255 على الشريط الأسود.
- محاولة إنشاء labels أو FilledRegion على الشيت عبر الـ API.

## مخرجات متوقعة
- شيتات A-/M-/PR- على الـ title block المعتمد، و`"<Project> - Working Drawings - RevNN.pdf"` مدمج، وقائمة افتراضات مفتوحة.

## مهارات مرتبطة
bim-revit-engineering، alkhalil-presentation-deck.
