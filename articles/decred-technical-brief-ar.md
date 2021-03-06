# DTB001: موجز تقني عن ديكريد

Christina Jepson *1

**موجز:** أصبحت دفاتر نظم الطابع الزمني الموزعة والمحفزة بتوكن مثل البتكوين الدعامة الأساسية للتمويل الحديث. قمنا بتطبيق منصة تجمع بين عناصر سلاسل كتل إثبات العمل (PoW) وإثبات الحصة (PoS) في محاولة لكسب فوائد النظامين. ويشمل التغيير الأقل أهمية لقواعد الإجماع الخاصة بشبكتنا إضافة توقيع المنحنى الإهليلجي، تنقيح وتعديل لغة البرمجة النصية الحالية، إضافة قابلية التوسع في مجالات متعددة لدعم التغييرات المستقبلية للبروتوكول، وتَجَمّع الحصص الموزع.

**الكلمات الدلالية:** النظام الهجين إثبات العمل/إثبات الحصة PoW/PoS - إثبات السلطة PoA

1 الشركة زيرو، شيكاغو، إلينوي

*المراسلات: cjepson@decred.org

## المحتوى

| **مقدمة**                                       | **1** |
| --- | --- |
| 1. التصميم الهجين إثبات العمل/إثبات الحصة       | **1** |
| 2. تَجَمّع الحصة اللامركزي                          | **2** |
| 3. عناصر التصميم الثانوية                       | **2** |
| 1.3 خوارزميات توقيع المنحنى الإهليلجي            |   2   |
| 2.3 دالة الهاش                                  |   3   |
| 3.3 ملحقات البرنامج النصي                       |   3   |
| 4.3 عزل توقيع البرنامج النصي و تزوير الإثباتات   |   3   |
| 5.3 ملحقات المعاملة                             |   3   |
| 6.3 تحسينات متنوعة                              |   3   |
| **المراجع**                                     | **4** |
| **الملحق أ: شنور متعدد التوقيعات**              | **5** |

## مقدمة

تم تطبيق بروتوكولات الطابع الزمني الموزعة لأول مرة على  جعل الشبكة المالية لامركزية في الورقة الرائدة عن البتكوين التي أعدها ناكاموتو [1]. وقد شهد الحقل متابعة متفجرة للبحث من كل من الهواة والمحترفين، يتنافسون على تقديم التمديدات والتعديلات، والتحسينات وتنقية البروتوكول الحالي. ومن بين التطبيقات البارزة للأفكار الجديدة، الإيثيريوم [2]، التي وسعت نطاق البرمجة النصية، CryptoNote [3]، التي حسنت الخصوصية، و Sidechains [4]، التي حققت في تثبيت توكنات البتكوين ذو إتجاهين ب 1:1. وتستخدم كافة هذه البروتوكولات إثبات العمل (PoW) كما هو موضح في المستند التقني الخاص بـالبتكوين.

إضَافَة شائعة لبروتوكول البتكوين تعمل على تعديل آلية الإجماع  لاستخدام إثبات الحصة (PoS) بشكل كامل أو جزئي، أو استخدام حصة الشخص (التوكنات) بدلاً من القدرة الحسابية للمشاركة في عملية الطابع الزمني.

تم تنفيذ أول سلسلة كتل إثبات الحصة قائمة على بروتوكول البتكوين في عام 2012 بواسطة King و Nadal [5]، وتتضمن كل من إثبات العمل و إثبات الحصة التي تميل تدريجيا نحو إثبات الحصة بالكامل مع مرور الزمن.

كانت الانتقادات لأنظمة إجماع إثبات الحصة المحضة وفيرة [6] [7]، مع معارضة شديدة قادمة من أولئك الذين يعملون بسلاسل كتل إثبات العمل المحضة. إن الحجة الأكثر شيوعاً ضد إثبات الحصة فيما يتصل بالطابع الزمني الموزع هي "لا شيء على المحك" أو "المحاكاة غير المكلفة"، و التي تصف عدم الاستقرار المنهجي الناجم عن قدرة أصحاب الحصص على إنشاء تواريخ جديدة بديلة وغير مكلفة بأنفسهم.

وعلى الرغم من هذا الجدل، فمن الواضح أن الأنظمة التي لديها نظام إثبات حصة تعتمد على نظام ختم الوقت لإثبات العمل قد تكون قادرة على تحقيق الإجماع بشكل مستقل. وقد تم استكشاف ذلك رياضياً من قبل  Bentov وزملاؤه [8] في ورقة عن مخططهم، إثبات السلطة (PoA)، ويبدو أنه امتداد قابل للتطبيق لبروتوكولات إثبات العمل التي قد تمكّن بعض الخصائص الجديدة المثيرة للاهتمام. وقد تم اقتراح تصميم مماثل يسمى MC2 من قبل Mackenzie في عام 2013 [9]. ونصف هنا بناء وتنفيذ نظام إجماع مماثل أسميناه "ديكريد".

## 1. التصميم الهجين إثبات العمل/إثبات الحصة

التباين الرئيسي مع مخطط ساتوشي الذي سبق وصفه [8] هو نظام يانصيب جديد حيث يجب شراء التذاكر ثم الانتظار لفترة استحقاق قبل أن يتم اختيارها أو إنفاقها. ويتم اختيار تذاكر الكتلة بطريقة معجمية من مجموعة تذاكر ناضجة إستنادا على عملية إختيار عشوائية مضمنة في رأس الكتلة. ولأن التلاعب في هذه العملية العشوائية أمر صعب في ظل نظام إثبات العمل، فإن التلاعب باختيار التذاكر يرتبط بتكاليف أساسية يتحملها المعدن. ويمكن وصف اختيار التذاكر على مدار فترة زمنية بواسطة دالة كثافة الاحتمالية شبيهة باحتمال الحصول على كتلة في إثبات العمل بمعدل تجزئة ثابت مع مرور الوقت عند صعوبة ثابتة [1]، مما يؤدي إلى توزيع احتمالي مع وضع يساوي تقريبًا نصف حجم تجمع التذاكر. ويتم تنظيم سعر شراء التذكرة من خلال صعوبة التحصيص الجديدة التي يتم تحديدها من خلال متوسط ​​الوزن الأسي لعدد التذاكر التي تم شراؤها وحجم مجمع التذاكر الناضجة في الكتل السابقة.

يتم شرح التحقق من كتل إثبات العمل بالخطوات التالية:

i. يتم استخراج الكتلة بواسطة المعدن الذي يختار المعاملات لوضعها بالداخل. ويتم إدراج المعاملات المتعلقة بنظام الحصة في مجموعة نواتج المعاملات غير المنفقة.

ii. يصوت معدنو إثبات الحصة على الكتلة من خلال إنتاج معاملة تصويت من تذكرتهم. يسمح التصويت على حد سواء ببناء كتلة فوق الكتلة السابقة ويحدد ما إذا كانت شجرة المعاملات العادية السابقة (التي تحتوي على قاعدة العملة والمعاملات غير المتعلقة بالحصة) صالحة.

iii. يبدأ معدن آخر في بناء كتلة، بإدراج أصوات معدني إثبات الحصة. ويجب إدراج أغلبية الأصوات المدلى بها في الكتلة التالية لكي تقبل الشبكة تلك الكتلة. ومن بين معاملات التصويت في هذه الكتلة الجديدة، يقوم المعدن بفحص علم لمعرفة ما إذا كان معدن إثبات الحصة قد أشار إلى ما إذا كانت شجرة المعاملات العادية الخاصة بالكتلة صالحة أم لا. يتم رفع أعلام التصويت هذه، وبناءً على تصويت الأغلبية، يتم وضع علامة بت في هذه الكتلة لتوضيح ما إذا كانت شجرة المعاملات العادية للكتلة السابقة صالحة.

iv. عند إيجاد رقم خاص يفي بصعوبة الشبكة، يتم إدراج الكتلة في سلسلة الكتل. وإذا تم التحقق من صحة شجرة المعاملات العادية للكتلة السابقة، أدخل هذه المعاملات في مجموعة مخرجات المعاملة غير المنفقة. انتقل إلى (i.).

لتثبيط التلاعب بالأصوات المضمنة، يتم تطبيق عقوبة دعم خطية على الكتلة الحالية إذا فشلوا في تضمين جميع معاملات التصويت في الكتلة الخاصة بهم. تساعد العقوبة "اللينة" لإبطال شجرة المعاملات السابقة على منع التخلي عن العمل، وهو أمر ضروري لتأمين النظام، ويفترض أن يتم الحصول على الكتلة التالية من قبل معدن غير مهتم بالحفاظ على دعم الكتلة السابقة لصالح نفسه. وحتى في حالة عدم صحة ذلك، سيحتاج المعدن الخبيث ذو معدل الهاش المرتفع على الأقل إلى $(\text{number for majority}/2)+1$ صوت لصالح شجرة معاملات الكتلة السابقة الخاصة بهم من أجل إنتاج كتلة تمنحهم أي دعم من الكتلة السابقة.

تتم إضافة علامات البت بوضوح إلى كل من رأس الكتلة والأصوات بحيث يمكن لأي معدن التصويت بسهولة على التفرعات الصلبة أو الناعمة الجديدة.

## 2. تجمع الحصة اللامركزي

من بين المسائل الناشئة عن تصاميم إثبات الحصة السابقة، كيفية إجراء تجمع في تعدين إثبات الحصة مماثل لتجمع تعدين إثبات العمل. وهذا أمر مفيد لتجمع معدن إثبات العمل لأن تجمع معدن إثبات الحصة لا يتطلب أجهزة مخصصة للتعدين تتجاوز مجرد تشغيل العقدة، وعلى عكس تعدين إثبات العمل، فمن غير المحتمل أن ينشأ سيناريو تعزيز المركزية حيث التكاليف الرأسمالية للتعدين تزداد مع انخفاض الأرباح. تحل ديكريد هذه المشكلة عن طريق السماح بإدخال العديد من المدخلات في عملية شراء التذاكر والالتزام بمبلغ دعم ناتج المعاملة غير المنفق لكل إدخال تناسبيا، مع الالتزام أيضًا بمفتاح إخراج عام أو نص برمجي جديد لهذه المكافآت النسبية. ثم يتم تقديم العلاوة لمن ينتجون التذكرة بطريقة غير موثوقة، ويمكن التوقيع على التذكرة من طرف العديد من الأشخاص بشكل دائري قبل تقديمها إلى الشبكة. والأهم من ذلك، أن التحكم في إنتاج التصويت نفسه يُمنح لمفتاح عام أو نص برمجي آخر لا يمكنه التلاعب في العلاوة المقدمة للمستلمين. ويمكن تحقيق عملية التصويت بطريقة موزعة باستخدام نص برمجي في التذكرة يسمح بتوقيع العديد من الموقعين.

## 3. عناصر التصميم الثانوية

### 3.1 خوارزميات توقيع المنحنى الإهليلجي

على الرغم من أن secp256k1 يعتبر على نطاق واسع أن لديه خيارًا آمنًا لمعلمات المنحنى البيضاوي، تبقى بعض الأسئلة حول أصل المنحنى. على سبيل المثال، اختيار منحنى كوبليتز،

<div dir="ltr">
$$ (y^2 + xy = x^3 + ax^2 + b \text{ and } a = a^2 \text{, } b = b^2 \text{; } a = 1 \text{ or } 2 \text{, } b != 0) $$
</div>

وعادة ما يتم ذلك عن طريق تعداد حقول Galois ذات الامتداد الثنائي $GF(2^m)$ حيث $m$ هو عدد أولي في المجال {$0$، $...$، أكبر نهاية} و 

<div dir="ltr">
$x,y \in GF(2^m)$
</div>

المرجع [10]. بالنسبة إلى الأمن ذي 128 بت، يجب أن يكون $m$ أكبر من أو يساوي 257 وعادة ما يكون أصغر عدد أولي ممكن في هذا المجال لتسهيل عملية الحساب السريع. في هذه الحالة، يكون الاختيار الواضح لـ $m$ هو 277 و a=0؛ على الرغم من وجود قيمة $m$ المناسبة التي يتم التعرف عليها بواسطة أدوات تحديد معلمات المنحنى [11] وحقيقة أنها كانت الحل الأكثر فعالية من الناحية الحسابية، تم تحديد المعلمات  m=283 و a=0 من بين ثلاثة خيارات ممكنة:

<div dir="ltr">
$$ (m = 277\text{, } a = 0\text{; } m = 283\text{, } a = 0\text{; } m = 283\text{, } a = 1) $$
</div>

بالنسبة لجميع مواصفات منحنى Koblitz الأخرى، يتم تحديد قيمة $m$ الأكثر وضوحًا. وعلى الرغم من أن هذا الأمر يثير الفضول، إلا أنه لا توجد هجمات معروفة يمكن تطبيقها باستخدام قيمة $m$ أكبر قليلاً في حقل Galois. وأثيرت أيضا اعتراضات أخرى على المعلمات التي يستخدمها secp256k1 [12].

خوارزمية توقيع رقمي (DSA) أخرى شائعة جدًا ذات أمن 128 بت هي Ed25519 [13]. يستخدم هذا خوارزمية توقيع EdDSA على شكل منحنى مكافئ لـ Curve25519 ويتم استخدامها على نطاق واسع اليوم. على عكس secp256k1 الخاص بـ ECDSA، يستخدم Ed25519 توقيعات Schnorr الأبسط التي يمكن إثباتها بشكل آمن في نموذج Oracle عشوائي [الملحق أ].

تم اقتراح توقيعات Schnorr أيضًا على البتكوين [14]. ومع ذلك ، بدلاً من استخدام كود تشغيل حصري لتوقيعات Schnorr باستخدام معلمات المنحنى لـ secp256k1، تستخدم ديكريد بدلاً من ذلك كود تشغيل جديد OP_CHECKSIGALT للتحقق من عدد غير محدود من مخططات التوقيع الجديدة. وفي التنفيذ الحالي، تتوفر توقيعات كل من Sectp256k1 Schnorr وEd25519 لتكملة توقيعات Sectp256k1 ECDSA. في المستقبل، من السهل إضافة مخططات توقيع جديدة في تفرع ناعم، مثل تلك المخططات الآمنة من حيث الكم. كما أن توفر جناحي Schnorr هذين يسمح أيضا بإنشاء توقيعات جماعية بسيطة تحتل نفس الحيز من التوقيع العادي [15]، والتي يتم تنفيذها لكلا الجناحين. في المستقبل، ستمكّن تواقيع العتبة باستخدام المشاركة السرية بدون تاجر أيضًا توقيعات العتبة t-من-n التي تحتل نفس المساحة [16].

### 3.2 دالة الهاش

لدى SHA256، المستخدم في البتكوين عددا من العيوب التقنية بسبب بناء Merkle–Damgard. وقد أدت نقاط الضعف هذه إلى منافسة SHA3 للحصول على دالة هاش جديدة تستند إلى بنية أساسية مختلفة. وقد إختارت ديكريد BLAKE256 كدالة هاش خاصة بها، وهي المتأهلة للمنافسة النهائية [17] [18]. تعتمد دالة الهاش على بناء HAIFA الذي يتضمن مجموعة متنوعة من تشفير تدفق ChaCha بواسطة Bernstein. تتميز وظيفة الهاش بأدائها العالي على بنية x86-64 المصغرة، حيث أنها أسرع للرسائل القصيرة من SHA256 [19] على الرغم من اعتبارها ذات هامش أمان أعلى بكثير عند 14 دورة.

### 3.3 ملحقات البرنامج النصي

بالإضافة إلى  OP_CHECKSIGALT و OP_CHECKSIGALTVERIFY المذكورين سابقاً، تم إجراء تعديلات أخرى على البرمجة النصية للبتكوين. تمت إضافة إصدار بايت إلى البرامج النصية للنواتج لتمكين التفرع الناعم إلى لغات البرمجة النصية الجديدة، كما اقترحها أولاً Wuille [20]. كما تمت إعادة تمكين جميع كودات التشغيل المتعلقة بالرياضيات والمنطق وهي تعمل الآن على تسجيلات int32. كما تم تنفيذ العديد من كودات التشغيل لمعالجة سلسلة البايت وإعادة تمكينها. كما تمت إعادة استخدام كودات تشغيل البتكوين المتبقية غير المستخدمة للتفرع الناعم في المستقبل. و إصلاح بعض الأخطاء القديمة في لغة البرمجة النصية للبتكوين [21] [22].

### 3.4 عزل توقيع البرنامج النصي و تزوير الإثباتات

لمنع سوء قابلية المعاملة، والقدرة على إنشاء معاملة بنفس مراجع المدخلات والنواتج، مع معرف معاملة مختلف، تمت إزالة البرامج النصية للإدخال من حساب هاش المعاملة. وقد كانت أصول هذا التعديل مثيرة للجدل، على الرغم من أنه يبدو أنه قد تم تنفيذه في كل من العملات CryptoNote و sidechains في الماضي [3] [23]. والآن يُقترح استخدامها للبتكوين كتفرع  ناعم يشار إليه باسم "الشاهد المنفصل" [20]. كما هو الحال في تنفيذ عناصر sidechains ، يتم تضمين الالتزامات ببيانات الشاهد في شجرة ميركل للكتلة [23]. بالإضافة إلى ذلك، يتم تعيين تزوير الإثباتات، كما هو مقترح لتفرع البتكوين الناعم [20]، من قبل المعدنين ويلتزمون بها أيضًا كجزء من البيانات في شجرة ميركل.

### 3.5 ملحقات المعاملة

تمت إضافة انتهاء صلاحية المعاملة، مما يسمح للمرء بتشذيب المعاملات من مجمع الذاكرة إذا ما وصلت سلسلة الكتل إلى ارتفاع معين [24]. في السابق، كانت الطريقة الوحيدة لإزالة المعاملة من ذاكرة التجمع هي بمضاعفة إنفاقها.

### 3.6 تحسينات متنوعة

كما هو الحال في البتكوين، تتضاؤل العلاوة بطريقة أسية مع ارتفاع الكتلة. ومع ذلك، فإن خوارزمية ديكريد، على الرغم من أنها بسيطة للغاية أيضًا، تستكمل بطريقة أفضل هذا التضاؤل بمرور الوقت حتى لا تحدث صدمة في السوق عند الإنخفاض الحاد في العلاوات مثل حال CryptoNote [2]. مثل البيركوين [5] ، يتم حساب صعوبة إثبات العمل من المتوسط ​​المرجح الأسي للاختلافات في أوقات الكتلة السابقة. ومع ذلك، يتم أيضًا استكمال هذا الحساب في فترات نافذة الصعوبات الشبيهة بالبتكوين. يتم تصحيح خطأ "timewarp" في البتكوين[25]، من خلال التأكد من أن كل اختلاف في وقت الكتلة مدرج في حساب الصعوبة.

وتجدر الإشارة أيضًا إلى أن العديد من هجمات التعدين المعروفة جيدًا، مثل التعدين الأناني [26] والتعدين العنيد [27]، لن تعمل بشكل مفيد بعد الآن في نظام حيث توجد لامركزية فعالة في تعدين الحصة ولا يوجد تواطؤ معدن إثبات العمل-إثبات الحصة. هذا ببساطة لأنه من المستحيل إنشاء ملحقات سرية لـسلسلة الكتل دون مساعدة معدني الحصة.

ستشكل المرونة في التعامل مع هجمات التعدين التي سبق وصفها، وهجمات التعدين التي تم التخطيط لها حديثاً والتي تخص نظامنا، مجالاً مثمراً للبحث في المستقبل.

## المراجع

<div dir="ltr">
[1] Nakamoto N. 2008. Bitcoin: A Peer-to-Peer Electronic Cash System. Self-published.
https://decred.org/research/nakamoto2008.pdf<br>

[2] Buterin V. 2014. A Next-Generation Smart Contract and Decentralized Application Platform. Self-published.
https://decred.org/research/buterin2014.pdf<br>

[3] von Saberhagen N. 2013. CryptoNote v 2.0. Self-published.
https://decred.org/research/saberhagen2013.pdf<br>

[4] Back A., Corallo M., Dashjr L., Friedenbach M., Maxwell G., Miller A., Poelstra A., Timon A., Wuille P. 2014. Enabling Bitcoin Innovations with Pegged Sidechains. BlockStream.
https://decred.org/research/back2014.pdf<br>

[5] King S. and Nadal S. 2012. PPCoin: Peer-to-Peer CryptoCurrency with Proof-of-Stake. Self-published.
https://decred.org/research/king2012.pdf<br>

[7] Poelstra A. 2015. On Stake and Consensus. Selfpublished.
https://decred.org/research/poelstra2015.pdf<br>

[8] Bentov I., Lee C., Mizrahi A., Rosenfeld M. 2014. Proof-of-Activity: Extending Bitcoin’s Proof of Work via Proof of Stake. Proceedings of the ACM SIGMETRICS 2014 Workshop on Economics of Networked Systems, NetEcon.
https://decred.org/research/bentov2014.pdf<br>

[9] Mackenzie A. 2013. MEMCOIN2: A Hybrid Proof-of-Work, Proof-of-Stake Crypto-currency. Self-published.
https://decred.org/research/mackenzie2013.pdf<br>

[10] Pornin T. 2013. StackExchange Cryptography: Should we trust the NIST-recommended ECC parameters?
https://decred.org/research/pornin2013.pdf<br>

[11] Solinas J. 2000. Efficient Arithmetic on Koblitz Curves. Designs, Codes and Cryptography. 19(2):195-249.
https://decred.org/research/solinas2000.pdf<br>

[12] Bernstein D. and Lange T. 2014. SafeCurves: choosing safe curves for elliptic-curve cryptography.
http://safecurves.cr.yp.to<br>

[13] Bernstein D., Duif N., Lange T., Schwabe P., Yang B. 2012. High-speed high-security signatures. Journal of Cryptographic Engineering. 2:77-89.
https://decred.org/research/bernstein2012.pdf<br>

[14] Osuntokun O. 2015. OP SCHNORRCHECKSIG: Exploring Schnorr Signatures as an Alternative to ECDSA for Bitcoin. Self-published.
https://decred.org/research/osuntokun2015.pdf<br>

[15] Petersen T. 1992. Distributed Provers and Verifiable Secret Sharing Based on the Discrete Logarithm Problem. Aarhus University Ph.D. Thesis. 55-57.
https://decred.org/research/petersen1992.pdf<br>

[16] Stinson D. and Strobl R. 2001. Provably Secure Distributed Schnorr Signatures and a (t,n) Threshold Scheme for Implicit Certificates. Certicom Corporation.<br>

[17] Aumasson J., Henzen L., Meier W., Phan R. 2010. SHA-3 Proposal BLAKE. Self-published.
https://decred.org/research/aumasson2010.pdf<br>

[18] Aumasson J., Henzen L., Meier W., Phan R. 2014. The Hash Function BLAKE. Springer-Verlag Berlin Heidelberg.<br>

[19] Bernstein D. and Lange T. eBACS: ECRYPT Benchmarking of Cryptographic Systems.
http://bench.cr.yp.to<br>

[20] Wuille P. 2015. Segregated Witness for Bitcoin. Scaling Bitcoin Hong Kong.
https://prezi.com/lyghixkrguao/segregated-witness-anddeploying-it-for-bitcoin/<br>

[21] Todd P. The difficulty of writing consensus critical code: the SIGHASH SINGLE bug. Bitcoin-development mailing list.
https://decred.org/research/todd2014.pdf<br>

[22] Franco P. Understanding Bitoin, 6.3: Multisignature (M-of-N) Transactions. John Wiley Sons Inc. p. 84.<br>

[23] Maxwell G. 2015. Bringing New Elements to Bitcoin with Sidechains. SF Bitcoin Devs Meetup.
https://decred.org/research/maxwell2015.pdf<br>

[24] ByteCoin. 2010. Need OP_BLOCKNUMBER to allow “time” limited transactions.
https://decred.org/research/bytecoin2010.pdf<br>

[25] ArtForz. 2011. Re: Possible way to make a very profitable 50 plus ish attack for pools? Bitcointalk Bitcoin Forums.
https://decred.org/research/artforz2011.pdf<br>

[26] Eyal I. 2015. The Miner‘s Dilemma. In IEEE Symposium on Security and Privacy, 2015.
https://decred.org/research/eyal2015.pdf<br>

[27] Nayak K., Kumar S., Miller A., Shi E. 2015. Stubborn Mining: Generalizing Selfish Mining and Combining with an Eclipse Attack. Cryptology 2015/796.
https://decred.org/research/nayak2015.pdf<br>

[28] Wuille P. 2015. Tree Signatures: Multisig on steroids using tree signatures.
https://decred.org/research/wuille2015.pdf<br>
</div>

## الملحق أ: شنور متعدد التوقيعات

تم اقتراح توقيع شنور لـلبتكوين وقد استخدم أيضا على نطاق واسع في عملات رقمية أخرى، مثل Nxt و عملات CryptoNote. في أبسط الحالات ، يمكن وصف نظام تشفير ECDSA لتوقيع Schnorr على النحو التالي:

* $y = xG$ حيث $y$ هي نقطة المفتاح العام على المنحنى، و $x$ هي المقياس الخاص، و $G$ هي منشئ المنحنى.
* $r = kG$ حيث $r$ هي النقطة على المنحنى الناتجة عن ضرب $k$، العدد السلمي للرقم الخاص، بواسطة المولد.
* $h = H(M \|\| r)$ حيث $H$ هي دالة هاش آمنة، و $M$ هي الرسالة (عادة ما يكون الهاش 32 بايت)، و $r$ هي نقطة التشفير الموصوفة سابقًا. $\|\|$ تشير إلى التسلسل.
* $s = k - hx$ حيث $s$ هي العدد السلمي من $k - hx$.
* التوقيع هو $(r,s)$، والتحقق هو ببساطة  $H(M \|\| r) == hQ + sG$.

في ما سبق ، فإن فالجداءات مع حرف كبير (على سبيل المثال، $kG$) هي مضاعفات بعدد سلمي، وبالتالي دائمًا ما تؤدي إلى نقطة على المنحنى. ويؤدي جمع هذه النقاط إلى نقطة أخرى. إن عمليات الجمع والضرب بين الأعداد السلمية هي نفسها عمليات الضرب العادية التي يمكنك القيام بها مع أي عدد صحيح. من المهم أن نلاحظ أن ضرب نقطة في عدد سلمي يعتبر خطوة لا رجعة فيها، لأن حساب العدد السلمي من النقطة الجديدة يكون افتراضياً لمشكلة اللوغاريتم المنفصل.

يتضح مما سبق أن $r$ هي نقطة على المنحنى، بينما $s$ هي عدد سلمي. لننظر إلى مجموعة الموقعين الممثلة بـ $x_{sum} = x_1 + ... + x_n$ مع الأرقام الخاصة $k_{sum} = k_1 + ... + k_n$. المفتاح العام لمجموع العدد السلمي الخاص هو: $y = x_{sum} G$. ويكون التوقيع على هذه المبالغ (من جميع المشاركين في المجموعة) على النحو التالي: $r' = k_{sum} G s' = k_{sum} - h x_{sum}$. لتوليد هذا التوقيع ، سيتعين على جميع المشاركين مشاركة مفتاحهم الخاص وأرقامهم الخاصة مسبقًا. من الواضح أننا نريد تجنب ذلك، لذا دعنا نطلب من كل مشارك إنشاء توقيع جزئي. $r_n = k_1 G + ... + k_n G = r'$ (مجموع الأرقام الخاصة العامة التي يمكن للمشاركين نشرها بشكل فردي) $s_n = k_n - h x_n$. استبدال ذلك بالصيغ العامة للتوقيعات واستخدام جمع النقاط أو الجمع باستخدام عدد سلمي:$(k_1 - h x_1) + ... + (k_n - h x_n) = s_1 + ... + s_n = s')$ (كما هو مذكور أعلاه) (إضافة عدد سلمي بسيط؛ والقيام بتوقيع m-من-n  ليس بسيطاً. وقد اقتُرح استخدام شجرة ميركل تحتوي على جميع المبالغ الرئيسية العامة الممكنة ل m من المشاركين في هذه الحالات، مما يؤدي إلى إنشاء [28] توقيع بحجم $log(n)$.

رابط النص الأصلي: [Decred Technical Brief](https://decred.org/dcr_technical_brief.pdf)

تمت الترجمة إلى اللغة العربية بواسطة: arij@. قام بالمراجعة abdulrahman4@.
