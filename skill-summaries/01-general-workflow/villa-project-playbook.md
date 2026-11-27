# villa-project-playbook — دليل تشغيل مشروع فيلا

> **الغرض:** إعداد مشروع الفيلا وحلقة العمل (Threads، الأدوار، تناوب Revit، منهج WEDAD). **متى تُستخدم:** عند بدء أو تشغيل أي مشروع فيلا/سكني جديد على Revit لشريف.

## الفكرة الأساسية
الـ skill ترسم الهيكل والحلقة؛ المعايير التفصيلية في skills أخرى. `DECISIONS_LOG.md` للمشروع واتفاقات العميل تتقدم دائمًا على الافتراضات، ولا تُنسخ قرارات/بيانات/IDs مشروع آخر (قيم WEDAD افتراضية فقط). قواعد إغلاق WEDAD (القسم 0) تلغي أي تعارض تحته. أول سؤال: لأي شركة؟ (SS+ Design & Engineering Services أو ALKHALIL ARCHITECTS) — الـ title block والترقيم والهوية تتبعها.

## القواعد والأرقام الأساسية
**قواعد إغلاق WEDAD (2026-11-22) — الأعلى أولوية**
- ترتيب العمل: المدخلات والقرارات المرئية أولًا قبل أي MEP (CAD الداخلي، استبيان العميل، المراجع، العينة المعتمدة، معيار الحمام، الأسقف + الإنارة، أماكن وحدات التكييف وaccess panels) ← تُعرض كصور وتُعتمد وتُجمّد.
- المعماري + الداخلي يُنهيان أولًا شيتًا شيتًا/منطقة/دورًا؛ MEP يعمل بالخلفية على آخر ARCH ولا يُغلق أو يُختم قبل إغلاق المعماري.
- الإغلاق مع شريف تخصص واحد في المرة حتى «تمام»؛ المغلق لا يُفتح إلا لمشكلة تنسيق حقيقية ثم يُعاد طبعه.
- التسليم: حزمة مستقلة كاملة لكل تخصص (غلاف، قائمة لوحات، كل الأقسام) = PDF مدمج واحد + DWG مدمج واحد (DXF فقط إن تعذّر DWG). CAD أصلي من بيانات الرسم، لا من PDF. ما ينتهي أولًا يُرسل أولًا.
- شريف يملك كل قرار تصميمي؛ المرئي ثابت والتنسيق يحرك المخفي فقط. لا افتراضات تصميمية من أي Thread/PM/مراجع. الإنشائي لا يُلمس أبدًا. (هذا يلغي «التغييرات المتفق عليها تُنفذ فورًا» و«revit review صاحب القرار الفني النهائي» فيما يخص التصميم.)
- Designer وDrafter منفصلان لكل تخصص MEP. أقل نموذج لأي Thread: Opus.
- الملاحظات تُصحح على المجموعة الحالية واحدة واحدة؛ لا رجوع لنسخة أقدم، لا إعادة رسم شاملة—فقط المتأثر. لا تراجع (No regression). كل ملاحظة فئة: كل دور، الوحدتان، كل شيت، كل تخصص متأثر. تعديلات الجدران من شريف تُنفذ فورًا بلا نقاش.
- **ممنوع (فشل في WEDAD):** تحويل ملاحظاته لقواعد رقمية بسكربت (مثل «lights centred within 50 mm»)؛ بوابات مراجعة على الصياغة؛ إعادة رسم MEP كاملة على خلفية جديدة (السباكة 10 إصدارات في ليلة)؛ ربط/استيراد CAD للأثاث (كل قطعة = نموذج Revit بنفس الشكل والمكان والدوران)؛ جدول يغلق MEP قبل المعماري؛ افتراضات Thread لبنود مرئية.
- الملفات: مجلد الجهاز هو الوحيد المشترك بين الحسابات؛ كل قاعدة جديدة في `CLAUDE.md` في نفس الساعة؛ كل Thread يعيد كتابة HANDOFF بعد كل مهمة. ملفات INBOX المشتركة: stage حديث، فحص الحجم، commit مع mtime guard، تأكد أن الحجم زاد.
- التذكيرات: فاصل طويل عادةً، كل 5 دقائق فقط وقت الإغلاق؛ لا ردود «nothing new».

**الافتتاح (Kickoff) — سؤال واحد بقائمة**
- نموذج Revit وجهازه/مجلده؛ النموذج الإنشائي (رابط خارجي من المهندس الإنشائي، مبدئي).
- الصك: الرقم، مساحة الأرض، إحداثيات الأركان؛ «اعتمد الصك» = قيم الصك تفوز.
- الرفع المساحي: ±0.00 مقابل MSL (TBC إن غاب، لا اختراع). ±0.00 = منسوب الشارع.
- استبيان العميل (يتحول لقائمة بنود MEP مرقمة)، الرندرات (الواجهة مقدسة)، تقرير التربة (تأكد أنه لنفس القطعة)، صرف عام أو بيارة.
- تصدير ARCH من المالك (PDF/CAD بالأثاث) = الخلفية الوحيدة لكل التخصصات؛ كل رفع جديد يستبدلها ويستلزم diff قطعة بقطعة قبل أي ختم.
- النطاق الافتراضي: معماري، HVAC، سباكة، كهرباء، تيار خفيف. الحريق وBOQ وsleeves/shop details والإنشائي فقط إن شملها العقد. رسومات مناقصة لا shop drawings: لا sleeves في النموذج.

**ملفات مجلد `claude` للمشروع**
- `PROJECT_STATE.md`، `00_START-HERE.md`، `CLAUDE.md`، `<PROJECT>_RULES.md`، `DECISIONS_LOG.md` (APPEND ONLY، أعد القراءة قبل الكتابة، كل مراجع له كتلة D-numbers مثل Review 1 D-14xx، Review 2 D-12xx، ELEC D-16xx)، `HANDOFF/`.
- `THREADS/REVIT_LOCK.txt`: سطر `HOLDER|YYYY-MM-DD hh:mm|note` (توقيت الرياض)، `FREE|...` عند الفراغ.
- `THREADS/00_THREADS_PROTOCOL.md`، `coord/REVIT_TURN_LOG.md` (`| time | thread | action | task | notes |`؛ GRANTED/RELEASED/QUEUE/DECISION/ORDER/PAUSE).
- `coord/COORD_REQUESTS.md`: `id | date | from | to | element+ID+location | problem | proposal | needed before | status` — الحالة OPEN / OPEN pending verify / CONFIRMED / ESCALATED / CLOSED؛ IDs CR-001...
- `FOR_STRUCTURAL_ENGINEER_rN_<date>_AR.md` (فقط بطلب شريف): عربي، بند لكل Element ID + grid، أولوية A/B.
- `qa/REVIEW_STATUS.md`، و`coord/REVIT_REVIEW_rules.md`، `_defects.tsv`، `_columns_vs_link.tsv`.

**هيكل الـ Threads (v2، 2026-11-11)**
| Thread | Model | يكتب Revit |
|---|---|---|
| PM (القناة الرئيسية، يمتص COORDINATION) | Opus | أبدًا |
| revit review (QA مستقل) | Fable | أبدًا |
| ARCH design (يشمل اللاندسكيب) | Fable | أبدًا |
| ARCH drafting | Opus | نعم |
| STR (اتصال بالمهندس الإنشائي فقط) | Opus | قراءة فقط |
| PLB design / HVAC design | Fable | أبدًا |
| ELEC design | Opus | أبدًا |
| MEP drafting (الكاتب الوحيد لـ MEP) | Opus | نعم |
- تفعيل الـ Threads حسب المرحلة (P1: PM، revit review، ARCH design، ARCH drafting، STR). Thread جديد فقط لنطاق حقيقي (رشاشات بحكم SBC 801 = FIRE design). Sonnet للتقارير البسيطة فقط.
- النطاقات الكبيرة تُقسم بالأدوار (B+GF / FF+RF) مع **lead** لكل تخصص. ELEC يُغلق أخيرًا.
- جلسة جديدة لكل مهمة ثقيلة `<ROLE> - <package>` تقرأ 00_START-HERE وRULES وDECISIONS_LOG وPROJECT_STATE وHANDOFF.
- سكربت القراءة: بلا Transaction، < 7.5 KB؛ المجهول `[ASSUMED]`، البيانات المنتظرة `[DUMP]`.
- المراجع لا يصلح أبدًا؛ يراجع الشيت/المجموعة كلها، يعيد تحميل رابط STR أولًا، يقيس النموذج كاملًا؛ الحكم APPROVED / REJECTED / BLOCKED، S1/S2/S3، والقيم غير المتحققة [غير متحقق].

**المراحل (PROJECT_STATE)**
- P0 الاستلام + التدقيق (`qa/AUDIT_<date>.md`: KEEP / FIX / REDO بالـ IDs).
- P1 الرخصة: مراجعة Balady/SBC، الواجهة مقابل الرندر، حجز فراغات MEP لمرة واحدة (shafts، risers، غرف المعدات، مناطق ODU، الخزان/البيارة). خروج: PDF الرخصة APPROVED وPR sheets مجمدة.
- P2 قاعدة التنسيق: ARCH × STR نظيف مقاس.
- P3 تصميم MEP على الورق لكل الأدوار. P4 رسم MEP تخصصًا واحدًا حتى 100% (script gate TOTAL = 0، column clashes 0، worksets)، منطقة بمنطقة في job كبير واحد، Pilot أول منطقة.
- P5 لوحات تنفيذية. P6 الإصدار: «IFC» هنا = Issued For Construction (لا IFC export)؛ PDF + DWG لكل تخصص، PDF مدمج، فهرس.

**مسار اللوحات الورقية (بدون Revit، 2026-11-15)**
- Python / PyMuPDF فوق ARCH PDF بأسلوب villa-mep-drawing-style؛ Pilot دور واحد.
- مراجعتان مستقلتان مقسمتان بالأدوار؛ كلتاهما تختمان قبل الإرسال.
- بوابة تراكب الأجهزة: كل W.C / F.D / shower drain مقابل الـ drop، tolerance 0.05 m، لكل غرفة. الأعمدة 1:1 مع STR (maroon). zero-ARCH-additions diff.
- ترتيب السباكة: drainage ← water supply ← gas ← site ← الحزمة النهائية؛ ثم HVAC؛ ELEC أخيرًا.
- الملفات: حرف مراجعة (r5، r5b) + نسخة `_<sha16>`، و`_REVIEW` أثناء المراجعة؛ احفظ مصدر المولّد tgz.

**حزم التصميم**: `claude\<discipline>\DESIGN_PACKAGE_<LEVEL-or-SCOPE>_<SYSTEM>_rN.md`؛ rN جديد لكل تغيير. الأجزاء: المدخلات، القرارات بأسبابها، جدول البناء (صف لكل عنصر)، الحسابات بمصدر أو [غير متحقق]، طلبات التنسيق، الاستثناءات.

**دورة الطلب**: Sherif ← PM ← design (حزمة + صفوف) ← drawable check ← revit review (يختم sha + اسم الـ job) ← PM يمنح الدور ← drafting ينفذ + script gate + صورة ← revit review ← PROJECT_STATE ← ملخص واحد لشريف.

**Write jobs (§4b)**
- Rows (TSV) ← lint = 0 (رمز UPPER = Revit يرفض؛ يُصلح على الورق) ← ختم sha.
- `coord\<job>_D.cs` و`<job>_W.cs` متطابقان عدا سطر `DRY`؛ loader < 1 KB (`LOADER_send_from_disk_v2.cs`؛ تعديل JOB, LOG, SV فقط) يتوقف إن وُجد الـ log أو النموذج معدّل أو NumberOfSaves ≠ SV.
- DRY مرة (bad 0، mine == expected، warnings 0) ← WET مرة (COMMIT + SYNC) ← قراءة المراجع (Comments = `<JOB> <ROW>`).
- بوابات داخل الـ job (أي فشل = rollback كامل): ذاكرة حرة ≥ 2.2 GB؛ title guard؛ sha؛ كل اسم يطابق عنصرًا واحدًا؛ الوضع ≤ 3 mm؛ create-only؛ count == rows؛ warnings 0؛ لا dialog؛ workset بالـ id.
- الذاكرة: إنهاء كل job بـ `D = null; GC.Collect(); GC.WaitForPendingFinalizers(); GC.Collect();`؛ ≥ 2.5 GB قبل الإرسال (توقف إن < 1.6 بعد GC)؛ PNG شيت واحد لكل إرسال (PixelSize 3000)؛ PDF 1-5 شيتات عبر Document.Export (لا PrintManager)؛ أعد تشغيل Revit فقط إن > 6 GB بعد GC.
- الحذف لا يكون بسكربت أبدًا: selection set + view مقفل عليه (SELECT SET ONLY)، شريف يحذف من الـ selection set فقط، ثم sync + restart، ثم integrity read.

**قواعد النمذجة المعمارية (§5)**
- فقط عائلة الباب/الشباك تقطع الجدار؛ لا voids إضافية.
- رأس الشباك تحت بطنية الكمرة مباشرة؛ الشباك ينزل لا الكمرة؛ نفس منسوب الرأس لكل واجهة/دور. لا شباك خلف جدار/عمود ولو 1–2 cm. تغيير يؤثر على نسخ أخرى = Type جديد.
- جدران البلوك من البلاطة/الـ raft حتى بطنية الكمرة/البلاطة (بلا فراغ أو تداخل) وتقطع تشطيب الأرضية؛ تُقسم عند حدود الكمرة الجزئية.
- كل الإنشائي مغطى بتشطيب؛ لا خرسانة مكشوفة. العمود أعرض من الجدار: يُسمَّك الجدار ليدفنه.
- الأعمدة: نسخ من الرابط على workset الإنشائي؛ الكمرات لا تُنسخ أبدًا. الإنشائي لا يُعدَّل.
- فتحات البلاطات = Shaft openings، لا قص sketch.
- المواسير داخل جدران قائمة فقط (أي سماكة) أو أفقيًا في فراغ سقف الغرفة السفلية. كل riser داخل جدار بالكامل، يمر في الكمرة بـ sleeve؛ تجاوز حدود الكمرة يذهب لقائمة المهندس الإنشائي (Sherif 2026-11-25). صفر إضافات معمارية من MEP.
- كل عنصر جديد/معدّل له WorksetId صريح.

**أمان Revit الإضافي (§6)**: `send_code_to_revit` < ~7.5 KB؛ C# 5 (لا `$""`، `?.`، `is T x`)، `ElementId.Value`؛ لا say_hello؛ `ClearAfterRollback(true)`؛ Edit Profile مسموح (2026-11-25) مع: حذف مقاطع < 1 cm، لا خطوط متقاطعة؛ لا حذف/إعادة إنشاء شيتات أو views؛ لا الكتابة فوق تعديلات شريف اليدوية.

**الطباعة والحسابات**
- «اطبع علي وضعه» = تصدير جديد من Revit المفتوح كما هو (بلا تعديل/sync/reload).
- الملفات المستبدلة تُسمى `_OLD_hhmm`، لا تُحذف.
- PC المكتب (16 GB): Revit + Claude + الكاتب (+ مساعد PM قراءة فقط).
- التواصل: عامية مصرية، قصيرة، ≤ 80 كلمة عند غيابه؛ نسبة الإنجاز = المرسوم + المُراجع فقط؛ توقيت الرياض.
- الموافقات: فحص الأمان على الـ PC يقبل فقط موافقة شريف المكتوبة داخل الـ drafting thread.

## خطوات العمل (Workflow)
1. اسأل عن الشركة، ثم قائمة Kickoff كاملة في رسالة واحدة.
2. أنشئ ملفات `claude` (STATE، START-HERE، CLAUDE.md، RULES، DECISIONS_LOG، LOCK، PROTOCOL، TURN_LOG، COORD_REQUESTS).
3. جمّد المدخلات والقرارات المرئية بموافقة شريف.
4. نفّذ المراحل P0→P6 وفق بواباتها.
5. لكل دور Revit: انتظر المنح ← Reload Latest + إعادة تحميل رابط STR ← خذ القفل (فقط إن FREE) ← جرد قبل/بعد ← jobs مراجَعة ← Sync with Central (relinquish) ← القفل FREE + «Revit free» + HANDOFF للمراجع. «وقف» = توقف فورًا ونظّف واترك القفل FREE.
6. أغلق التخصصات واحدًا واحدًا مع شريف وسلّم حزمة كاملة لكل منها.
7. في الإغلاق: حدّث الـ skills بكل قاعدة أو درس جديد.

## نقاط الفحص / QA Checklist
- [ ] المدخلات المرئية معتمدة ومجمدة قبل أي MEP
- [ ] المعماري مغلق قبل ختم أي MEP
- [ ] كل job: DRY مرة ← WET مرة، sha مختوم، log + dump للـ ids الجديدة
- [ ] كل عنصر جديد على workset تخصصه
- [ ] بوابة تراكب الأجهزة 0.05 m مُطبقة على كل خلفية ARCH جديدة
- [ ] صفر إضافات معمارية من MEP؛ كل riser داخل جدار
- [ ] «تم» فقط بعد تصدير الشيت وقراءته مقابل كل ملاحظة + APPROVED من revit review
- [ ] التصحيح في الوحدة A طُبق فورًا في الوحدة B؛ الأعداد مع الـ IDs
- [ ] الحزمة النهائية كاملة: PDF مدمج + DWG أصلي

## أخطاء ممنوعة / تحذيرات
- كتابة شخصين على Revit معًا أو الكتابة فوق قفل آخر.
- إعادة إرسال أمر مرفوض/محجوز أو تقسيمه أو الالتفاف عليه؛ إعادة تسمية أو مسح STOP/guard log.
- حذف بسكربت، أو تعليمات «احذف كل الظاهر».
- لمس الإنشائي أو إضافة جدران/صناديق/shafts من MEP.
- إرسال تصدير قديم عند طلب «اطبع علي وضعه».
- بطء الـ PC ليس عذرًا يُقال لشريف.
- إذا ساءت التصحيحات بعد ~10 جولات: جلسة جديدة تعيدها مرة واحدة بطريقة واحدة (الهيكل أولًا: risers، mains، vents، ثم منطقة بمنطقة).

## مخرجات متوقعة
- مجلد `claude` منظم بالملفات أعلاه، حزم تصميم rN، jobs موثقة، PROJECT_STATE محدث، PDF + DWG لكل تخصص بفهرس، دروس الإغلاق في الـ skills.

## مهارات مرتبطة
revit-api-safety، revit-sheet-methodology، output-quality-control، revit-clash-coordination، villa-levels-regulations، khl-area-calculation-rules، villa-mep-design-rules، villa-permit-drawings، villa-mep-check-scripts، villa-mep-drawing-style. مرجع خارجي: دليل شريف «دليل شاتات المهندسين و revit review».
