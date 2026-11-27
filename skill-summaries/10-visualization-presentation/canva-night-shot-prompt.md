# canva-night-shot-prompt — تحويل render نهاري إلى لقطة ليلية في Canva

> **الغرض:** كتابة prompt تعديل بأسلوب مصور معماري لتحويل render نهاري معتمد إلى لقطة ليلية/blue hour في Canva بحد أقصى 1,500 حرف. **متى تُستخدم:** عندما يرسل شريف render نهارياً نهائياً ويطلب نسخة ليلية ("حوّلها شوت ليلي").

## الفكرة الأساسية
الـ render النهاري هو Reference 1 الوحيد ومصدر الكاميرا والعمارة واللاندسكيب والمواد. تنفيذ المطلوب فقط: لا عناصر جديدة ولا تغييرات تصميمية، والإنارة وحدات خفية مدمجة أو معقولة. الحد الصارم 1,500 حرف لأن Canva Dream Lab يقطع ما بعده، لذا القواعد الحرجة أولاً.

## القواعد والأرقام الأساسية
- الرد بالعربية، والـ prompt بالإنجليزية.
- **الـ prompt ≤ 1,500 حرف**؛ عدّ الأحرف بـ `wc -c` قبل الإرسال.
- رفع مرجع واحد فقط: الـ render النهاري كـ Reference 1.
- نص اللافتة يبقى كما هو حرفاً بحرف.
- حفظ نسخة من الـ prompt بجانب الـ render في ملفات المشروع.
- السماء: blue hour عميقة، **15 دقيقة بعد الغروب**، صافية مع توهج دافئ خفيف عند الأفق؛ لا نجوم ولا قمر.
- الإنارة: **دافئة 2700K**، وحدات خفية مدمجة فقط: wall-washers عند قاعدة السور وأعمدة البوابة، downlights فوق كل باب، uplights على النخيل/الأشجار، إنارة أرضية موجودة مضاءة، توهج دافئ من النوافذ (كغرف مضاءة خلف ستائر شفافة، غير محترق)، خط إنارة تحت الكرانيش اختياري. **لا إنارة ملونة أو neon أو strip على الواجهة.**
- الجودة: نفس إطار **16:9**، بلا نص مضاف.

**قالب الـ prompt (يُملأ [ ] ويبقى تحت 1,500 حرف):**
```
Act as an award-winning architectural night photographer and CGI specialist. Reference 1 is my approved day render:
edit it into a blue-hour night photograph. It is the ONLY source of camera, framing, architecture, landscape and materials.
1. PRESERVATION: keep the camera, composition, every [wall, cornice, moulding, window, balustrade, door, garage door,
   sign, trees, hedge, kerb] exactly; do not move, add, remove or redesign anything. Keep the sign text "[TEXT]" unchanged.
2. SKY: deep blue hour, 15 minutes after sunset, clear sky with a faint warm glow low on the horizon; no stars, no moon.
3. LIGHTING (warm 2700K, only discreet built-in fixtures): soft grazing wall-washers at the base of the boundary wall
   and gate piers; small downlights over each door; gentle uplights on [palms/trees]; existing ground lights lit; a soft
   warm glow through the windows, as from lit rooms behind sheer curtains, not overexposed; [optional: a subtle line
   light under the cornices]. No coloured, neon or strip lighting on the façade.
4. MATERIALS: [stone] shows its texture in the grazing light; [wood doors] glow softly; glass reflects the blue sky.
5. QUALITY: ultra-photorealistic, balanced exposure between sky and façade, deep but clean shadows, no blown highlights,
   no noise, no AI artefacts. Same 16:9 framing, no added text.
Prioritize architectural accuracy over creativity.
```

## خطوات العمل (Workflow)
1. استلام الـ render النهاري المعتمد.
2. ملء القالب بعناصر المشروع (الجدران، الكرانيش، الأبواب، اللافتة ونصها، الأشجار، المواد).
3. عدّ الأحرف بـ `wc -c` والتأكد ≤ 1,500.
4. إرسال الـ prompt في code block مع عدد أحرفه وعبارة: "upload the day render as Reference 1 only".
5. تسمية أي سطر قد يرغب في حذفه (مثل خط إنارة الكرانيش).
6. حفظ نسخة بجانب الـ render في ملفات المشروع.
7. إن أخرجه بنفسه: إرسال النتيجة إلى خيط المراجعة بالحجم الكامل قبل اعتبارها ناجحة.

## نقاط الفحص / QA Checklist
- [ ] الـ prompt ≤ 1,500 حرف (مُتحقق بـ `wc -c`).
- [ ] مرجع واحد فقط (الـ render النهاري).
- [ ] نص اللافتة مطابق حرفاً بحرف.
- [ ] لا عناصر جديدة ولا تغيير تصميم؛ الكاميرا والتكوين محفوظان.
- [ ] الإنارة دافئة 2700K مدمجة وخفية؛ لا ملونة/neon/strip.
- [ ] المراجعة على الصورة بالحجم الكامل لا المعاينة.

## أخطاء ممنوعة / تحذيرات
- إضافة عناصر أو تغييرات لم يطلبها.
- رفع أكثر من مرجع.
- تجاوز 1,500 حرف (يُقطع الباقي).
- **حدود Canva المعروفة:** أداة generate-image في الـ connector تعيد توليد الصورة كاملة حتى لو قال الـ prompt "keep everything" — للتعديل الموضعي الحقيقي يُعطى شريف prompt قصير لـ Magic Edit للمنطقة التي يحددها بالفرشاة. الـ connector يعيد معاينة 200 px فقط؛ المراجعة الكاملة تحتاج الصورة التي ينزّلها شريف.

## مخرجات متوقعة
- prompt إنجليزي في code block مع عدد أحرفه وتعليمة رفع المرجع.
- ملاحظة بالأسطر الاختيارية القابلة للحذف.
- نسخة محفوظة بجانب الـ render في ملفات المشروع.

## مهارات مرتبطة
*(استنتاجاً — المصدر لا يسرد مهارات مرتبطة صراحةً)*
architectural-photography-render-prompt، architectural-visualization-director.
