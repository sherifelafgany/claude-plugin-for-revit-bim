# تعارضات بين الـ Skills (محتاجة قرار)

أثناء التلخيص ظهرت قواعد بتختلف من skill لـ skill. هذه القائمة **لم تُحسم** — القرار لشريف. (القاعدة العامة في الـ skills: قواعد WEDAD close-out الأحدث تغلب.)

## معماري / لوحات
| البند | Skill A | Skill B |
|---|---|---|
| ملء اللوحة | bim-revit-engineering: balanced whitespace | revit-sheet-methodology: املأ اللوحة بدون فراغ |
| Revision clouds | bim-revit-engineering: clouding where required | revit-sheet-methodology: لا clouds إلا بطلب |
| أبعاد الغرف في لوحات التشطيب | senior-architect-revit-practice §6: مطلوبة | نفس الـ skill §10 (WEDAD): لا أبعاد — **§10 يغلب** |
| دروة السطح | villa-levels-regulations: الرئيسي 1.80 م، الملحق 0.40 م | facade-qa-render-match: 0.40 فوق الخرسانة للكل / senior-architect: دروة السطح العلوي لا تُرفع |
| سماكة التكسية | facade-qa-render-match: حجر 7 سم، دهان 2 سم | نفس الـ skill في أمثلة التسمية: `Stone 50` / `Paint 25` |
| Title block / ترقيم | alkhalil-revit-delivery-workflow: ALKHALIL A1، A-xxx، أبعاد على الواجهات | riyadh/villa-permit: `KHL_TB_A1 Rayon`، PR-01xxxx، لا أبعاد فتحات على الواجهات |

## الرخص
| البند | riyadh-permit-drawings | villa-permit-drawings (يغلب للفلل) |
|---|---|---|
| مناسيب الموقع العام | spot level لكل سطح ودروة | أسماء الأسطح فقط |
| الغلاف وقائمة اللوحات | موجودة | محذوفة في WEDAD |
| مسقط السطح | فقط بطلب | ضمن المجموعة |

## MEP
| البند | Skill A | Skill B |
|---|---|---|
| ارتفاع vent cowl | villa-mep-design-rules: 0.30 م فوق سطح غير مستخدم / 2.10 م فوق سطح مستخدم | plumbing skill & P.858: 0.30–0.60 م |
| صرف الغسالة/غسالة الصحون | villa-mep-design-rules: لمداخل F.D الجانبية | senior-plumbing: standpipe بسيفون لفرع WP (بسبب الرغوة) |
| فتحات صيانة المحابس | plumbing: 40×40 (أو 30×30) | design rules: 300×300 |
| Issue stamp | plumbing: ISSUED FOR CONSTRUCTION (IFC) | hvac drafter: FOR REVIEW - NOT FOR CONSTRUCTION |
| تبطين بلوك 100 مم على جدران RC | plumbing: يُطلب من المعماري | design rules: ZERO ARCH ADDITIONS |
| Condensate | plumbing: CD Ø32 في الـ screed | design rules / drawing style: Ø32 @CLG / Ø25 U/T |
| لون الإنشائي المقطوع (maroon) | 128,0,0 (ARCH PDF) | 148,28,33 (STR) / 140,26,31 (raster) / 139,26,30 @50% (HVAC) |
| محور الـ mirror | drawing style: 26.00 − x | hvac drafter: 26.012 − x / check scripts: x < 13.00 |

## HVAC
| البند | senior-hvac-engineer | villa-mep-design-rules |
|---|---|---|
| خلوص الوحدة الخارجية ODU | جانب ≥0.40، خلف ≥0.25، ظهر لظهر ≥0.50، طرد ≥1.00، وجه لوجه ≥2.50 م | 600 مم كل جانب و1.0 م أمام |
| حرارة التصميم الخارجية | T3 46 °C | 48/50 °C |
| الرطوبة الداخلية | ≤ 55% | 50% |
| شفط الحمام | 25 L/s مستمر أو 50 L/s متقطع (للتحقق) | 25 L/s متقطع |
| تصريف condensate | ممكن لـ basin tailpiece (IPC 314.2.1.1) | أقرب FD |

## عام
- اللغة: sherif-working-style يقول "عربي"؛ skills أخرى تحدد "مصري".
- project-baseline-review يستخدم COORDINATION thread، بينما villa-project-playbook v2 دمجه في PM.
- villa-project-playbook §2 ("revit review يحسم فنياً") يتعارض مع §0 WEDAD (شريف يملك كل قرار تصميم) — **§0 يغلب**.
- architectural-drawing-style يشير لمجلد `assets/` (صور مرجعية و`lib.js`) غير موجود في النسخة المتزامنة.
- alkhalil-revit-delivery-workflow يشير لـ skill اسمه `alkhalil-presentation-deck` غير موجود.
