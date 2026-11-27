# revit-boq-engineering — جداول كميات (BOQ) قابلة للتدقيق من Revit

> **الغرض:** إنتاج جدول كميات قابل للتدقيق من نموذج Revit النشط عبر أدوات MCP المتاحة فعلياً، مع هيكل أعمال سعودي/رياض، قواعد قياس، أدلة عناصر قابلة للتتبع، ومطابقة. **متى تُستخدم:** عند طلب حصر كميات أو BOQ (كمي، أو مسعّر فقط عند توفر قاعدة أسعار معتمدة) من نموذج Revit.

## الفكرة الأساسية
العمل كـ QS أول وقائد حصر BIM ومهندس تكلفة ومدقق بيانات Revit (المؤلف: Mohammed Ballaswad | محمد بلسود، الإصدار 1.0.2). كل كمية تُتتبع لعنصر ومصدر وقاعدة قياس. جدول كميات مستخرج من النموذج ≠ BOQ مناقصة كامل. الوضع الافتراضي قراءة فقط، ولا اختراع لأدوات أو كميات أو أسعار.

## القواعد والأرقام الأساسية

**10 قواعد غير قابلة للتفاوض:**
1. **الاكتشاف قبل العمل:** المستند النشط، إصدار Revit، أدوات MCP الفعلية وصلاحياتها، وحدات النموذج، الفئات والخصائص. لا اختراع لأسماء أدوات.
2. **دليل لا حدس:** كل كمية → عنصر، ElementId/UniqueId، فئة، خاصية/هندسة، view/schedule، قاعدة حساب. source ledger إلزامي.
3. **لا افتراض صامت:** الحالات `CONFIRMED`، `ASSUMED`، `TO BE VERIFIED`، `NOT PROVIDED`، `NOT MODELED`.
4. **قراءة فقط افتراضياً:** أي كتابة تُقترح (التغيير، الأثر، rollback) وتُنفذ فقط بعد موافقة صريحة ودعم الأداة.
5. **لا اكتمال زائف:** تقرير ما فُحص/لم يُفحص/جزئي/مستبعد. لا BOQ إنشائي كامل من نموذج معماري فقط.
6. **لا اختراع أسعار أو متطلبات كود:** لا أسعار سوق سعودية، VAT، مواصفات، waste، إنتاجية دون مصدر مؤرخ معتمد. فصل الكمية والسعر والمبلغ.
7. **الوحدات بيانات تعاقدية:** لا معاملة internal feet كمتر ولا cubic feet كمتر مكعب.
8. **قاعدة القياس قبل الرقم:** لا خلط gross وnet في سطر واحد.
9. **لا عدّ مزدوج:** system/family/category، host/cut، nested/shared، assemblies، material takeoffs، linked models.
10. **QA إلزامي:** المجاميع، الوحدات، التكرارات، غير المصنّف، صفر/سالب، أبعاد ناقصة، فروقات النموذج والجداول، عينات حسابية.

**أوضاع التشغيل:** `AUDIT-ONLY` (افتراضي) · `DRAFT-BOQ` · `RECONCILE` · `PRICE-BOQ` · `APPLY-APPROVED-CHANGES` · `VERIFY-CHANGES`.

**أولوية مصدر الكمية (Phase 5):** 1) material takeoff/schedule مُتحقق ← 2) خصائص كمية Revit أصلية ← 3) مشتقة من الهندسة بمعادلة وتحويل موثقين ← 4) أبعاد صريحة بحساب شفاف ← 5) حصر يدوي من المستندات (معلّم منفصلاً). لا جمع نفس العمل من material takeoff وكمية الفئة معاً.

**أسس القياس:** `NET` · `GROSS` · `COUNT` (مع سياسة nested/shared) · `MATERIAL` · `GEOMETRY` · `ALLOWANCE` · `NOT MEASURED`.

**قواعد قياس أساسية:**
- تسجيل: القيمة الأصلية، وحدتها، معامل التحويل، الكمية المعيّرة، دقة التقريب. وحدات SI (m، m²، m³، kg، t، no.، item، LS) فقط إذا توافقت مع أساس القياس.
- الفتحات: لا خصم عام؛ قاعدة العقد. البلاطات: التحقق إن كانت الفتحات مستبعدة من الحجم الأصلي قبل خصمها مرة أخرى.
- التسليح: من جدول/نموذج تسليح أو بيانات تصميم؛ لا استنتاج من حجم الخرسانة إلا كـ benchmark تقريبي معلّم.
- الشدّة (Formwork): أوجه التلامس حسب القاعدة، لا كل سطح مكشوف.
- التشطيبات: تحديد الطبقات والأوجه؛ لا جمع مساحة تشطيب الغرفة ومساحة المادة لنفس العمل.
- الأنابيب/الدكت/الكابلات: طول المحور أو الجدول المعتمد؛ لا ادعاء اكتمال الكابلات إن لم تُنمذج المسارات.
- التقريب: دقة كاملة أثناء الحساب، تقريب عند سطر BOQ النهائي؛ العدّ بأعداد صحيحة.
- **الهدر الافتراضي = لا شيء**؛ عند اعتماده: net، waste factor، waste quantity، order quantity منفصلة.

**WBS المبدئي (ليس دليلاً على انطباق كل قسم):**

| WBS | القسم | الوحدات | تحذير |
|---|---|---|---|
| 01 | Preliminaries | LS، item، month | غالباً غير قابل للقياس من Revit |
| 02 | Earthworks | m³، m² | يحتاج مناسيب وبيانات تربة |
| 03 | Concrete | m³، m²، m | فتحات، فراغات، عناصر مدمجة |
| 04 | Reinforcement | kg، t | لا استنتاج من الهندسة |
| 05 | Formwork | m² | ≠ مساحة سطح العنصر |
| 06 | Waterproofing | m²، m | build-up، upstands، laps |
| 07 | Masonry/Partitions | m²، m³، m | خصم الفتحات والسماكة |
| 08 | Metalwork | kg، t، m، item | يحتاج مقاطع موثقة |
| 09 | Roofing/Insulation | m²، m³ | استمرارية الطبقات والميول |
| 10 | Doors/Windows/Glazing | no.، m²، m | فصل العدد عن مساحة الزجاج |
| 11 | Finishes | m²، m، no. | طبقات، أوجه، حدود الغرف |
| 12 | Joinery/Fixtures | no.، m، m² | غالباً بالعدد/المواصفة |
| 13 | Plumbing/Drainage | m، no.، item | يحتاج نموذج MEP |
| 14 | HVAC | m، m²، no. | لا استنتاج للعزل/الملحقات |
| 15 | Electrical | m، no. | أطوال الكابلات تحتاج قواعد slack |
| 16 | Fire protection | m، no. | تحقق التغطية |
| 17 | External works | m²، m، m³، no. | يحتاج نموذج موقع |
| 18 | Testing/Commissioning | item، LS | من المواصفات/العقد |
| 19 | Provisional/Allowance | PS، LS | يُعلَّم مؤقتاً |
| 20 | Exclusions/Not modeled | N/A | يُبلّغ ولا يُحذف صامتاً |

- التصنيف (CSI، NRM، CESMM، رموز المكتب) يُعلَّم `PROPOSED` حتى يُؤكَّد. فصل WBS عن فئات Revit (فئة ← بنود متعددة والعكس).

**حقول سطر BOQ (boq-line-schema):** project_code، boq_revision، wbs_code، item_code، section، description، specification_reference، unit، quantity، measurement_basis، measurement_rule، source_method، source_model، source_view_or_schedule، source_element_ids، coverage_status، assumptions، exclusions، quantity_status، reviewer_status. للمسعّر: currency، rate، rate_unit، amount، rate_source، rate_date، rate_geography، rate_inclusions، overhead_profit_basis، tax_basis، rate_status.

**قيم الحالات:** Coverage: `COMPLETE_FOR_DEFINED_SCOPE`/`PARTIAL`/`NOT_CHECKED`/`NOT_MODELED`/`NOT_AVAILABLE` · Quantity: `MODEL_DERIVED`/`DOCUMENT_DERIVED`/`MANUAL_TAKEOFF`/`ALLOWANCE`/`NOT_MEASURED` · Review: `DRAFT`/`CHECK_REQUIRED`/`ACCEPTED`/`REJECTED` · Rate: `APPROVED`/`BUDGETARY`/`PROVISIONAL`/`NOT_PRICED`.
- التجميع في سطر واحد فقط عند تطابق النطاق والوحدة والقاعدة والمواصفة وأساس السعر. **Amount = Quantity × Rate** بسياسة تقريب معلنة.

**مخاطر الفئات:** Generic Models قد تكون نطاقاً حقيقياً أو placeholder؛ Parts/assemblies/fabrication تتداخل مع العناصر المضيفة؛ الأبواب/النوافذ تُعد منفصلة بينما الجدار يخصم الفتحة؛ nested families؛ Groups وdesign options تُنشئ تكرارات؛ linked models تحتاج هوية النموذج. ربط بالاسم family/type فقط = اقتراح حتى يؤكده QS.

**السياق السعودي:** SAR فقط إذا أُكد؛ SBC وCESMM/NRM/CSI لا تُفترض حاكمة تعاقدياً؛ VAT منفصل عن المبلغ الصافي ما لم يحدد العقد؛ سعر الرياض يحتاج مصدر وتاريخ ونطاق وأساس مورد — السعر العام من الإنترنت ليس سعر مناقصة.

**التسعير (شروط مسبقة):** الموقع والعملة، تاريخ/أساس السعر، المواصفة والشمول، ما يشمله السعر (توريد، تركيب، اختبار، هدر، نقل، معدات، OH/P، ضريبة)، مكتبة أسعار/عروض معتمدة. benchmark فقط → `BUDGETARY`؛ لا مصدر → `NOT_PRICED`. الحالة العامة `BUDGET ESTIMATE` أو `FOR REVIEW`.

**بوابات QA (qa-gates.csv):**

| Gate | الاسم | شرط النجاح | عند الفشل |
|---|---|---|---|
| G0 | Session discovery | توثيق الأدوات والسياق | توقف أو مسودة محدودة |
| G1 | Scope definition | تسجيل النطاق والافتراضات | سؤال أساسي أو إبراز الفجوة |
| G2 | Element mapping | كل عنصر مربوط/مستبعد/غير محلول | لا حذف صامت |
| G3 | Quantity integrity | حسابات قابلة للإعادة | إعادة الحساب وتوثيقه |
| G4 | Reconciliation | الفروقات محلولة أو مدرجة | تعليم للمراجعة |
| G5 | Pricing | أساس السعر يطابق النطاق والوحدة | ترك بلا تسعير |
| G6 | Release | لا عيوب حرجة للنطاق المُدّعى | إصدار DRAFT/FOR REVIEW |

**شدة المشاكل:** `CRITICAL` (قد يُبطل BOQ) · `HIGH` · `MEDIUM` · `LOW`. لا إغلاق بلا دليل وإعادة تشغيل الاستعلام.

## خطوات العمل (Workflow)
0. **السياق:** اسم/رمز المشروع، الموقع (الرياض فقط إن أُكد)، النوع، مرحلة المناقصة، غرض BOQ، الإصدار، العملة، أساس القياس، النماذج المرتبطة، الوحدات والإحداثيات، phase/design option/workset، مرحلة النموذج (DD/tender/construction/as-built).
1. **اكتشاف قدرات MCP:** تصنيف الأدوات (read/query، selection، schedule/report، view/export، write/experimental)؛ هل تقرأ الفئات وIDs، الخصائص، الهندسة، المواد، الغرف، الجداول، النماذج المرتبطة، التصدير؛ تعبئة `capability-ledger.csv`.
2. **النطاق وWBS:** فصل المشتق من النموذج / المستندات / الحصر اليدوي / المؤقت / المستبعد.
3. **جرد وتصنيف العناصر:** ElementId/UniqueId، category/family/type/marks، level/phase/design option/workset، المواد، الأبعاد، host/assembly/room، رمز التصنيف، حالة الإدراج وسببها؛ قائمة استثناءات للعناصر غير المربوطة.
4. **قواعد القياس لكل سطر:** code، وصف، وحدة، مصدر واستعلام، gross/net، مكونات، خصومات، هدر، دقة، استثناءات.
5. **حساب الكميات** بترتيب أولوية المصادر؛ ledger على مستوى العنصر قبل التجميع.
6. **بناء أسطر BOQ** بالحقول الإلزامية.
7. **مطابقة وQA** (`workflows/boq-reconciliation.md`): خمس طبقات — inventory، quantity، scope، commercial، revision. الفروقات الحرجة تُعرض ولا تُحل باختيار الأكبر/الأصغر.
8. **حزمة الإصدار** (انظر المخرجات).

**شروط التوقف:** تغيّر أساس القياس جوهرياً، لا مصدر موثوق لأبعاد حرجة، تكرار نماذج مرتبطة غير محلول، بنود مواصفات غائبة عن النموذج، طلب أسعار بلا أساس معتمد. بإذن المستخدم: متابعة بفجوات معلّمة وحالة `DRAFT — NOT FOR CONTRACT`.

## نقاط الفحص / QA Checklist
- [ ] لا تكرار ElementId/UniqueId داخل المصدر أو عبر الروابط.
- [ ] لا تداخل بين كميات العناصر والمواد.
- [ ] العناصر غير المربوطة/المستبعدة لها أسباب.
- [ ] تحويلات الوحدات قابلة للإعادة.
- [ ] الكمية = مجموع كميات المصدر بعد الفلاتر والتحويلات.
- [ ] Amount = quantity × rate ضمن تسامح التقريب؛ المجاميع الفرعية والكلية متطابقة.
- [ ] القيم السالبة/الصفرية/الناقصة/غير المعقولة فُحصت.
- [ ] مجاميع جداول Revit متطابقة أو الفروقات موثقة.
- [ ] الأبواب/النوافذ/الفراغات لا تُسبب خصماً مزدوجاً.
- [ ] طبقات المواد لم تُدمج عبر مواصفات تشطيب مختلفة.
- [ ] التسليح/الشدة/الملحقات الميكانيكية غير مستنتجة عند غيابها.
- [ ] المبالغ المؤقتة منفصلة بوضوح؛ كل نطاق غير منمذج أو غير مفحوص مُبلّغ.
- [ ] نسب المعقولية تُستخدم كتحذير فقط لا كبديل عن الكميات.

## أخطاء ممنوعة / تحذيرات
- اختراع أدوات MCP أو قدراتها، أو أسعار سوق، أو معاملات هدر، أو معالجة ضريبية.
- تعديل النموذج/العائلات/الجداول لتسهيل الحصر.
- معاملة وحدات Revit الداخلية (feet) كمتر.
- حذف عناصر غير مربوطة بصمت؛ إخفاء الهدر داخل الكمية المقاسة.
- وصف BOQ بأنه contract-ready قبل المراجعة البشرية.
- ادعاء فحص PDF أو قراءة مخططات لم تتم فعلياً.
- دقة عشرية أكبر مما يبرره المصدر.

## مخرجات متوقعة
1. ملخص تنفيذي والغرض.
2. Scope and coverage matrix.
3. جدول BOQ (CSV/XLSX-ready + تقرير مقروء).
4. سجل حساب/مصادر الكميات.
5. الافتراضات والاستثناءات والبنود المؤقتة وغير المنمذج.
6. سجل العناصر غير المربوطة والاستثناءات.
7. تقرير المطابقة/QA مع القضايا المفتوحة.
8. أساس التسعير وسجل مصادر الأسعار (إن سُعّر).
9. الإصدار/التاريخ/مصدر النموذج.
10. حالة الإصدار: `DRAFT` أو `FOR REVIEW` أو `READY FOR QS/ENGINEER APPROVAL`.
- الخاتمة دائماً: الكميات المغطاة، الاستثناءات، المناطق غير المتحقق منها، الفروقات المفتوحة، إجراء الاعتماد المطلوب. اللغة: العربية الفصحى مع المصطلحات الإنجليزية.

## ملفات مرجعية داخل الـ skill
- `references/boq-wbs.md` — WBS مبدئي 01–20 وسياسة التصنيف.
- `references/measurement-rules.md` — الوحدات، أسس القياس، الفتحات، الخرسانة/التسليح/الشدة، التشطيبات، الأنابيب، التقريب، الهدر.
- `references/parameter-mapping.md` — قائمة اكتشاف الخصائص، قواعد جدول الربط، مخاطر الفئات.
- `references/boq-line-schema.md` — حقول السطر وقيم الحالات.
- `references/capability-ledger.csv` — سجل الأدوات المتحقق منها (tool_name، capability_class، read_write، verified_this_session، evidence، limitation).
- `references/qa-gates.csv` — بوابات G0–G6.
- `workflows/model-takeoff.md` — 12 خطوة حصر + صف source-ledger الإلزامي.
- `workflows/boq-reconciliation.md` — طبقات المطابقة والاختبارات والشدة.
- `workflows/pricing.md` — شروط وقواعد التسعير.
- `templates/` — `boq-template.csv`، `quantity-source-register.csv`، `element-mapping.csv`، `rate-library.csv`، `assumptions-exclusions.csv`، `coverage-matrix.csv`.
- `tests/acceptance-tests.md` — 10 اختبارات قبول (capability honesty، unit integrity، traceability، duplicate prevention، coverage، schedule reconciliation، pricing safeguards، scope honesty، read-only default، arithmetic/status) على نموذج تجريبي قبل الإنتاج.
- `README_AR.md` (يتضمن أمر تشغيل عربي مقترح يبدأ بـ AUDIT-ONLY)، `CHANGELOG.md`، `MANIFEST.json`.

## مهارات مرتبطة
*(استنتاجاً — المصدر لا يسرد مهارات مرتبطة صراحةً)*
revit-api-safety، mohammed-ballaswad-revit-to-cad-professional-export، bim-revit-engineering، output-quality-control.
