<!DOCTYPE html>
<html lang="ar" dir="rtl"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>منظومة نافس الذكية للعلوم - المرحلة الابتدائية</title>
<style>
:root{--sk:#e8f5fc;--sk2:#bfe3f5;--sk3:#6fbfe6;--pk:#fdebf2;--pk2:#f7bdd3;--pk3:#e48bb1;--tx:#34495e}
*{box-sizing:border-box}body{margin:0;font-family:Tahoma,Arial,sans-serif;background:#fff;color:var(--tx);line-height:1.8}
header{background:linear-gradient(90deg,var(--sk),var(--pk));padding:14px 18px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:8px;border-bottom:3px solid var(--sk2)}
h1{font-size:1.2rem;margin:0}h3{margin:.2rem 0;color:#2d7ea6}h4{margin:.8rem 0 .2rem;color:var(--pk3)}
main{max-width:900px;margin:auto;padding:16px}.view{display:none}.view.on{display:block}
.card{background:#fff;border:2px solid var(--sk2);border-radius:16px;padding:14px;margin:10px 0}
.card.p{border-color:var(--pk2)}
input,select{font:inherit;padding:8px 10px;border:2px solid var(--sk2);border-radius:10px;margin:4px;max-width:100%}
button{font:inherit;cursor:pointer;border:0;border-radius:12px;padding:9px 16px;background:var(--sk3);color:#fff;margin:4px}
button.pk{background:var(--pk3)}button.lt{background:var(--sk);color:var(--tx);border:2px solid var(--sk2)}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px}
.lv{cursor:pointer;background:var(--sk);border:2px solid var(--sk2);border-radius:16px;padding:14px;text-align:center}
.lv:nth-child(even){background:var(--pk);border-color:var(--pk2)}.lv b{display:block;font-size:1.05rem}
.bar{height:10px;background:var(--sk);border-radius:9px;overflow:hidden}.bar i{display:block;height:100%;background:var(--pk3);width:0}
.opt{display:block;width:100%;text-align:right;background:var(--sk);color:var(--tx);border:2px solid var(--sk2)}
.opt.sel{background:var(--pk);border-color:var(--pk3)}.opt.ok{background:#e3f6e8;border-color:#7dc58f}.opt.no{background:#fde2e2;border-color:#e59a9a}
table{width:100%;border-collapse:collapse;font-size:.9rem}th,td{border:1px solid var(--sk2);padding:6px;text-align:center}th{background:var(--sk)}
.vis{background:var(--pk);border-radius:10px;padding:8px;font-size:1.1rem}.err{color:#c0392b}.row{margin:4px 0}
.badge{display:inline-block;background:var(--pk);border:2px solid var(--pk2);border-radius:20px;padding:4px 12px;margin:3px}
#qr svg{width:170px;height:170px}
@media print{body *{visibility:hidden}#rep,#rep *,#sr,#sr *{visibility:visible}#rep,#sr{position:absolute;top:0;right:0;left:0}.np{display:none!important}}
</style></head><body>
<header><h1>🔬 منظومة نافس الذكية للعلوم - المرحلة الابتدائية</h1>
<div><button class="lt" onclick="show('home')">الرئيسية</button><button class="pk" onclick="dash()">لوحة المعلمة</button></div></header>
<main>
<section id="home" class="view on">
<div class="card"><h3>بيانات الطالبة (إلزامية)</h3>
<input id="nm" placeholder="اسم الطالبة"><input id="gr" placeholder="الصف الدراسي"><div id="he" class="err"></div></div>
<h3>اختاري الاختبار (25 سؤالاً لكل اختبار)</h3><div class="grid" id="lvs"></div></section>

<section id="quiz" class="view"><div class="card"><div id="qh"></div><div class="bar"><i id="pb"></i></div>
<h3 id="qt"></h3><div id="qo"></div><button id="nx" onclick="nextQ()" disabled>التالي</button></div></section>

<section id="res" class="view"><div class="card" id="rs"></div><div id="pl"></div></section>

<section id="dsh" class="view"><div id="rep"><div class="card"><h3>لوحة المعلمة - أرشيف الشواهد</h3>
<div id="tb"></div>
<div class="np card p"><b>📥 استيراد نتائج الطالبات</b><br>الصقي رسائل الواتساب التي أرسلتها الطالبات (يمكن لصق عدة رسائل معاً):<textarea id="im" rows="3" style="width:100%"></textarea><button onclick="imp()">إضافة النتائج</button><span id="ie"></span></div>
<div style="margin-top:12px"><div class="np">رابط المنصة للرمز: <input id="ul" size="40" dir="ltr"><button class="lt" onclick="drawQR()">تحديث الرمز</button></div>
<div id="qr"></div><div id="qe" class="err"></div></div></div></div>
<button class="pk np" onclick="window.print()">طباعة أو حفظ التقرير (شاهد رسمي)</button></section>
</main>
<script>
const $=id=>document.getElementById(id),esc=s=>String(s).replace(/[&<>"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const sh=a=>{a=a.slice();for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]]}return a};
const K={g:{n:"الغذاء والبيئة",sum:"النبات منتج يصنع غذاءه بالشمس، والمستهلكات تتغذى على غيرها، والمحللات تحلل بقايا الأحياء الميتة. الشبكة الغذائية سلاسل متداخلة، وتقل الطاقة عند كل انتقال.",vis:"☀️ → 🌱 → 🐛 → 🐦 → 🦅 ↘ 🍄",k:"ارسمي شبكة غذائية من 6 كائنات بأسهم تدل على اتجاه انتقال الطاقة.",p:[["المنتج","يصنع غذاءه"],["المستهلك","يتغذى على غيره"],["المحلل","يحلل بقايا الأحياء"]],f:["تبدأ الطاقة في السلسلة الغذائية من ___","الشمس","الماء","التربة"]},
b:{n:"أجهزة الجسم",sum:"تتعاون الأجهزة: الهضمي يمتص الغذاء، والتنفسي يوفر الأكسجين، والدوري ينقلهما للخلايا، والعصبي ينسق الإحساس والحركة، والهيكلي والعضلي يدعمان الحركة.",vis:"🍎 → 🫁 → ❤️ → 🧠 → 💪",k:"اجري في مكانك دقيقة وقيسي نبضك قبل الجري وبعده.",p:[["القلب","يضخ الدم"],["الرئتان","تبادل الغازات"],["الدماغ","مركز التحكم"]],f:["ينقل الجهاز ___ الأكسجين والغذاء إلى الخلايا","الدوري","الهضمي","الهيكلي"]},
c:{n:"الخلية والوراثة",sum:"الخلية وحدة البناء في الكائنات؛ النواة تتحكم، والغشاء ينظم دخول المواد وخروجها، والبلاستيدات في النبات للبناء الضوئي. الصفات الوراثية تنتقل بالجينات، أما المكتسبة فتُتعلَّم.",vis:"🧬 جينات ← أبوان ← صفات موروثة | 📚 تعلم ← مهارات مكتسبة",k:"صنفي 6 صفات إلى وراثية ومكتسبة في جدول.",p:[["النواة","تتحكم بالخلية"],["البلاستيدات","تصنع الغذاء في النبات"],["الغشاء الخلوي","ينظم دخول المواد"]],f:["لون العينين صفة ___","وراثية","مكتسبة","متعلمة"]},
m:{n:"المادة وخواصها",sum:"للمادة ثلاث حالات: صلبة وسائلة وغازية. التبخر والتكثف والانصهار تغيرات فيزيائية، أما الصدأ والاحتراق فتغيرات كيميائية تنتج مواد جديدة. الكثافة = الكتلة ÷ الحجم.",vis:"🧊 ⇄ 💧 ⇄ ☁️",k:"ذوّبي ملحاً في الماء ثم افصليه بالتبخير.",p:[["التبخر","سائل إلى غاز"],["الانصهار","صلب إلى سائل"],["الصدأ","تغير كيميائي"]],f:["الكثافة = الكتلة ÷ ___","الحجم","الطول","الزمن"]},
e:{n:"الطاقة والكهرباء",sum:"الدائرة الكهربائية تحتاج مصدراً وأسلاكاً وحملاً ودائرة مغلقة. المواد الموصلة تمرر التيار والعازلة لا تمرره. تتحول الطاقة من شكل لآخر ولا تفنى.",vis:"🔋 → ➖ → 💡 → ➖ ↩",k:"ركّبي دائرة بسيطة وجربي مواد موصلة وعازلة.",p:[["البطارية","مصدر الطاقة"],["النحاس","موصل"],["المطاط","عازل"]],f:["لا تفنى الطاقة بل ___ من شكل لآخر","تتحول","تختفي","تتضاعف"]},
s:{n:"الأرض والفضاء",sum:"دوران الأرض حول نفسها يسبب الليل والنهار، ودورانها حول الشمس مع ميل المحور يسبب الفصول. تتغير أطوار القمر بحسب الجزء المضاء الذي نراه.",vis:"🌍 ↻ ☀️ | 🌑 🌓 🌕",k:"استخدمي كرة ومصباحاً لتمثيل الليل والنهار.",p:[["دوران الأرض حول نفسها","الليل والنهار"],["ميل المحور","الفصول"],["الكسوف الشمسي","القمر بين الشمس والأرض"]],f:["تتكرر أطوار القمر كل ___ تقريباً","شهر","يوم","سنة"]},
i:{n:"الاستقصاء العلمي",sum:"خطواته: تحديد المشكلة، الفرضية، تنفيذ التجربة بتغيير متغير واحد مع تثبيت الباقي، تسجيل النتائج، ثم الاستنتاج وإعادة التجربة للتأكد.",vis:"❓ → 💡 → 🔬 → 📝 → ✅",k:"صمّمي تجربة صغيرة وحددي المتغير المستقل والتابع.",p:[["الفرضية","إجابة مقترحة تُختبر"],["المتغير المستقل","ما نغيره عمداً"],["المتغير التابع","ما نقيسه"]],f:["نغيّر ___ واحداً فقط في كل تجربة","متغيراً","كل المتغيرات","الأدوات"]},
d:{n:"تحليل البيانات",sum:"نقرأ الجداول والرسوم لنقارن ونستنتج. المتوسط = مجموع القيم ÷ عددها. الخط البياني يناسب تغير القيمة مع الزمن، والأعمدة تناسب المقارنة.",vis:"📊 أعمدة = مقارنة | 📈 خط = تغير",k:"اجمعي أطوال زميلاتك واحسبي المتوسط وارسميها.",p:[["الأعمدة","مقارنة بين مجموعات"],["الخط البياني","تغير مع الزمن"],["المتوسط","المجموع ÷ العدد"]],f:["أنسب تمثيل لتغير الحرارة مع الزمن هو الخط ___","البياني","الدائري للأسماء","الصوري"]}};
/* الصيغة: السؤال|الإجابة الصحيحة|خاطئة|خاطئة|المهارة */
const L=[{t:"الاختبار التمهيدي",s:"الأساسيات",q:[
"ما الكائن الذي يصنع غذاءه بنفسه؟|النبات|الأرنب|الصقر|g","يسمى الحيوان الذي يأكل النبات فقط:|آكل أعشاب|آكل لحوم|آكل كل شيء|g","المصدر الأول للطاقة في السلسلة الغذائية:|الشمس|التربة|الماء|g","الكائنات التي تحلل بقايا الأحياء الميتة:|المحللات|المنتجات|المستهلكات|g",
"أي مما يلي ضمن الجهاز الهضمي؟|المعدة|الرئتان|القلب|b","العضو الذي يضخ الدم في الجسم:|القلب|الكبد|المعدة|b","يتم تبادل الغازات في:|الرئتين|الكلى|الأمعاء|b","الجمجمة تحمي:|الدماغ|القلب|الرئتين|b",
"الوحدة الأساسية في بناء الكائنات الحية:|الخلية|الذرة|العضو|c","الجزء الذي يتحكم بأنشطة الخلية:|النواة|الجدار الخلوي|الفجوة|c","انتقال الصفات من الآباء إلى الأبناء يسمى:|الوراثة|التبخر|الهضم|c",
"أي مما يلي من حالات المادة؟|الصلبة|الحركية|الضوئية|m","تحول الماء السائل إلى بخار يسمى:|التبخر|التجمد|الانصهار|m","وحدة قياس الكتلة:|الكيلوجرام|اللتر|المتر|m",
"مادة موصلة للكهرباء:|النحاس|الخشب|البلاستيك|e","مصدر طبيعي للضوء والحرارة:|الشمس|المرآة|القمر|e","جزء الدائرة الذي يعد مصدر الطاقة:|البطارية|السلك|المصباح|e",
"يدور القمر حول:|الأرض|الشمس|المريخ|s","سبب تعاقب الليل والنهار:|دوران الأرض حول نفسها|دوران القمر|دوران الأرض حول الشمس|s","أولى خطوات الطريقة العلمية:|تحديد المشكلة|كتابة النتيجة|تنفيذ التجربة|i",
"أداة قياس درجة الحرارة:|الترمومتر|المسطرة|الميزان|i","الفرضية هي:|إجابة مقترحة قابلة للاختبار|نتيجة نهائية مؤكدة|سؤال بلا إجابة|i","المتغير الذي نقيسه في التجربة يسمى:|المتغير التابع|المتغير المستقل|الثابت|d","التمثيل الذي يقارن بين القيم بأعمدة:|الأعمدة البيانية|الميزان|المجهر|d"]},
{t:"الاختبار الاستقصائي",s:"التطبيق",q:[
"في السلسلة عشب ← جندب ← ضفدع ← ثعبان، المستهلك الثاني هو:|الضفدع|الجندب|الثعبان|g","إذا قلّت الضفادع فعدد الجنادب غالباً:|يزداد|يقل|لا يتغير|g","الصقر في سلسلة نبات ← فأر ← صقر يعد:|مستهلكاً ثانوياً|منتجاً|محللاً|g","الشبكة الغذائية أكثر ترابطاً من السلسلة لأن:|الكائن يتغذى على أكثر من نوع|الشمس تغيب|الماء قليل|g",
"تمتص الأمعاء الدقيقة:|الغذاء المهضوم|الأكسجين|الفضلات الصلبة|b","يزداد التنفس عند الجري لأن الجسم:|يحتاج أكسجيناً أكثر|يحتاج ماء أقل|يريد النوم|b","الجهازان اللذان يوصلان الأكسجين للخلايا:|التنفسي والدوري|الهضمي والعصبي|الهيكلي والبولي|b","نحس بحرارة الماء بواسطة:|الجلد والأعصاب|العظام|الرئتين|b",
"يوجد في الخلية النباتية ولا يوجد في الحيوانية:|الجدار الخلوي|النواة|الغشاء الخلوي|c","تقوم البلاستيدات الخضراء بـ:|البناء الضوئي|الهضم|الحركة|c","لون العينين صفة:|وراثية|مكتسبة|متعلمة|c","ركوب الدراجة صفة:|مكتسبة|وراثية|جينية|c",
"تكاثف بخار الماء على غطاء إناء ساخن يسمى:|تكثف|انصهار|تجمد|m","لفصل الملح عن الماء المالح نستخدم:|التبخير|المغناطيس|الميزان|m","لفصل برادة الحديد عن الرمل نستخدم:|المغناطيس|التبخير|الميزان|m",
"عند فتح مفتاح الدائرة الكهربائية:|يتوقف التيار|يزداد التيار|ينعكس التيار|e","مادة عازلة للكهرباء:|المطاط|الحديد|الألمنيوم|e","تتحول الطاقة الكهربائية في المروحة إلى طاقة:|حركية|كيميائية|نووية|e",
"يحدث الكسوف الشمسي عندما يمر:|القمر بين الأرض والشمس|الأرض بين الشمس والقمر|الشمس بين الأرض والقمر|s","فصول السنة سببها:|ميل محور الأرض مع دورانها حول الشمس|قرب القمر|الرياح|s",
"لاختبار أثر الضوء على نمو النبات نغيّر:|كمية الضوء فقط|الضوء والماء والتربة معاً|لا شيء|i","لتكون التجربة عادلة نبقي:|بقية المتغيرات ثابتة|كل شيء متغيراً|الأدوات بلا تسجيل|i","بعد انتهاء التجربة:|نسجل النتائج ونفسرها|نتجاهلها|نغيرها دون دليل|i",
"نبتة طولها 2 سم يوم1، و4 سم يوم2، و6 سم يوم3؛ النمو اليومي:|2 سم|3 سم|6 سم|d","أنسب تمثيل لتغير الحرارة خلال اليوم:|الخط البياني|قائمة أسماء|صورة|d"]},
{t:"اختبار التفكير العميق",s:"تحليل البيانات",q:[
"أرانب: 40، 60، 80 خلال ثلاث سنوات، وثعالب: 5، 4، 3. نستنتج أن:|قلة الثعالب ترتبط بزيادة الأرانب|الأرانب تزداد بزيادة الثعالب|لا علاقة بينهما|g","عشب ← جندب ← ضفدع ← صقر، وعشب ← أرنب ← صقر. إن اختفى الجندب فالأكثر تأثراً مباشرة:|الضفدع|الأرنب|العشب|g","تنتقل نحو 10% من الطاقة لكل مستوى؛ إن حمل العشب 1000 وحدة فالجندب يحصل على نحو:|100|500|10|g","ماتت 90 من كل 100 نبتة بسبب الجفاف؛ الأثر على المستهلكات:|يقل غذاؤها|يزداد غذاؤها|لا تتأثر|g",
"النبض: 75 في الراحة و120 بعد الجري. نستنتج أن:|الجهد يرفع النبض|الجري يخفضه|لا علاقة|b","التنفس 16 مرة في الراحة و30 بعد الجري؛ الفرق:|14|46|16|b","ما تفسير ارتفاع النبض بعد الجري؟|الخلايا العضلية تحتاج طاقة أكثر|الدم يقل|المعدة تمتلئ|b","بدأ هضم النشا في الفم، فاللعاب:|يحتوي إنزيمات|يحتوي دماً|يقتل الميكروبات فقط|b",
"خلية فيها جدار خلوي وبلاستيدات؛ هي خلية:|نباتية|حيوانية|لا نواة لها|c","ظهر اللون البني في 8 من 10 أبناء؛ هذا يدل غالباً على:|صفة سائدة|صفة مكتسبة|صدفة تامة|c","أي معطى يدل على صفة مكتسبة؟|إتقان العزف بالتدريب|لون الشعر|فصيلة الدم|c","خلية جسم الإنسان فيها 46 كروموسوماً، فالخلية الجنسية فيها:|23|46|92|c",
"كتلة 50 جم وحجم 10 سم³؛ الكثافة:|5 جم/سم³|0.2 جم/سم³|500 جم/سم³|m","مادة كثافتها 2 جم/سم³ في الماء:|تغرق|تطفو|تذوب فقط|m","ارتفعت الحرارة من 20 إلى 60 في 4 دقائق؛ المعدل:|10 درجات/دقيقة|20 درجة/دقيقة|4 درجات/دقيقة|m",
"كلما زادت البطاريات 1، 2، 3 زاد توهج المصباح؛ العلاقة:|طردية|عكسية|لا علاقة|e","احترق مصباح في دائرة فانطفأت كل المصابيح؛ التوصيل:|على التوالي|على التوازي|لا توجد دائرة|e","لوح شمسي: 200 وحدة ظهراً و50 عصراً؛ أفضل وقت:|الظهيرة|العصر|متساويان|e",
"طول الظل: 8 م صباحاً، 1 م ظهراً، 6 م عصراً. أقصر ظل عند:|الظهر|الصباح|العصر|s","تحدث أطوار القمر بسبب:|اختلاف الجزء المضاء الذي نراه|ظل الأرض|الغيوم|s","كم دورة كاملة تدورها الأرض حول نفسها في 3 أيام؟|3|24|72|s",
"تغيير أكثر من متغير في تجربة واحدة يجعل النتائج:|يصعب تفسيرها|أدق|أسرع|i","كررت طالبة التجربة 3 مرات فكانت 10، 10، 11؛ هذا يزيد:|الثقة بالنتيجة|خطأ الفرضية|عطل الأداة|i","في الأعمدة البيانية، أطول عمود يمثل:|أعلى قيمة|أقل قيمة|المتوسط|d","متوسط القيم 4 و6 و8 و10 هو:|7|8|6|d"]},
{t:"اختبار التحدي الأقصى",s:"المهارات المتقدمة",q:[
"عند إدخال مفترس جديد إلى بيئة، الأثر الأرجح:|اختلال التوازن في الأعداد|ثبات تام|توقف الشمس|g","لا تتجاوز حلقات السلسلة عادة 4 أو 5 لأن:|الطاقة تقل عند كل انتقال|الشمس تضعف|الماء ينفد|g","أنسب إجراء لتقليل أثر التلوث على الشبكة:|حماية المنتجات لأنها أساس الطاقة|حماية المفترسات فقط|إزالة المحللات|g","عند اختفاء المحللات يتراكم:|بقايا الأحياء الميتة|الأكسجين فقط|غذاء النبات الجاهز|g",
"يتخلص الجسم من حرارة زائدة أثناء الرياضة عبر:|التعرق|زيادة الأكل|توقف التنفس|b","أدق سبب لزيادة سرعة القلب عند الجهد:|حاجة الخلايا لأكسجين وغذاء أكثر|امتلاء المعدة|قلة الماء|b","بعد الأكل ينتقل الجلوكوز إلى الخلايا عبر الجهاز:|الدوري|الهيكلي|الجلد|b","إذا تعطلت الأعصاب في الذراع يتأثر:|الإحساس والحركة|الهضم|التنفس|b",
"غياب البلاستيدات عن نبات يقلل قدرته على:|صنع الغذاء|الامتصاص|النمو للأعلى|c","قد يختلف الأبناء عن والديهم بسبب:|اختلاف تركيب الجينات الموروثة|التغذية فقط|الحظ|c","وظيفة الغشاء الخلوي:|تنظيم دخول المواد وخروجها|صنع الغذاء|حفظ المعلومات|c","أي مما يلي لا ينتقل بالوراثة؟|ندبة من حادث|لون البشرة|شكل الأذن|c",
"إذابة السكر في الماء تنتج:|محلولاً|عنصراً جديداً|مادة كيميائية جديدة|m","أي مما يلي تغير كيميائي؟|صدأ الحديد|ذوبان الجليد|تقطيع الورق|m","دليل التغير الكيميائي:|تكون مادة جديدة|تغير الشكل|تغير الحجم|m","للمقارنة العادلة بين كثافة سائلين نقيس:|الكتلة والحجم لكل سائل|اللون|الاسم|m",
"ميزة التوصيل على التوازي:|تعمل بقية المصابيح إذا احترق أحدها|تضعف الإضاءة دائماً|لا تحتاج مصدراً|e","الطاقة لا تفنى ولا تُستحدث بل:|تتحول من شكل لآخر|تختفي|تتضاعف|e","أفضل مصدر متجدد للطاقة:|الطاقة الشمسية|الفحم|النفط|e",
"نرى القمر يتغير خلال الشهر لأننا نرى:|جزءاً مختلفاً من نصفه المضاء|ظل الأرض كل يوم|قمراً يصغر|s","الصيف أطول نهاراً لأن:|ميل المحور يعرض نصف الكرة للشمس أكثر|الأرض أقرب دائماً|القمر أكبر|s",
"أفضل صياغة للفرضية:|إذا زاد الضوء فسينمو النبات أكثر|النبات جميل|الضوء مفيد دائماً|i","عند ظهور نتائج غير متوقعة:|نراجع الخطوات ونعيد التجربة|نحذفها|نغيّر النتائج|i",
"خط بياني يصعد ثم يثبت يعني أن القيمة:|زادت ثم استقرت|تناقصت|عشوائية|d","نتيجتان لقياسين: 8 و20؛ الأدق للحكم:|إعادة القياس وتكرار التجربة|اختيار الأكبر|اختيار الأصغر|d"]}];
const EN=[["تحدي STEM: لتصنعي مجففاً شمسياً للفواكه، أي لون للسطح يمتص حرارة أكثر؟","الأسود","الأبيض","الشفاف","🌞 شارة مهندسة الطاقة"],
["تحدي هندسة: أي تغيير يزيد تحمل جسر ورقي؟","طي الورقة على شكل أمواج","فردها مسطحة","تقليل الأعمدة","🌉 شارة المهندسة المبتكرة"],
["لغز: أبدأ السلسلة ولا آكل أحداً، أصنع غذائي بالضوء، من أنا؟","النبات","الصقر","الفطر","🌱 شارة حلّالة الألغاز"],
["لغز تفكير عليا: إذا اختفت كل المحللات من غابة فماذا يحدث؟","تتراكم البقايا وتقل مغذيات التربة","تزداد المغذيات","لا شيء","🍄 شارة المفكرة العليا"],
["تحدي استقصاء: لاختبار سرعة ذوبان السكر بتغيير متغير واحد نغيّر:","حرارة الماء فقط","الماء والكأس والكمية معاً","كل شيء","⚗️ شارة العالمة الصغيرة"]];
/* للإرسال التلقائي للمعلمة: الصقي هنا رابط نموذج Formspree الخاص بك بين علامتي الاقتباس، مثال: "https://formspree.io/f/xxxxxxx" */
const TEACHER_ENDPOINT="";
const S={};let mem=[];
function repText(r,f,ok){return `تقرير نافس - علوم\nالطالبة: ${r.n}\nالصف: ${r.g}\nالاختبار: ${r.l}\nالدرجة: ${r.sc}\nالمهارات التي حدث فيها إخفاق: ${r.f.length?r.f.join('، '):'لا يوجد'}\nالخطة المخصصة: ${r.p}\nالتاريخ: ${r.d}\nرمز النتيجة (للاستيراد في لوحة المعلمة): NF:${btoa(encodeURIComponent(JSON.stringify(r)))}`}
function repHTML(r,f,ok){return `<h3>تقرير ${esc(r.p)} - علوم الصف السادس</h3><p>الطالبة: <b>${esc(r.n)}</b> • الصف: ${esc(r.g)} • التاريخ: ${esc(r.d)}</p><p>الاختبار: ${esc(r.l)} • الدرجة: <b>${esc(r.sc)}</b></p>`+(f.length?`<p>المهارات التي حدث فيها إخفاق: ${esc(r.f.join('، '))}</p>`:'<p>لا توجد مهارات إخفاق.</p>')+(ok?'<p>🌟 أتمّت الطالبة الاختبار بتميز وتم توجيهها إلى مسار الإثراء.</p>':f.map(k=>{const c=K[k];return `<div style="border:1px solid #bfe3f5;border-radius:10px;padding:8px;margin:6px 0"><b>${c.n}</b><p>📖 ${c.sum}</p><p>👁️ ${c.vis}</p><p>✋ ${c.k}</p></div>`}).join(''))+'<p><small>منظومة نافس الذكية للعلوم - المرحلة الابتدائية</small></p>'}
function autoSend(r,t){const bad=()=>{$('sst').textContent='تعذر الإرسال التلقائي، استخدمي زر المشاركة'};fetch(TEACHER_ENDPOINT,{method:'POST',headers:{'Content-Type':'application/json',Accept:'application/json'},body:JSON.stringify({name:r.n,grade:r.g,test:r.l,score:r.sc,message:t})}).then(x=>{$('sst').textContent=x.ok?'✅ تم إرسال التقرير إلى المعلمة تلقائياً':'تعذر الإرسال التلقائي، استخدمي زر المشاركة';if(!x.ok)bad()}).catch(bad)}
async function shareRep(){const h='<html dir="rtl" lang="ar"><meta charset="utf-8"><body style="font-family:Tahoma">'+S.rh+'</body></html>';
try{const fl=new File([h],'nafes-report.html',{type:'text/html'});if(navigator.canShare&&navigator.canShare({files:[fl]})){await navigator.share({files:[fl],title:'تقرير نافس',text:S.rt});return}if(navigator.share){await navigator.share({title:'تقرير نافس',text:S.rt});return}}catch(e){if(e.name=='AbortError')return}
location.href='mailto:?subject='+encodeURIComponent('تقرير نافس')+'&body='+encodeURIComponent(S.rt)}
const ld=()=>{try{return JSON.parse(localStorage.getItem('nafes')||'[]')}catch(e){return mem}};
const sv=a=>{mem=a;try{localStorage.setItem('nafes',JSON.stringify(a))}catch(e){}};
function show(id){document.querySelectorAll('.view').forEach(v=>v.classList.toggle('on',v.id==id));scrollTo(0,0)}
$('lvs').innerHTML=L.map((l,i)=>`<div class="lv" onclick="start(${i})"><b>المستوى ${['الأول','الثاني','الثالث','الرابع'][i]}</b>${l.t}<br><small>${l.s} • ${l.q.length} سؤالاً</small></div>`).join('');
function start(i){const n=$('nm').value.trim(),g=$('gr').value.trim();if(!n||!g){$('he').textContent='يرجى إدخال اسم الطالبة والصف الدراسي أولاً.';return}$('he').textContent='';
S.n=n;S.g=g;S.l=i;S.i=0;S.w={};S.sc=0;
S.qs=L[i].q.map(r=>{const p=r.split('|');return{q:p[0],a:p[1],o:sh([p[1],p[2],p[3]]),k:p[4]}});show('quiz');showQ()}
function showQ(){const q=S.qs[S.i];S.pick=null;$('qh').innerHTML=`<small>${L[S.l].t} • السؤال ${S.i+1} من ${S.qs.length}</small>`;$('pb').style.width=(S.i/S.qs.length*100)+'%';
$('qt').textContent=q.q;$('qo').innerHTML='';q.o.forEach(o=>{const b=document.createElement('button');b.className='opt';b.textContent=o;b.onclick=()=>{S.pick=o;[...$('qo').children].forEach(x=>x.classList.remove('sel'));b.classList.add('sel');$('nx').disabled=false};$('qo').appendChild(b)});
$('nx').disabled=true;$('nx').textContent=S.i==S.qs.length-1?'إنهاء الاختبار':'التالي'}
function nextQ(){const q=S.qs[S.i];if(S.pick===q.a)S.sc++;else S.w[q.k]=1;S.i++;S.i<S.qs.length?showQ():finish()}
function finish(){const pct=Math.round(S.sc/25*100),ok=pct>=80,f=Object.keys(S.w);
const rec={n:S.n,g:S.g,l:L[S.l].t,sc:S.sc+'/25 ('+pct+'%)',f:f.map(k=>K[k].n),p:ok?'مسار الإثراء':'الخطة العلاجية',d:new Date().toLocaleDateString('ar-SA')};
const a=ld();a.push(rec);sv(a);
$('rs').innerHTML=`<h3>${esc(S.n)} - ${L[S.l].t}</h3><p style="font-size:1.3rem">الدرجة: <b>${S.sc} من 25</b> (${pct}%)</p><p>${ok?'🌟 أداء متميز! تم توجيهك إلى مسار الإثراء.':'📘 لنعزز المفاهيم معاً! تم توجيهك إلى الخطة العلاجية.'}</p>`+(f.length?`<p>مهارات تحتاج إلى تعزيز: ${rec.f.join('، ')}</p>`:'')+`<button onclick="show('home')">اختبار آخر</button>`;
S.rt=repText(rec,f,ok);S.rh=repHTML(rec,f,ok);
$('rs').innerHTML+=`<div class="card"><b>📤 تقرير الطالبة للمعلمة</b><p id="sst">${TEACHER_ENDPOINT?'جارٍ إرسال التقرير إلى المعلمة...':''}</p><p><button class="pk" onclick="shareRep()">مشاركة التقرير مع المعلمة</button><button class="lt" onclick="window.print()">طباعة / حفظ PDF</button></p></div><div id="sr" class="card">${S.rh}</div>`;
if(TEACHER_ENDPOINT)autoSend(rec,S.rt);
$('pl').innerHTML=ok?enrich():remed(f);show('res')}
function imp(){const t=$('im').value,a=ld();let n=0;(t.match(/NF:[A-Za-z0-9+\/=]+/g)||[]).forEach(c=>{try{const r=JSON.parse(decodeURIComponent(atob(c.slice(3))));if(r&&r.n&&!a.some(x=>JSON.stringify(x)==JSON.stringify(r))){a.push(r);n++}}catch(e){}});
sv(a);$('im').value='';$('ie').textContent=n?`تمت إضافة ${n} نتيجة`:'لم يتم العثور على نتائج جديدة صالحة';dash()}
function remed(f){return '<h3>الخطة العلاجية</h3>'+f.map(k=>{const c=K[k],ds=sh(c.p.map(x=>x[1]));
const m=c.p.map(x=>`<div class="row"><b>${x[0]}</b> ← <select data-a="${x[1]}"><option value="">اختاري</option>${ds.map(d=>`<option>${d}</option>`).join('')}</select></div>`).join('');
const t=c.f[0].split('___'),fo=sh(c.f.slice(1));
return `<div class="card p"><h3>${c.n}</h3><p>📖 <b>ملخص:</b> ${c.sum}</p><p class="vis">👁️ <b>ملخص بصري:</b> ${c.vis}</p><p>✋ <b>نشاط حركي:</b> ${c.k}</p><h4>صلي المفهوم بما يناسبه</h4>${m}<h4>أكملي الفراغ</h4><div class="row">${t[0]}<select data-a="${c.f[1]}"><option value="">اختاري</option>${fo.map(d=>`<option>${d}</option>`).join('')}</select>${t[1]||''}</div><button onclick="chk(this)">تحقق</button> <span class="r"></span></div>`}).join('')}
function chk(b){const c=b.parentNode;let ok=0;const s=c.querySelectorAll('select');s.forEach(x=>{const r=x.value===x.dataset.a;if(r)ok++;x.style.borderColor=r?'#7dc58f':'#e59a9a'});c.querySelector('.r').textContent=ok==s.length?'🎉 أحسنتِ! جميع إجاباتك صحيحة':`أصبتِ في ${ok} من ${s.length}، راجعي الملخص وحاولي مجدداً`}
function enrich(){S.b=[];return `<h3>مسار الإثراء</h3><div class="card"><b>شاراتك:</b> <span id="bd">لم تحصلي على شارات بعد</span></div>`+EN.map((e,i)=>`<div class="card"><p><b>${i+1}.</b> ${e[0]}</p>${sh([e[1],e[2],e[3]]).map(o=>`<button class="opt" onclick="en(this,${i},${o==e[1]})">${o}</button>`).join('')}</div>`).join('')}
function en(b,i,r){const p=b.parentNode;if(p.dataset.d)return;p.dataset.d=1;b.classList.add(r?'ok':'no');
if(r){S.b.push(EN[i][4]);$('bd').innerHTML=S.b.map(x=>`<span class="badge">${x}</span>`).join('')}else{[...p.querySelectorAll('.opt')].find(x=>x.textContent==EN[i][1]).classList.add('ok')}}
function dash(){const a=ld();$('tb').innerHTML=a.length?`<table><tr><th>اسم الطالبة</th><th>الاختبار المُجتاز</th><th>المهارات التي حدث فيها إخفاق</th><th>الخطة المخصصة</th></tr>`+a.map(r=>`<tr><td>${esc(r.n)}<br><small>${esc(r.g)}</small></td><td>${esc(r.l)}<br><small>${esc(r.sc)}</small></td><td>${r.f.length?esc(r.f.join('، ')):'لا يوجد'}</td><td>${esc(r.p)}</td></tr>`).join('')+'</table>':'<p>لا توجد نتائج مسجلة بعد.</p>';
show('dsh');if(!$('ul').value)$('ul').value=location.href.split('#')[0];try{drawQR()}catch(e){$('qe').textContent='تعذر رسم الرمز، تأكدي من الرابط'}}
/* مولد رمز QR (النسخ 1-5، تصحيح L، وضع البايت) */
function qr(t){const b=[...new TextEncoder().encode(t)],V=[[17,19,7],[32,34,10],[53,55,15],[78,80,20],[106,108,26]],v=V.findIndex(x=>b.length<=x[0]);if(v<0)return null;
const dc=V[v][1],ec=V[v][2],n=21+4*v,bits=[],push=(x,l)=>{for(let i=l-1;i>=0;i--)bits.push((x>>i)&1)};
push(4,4);push(b.length,8);b.forEach(x=>push(x,8));push(0,Math.min(4,dc*8-bits.length));while(bits.length%8)bits.push(0);
const d=[];for(let i=0;i<bits.length;i+=8)d.push(parseInt(bits.slice(i,i+8).join(''),2));for(let p=0xEC;d.length<dc;p^=0xFD)d.push(p);
const mu=(x,y)=>{let z=0;for(let i=7;i>=0;i--){z=(z<<1)^((z>>7)*285);z^=((y>>i)&1)*x}return z};
const g=Array(ec).fill(0);g[ec-1]=1;let r=1;for(let i=0;i<ec;i++){for(let j=0;j<ec;j++){g[j]=mu(g[j],r);if(j+1<ec)g[j]^=g[j+1]}r=mu(r,2)}
const e=Array(ec).fill(0);d.forEach(x=>{const f=x^e.shift();e.push(0);g.forEach((c,i)=>e[i]^=mu(c,f))});const cw=d.concat(e);
const m=Array.from({length:n},()=>Array(n).fill(0)),F=Array.from({length:n},()=>Array(n).fill(0));
const set=(x,y,w)=>{if(x>=0&&y>=0&&x<n&&y<n){m[y][x]=w?1:0;F[y][x]=1}};
for(let i=0;i<n;i++){set(6,i,i%2==0);set(i,6,i%2==0)}
[[3,3],[n-4,3],[3,n-4]].forEach(([cx,cy])=>{for(let dy=-4;dy<=4;dy++)for(let dx=-4;dx<=4;dx++){const k=Math.max(Math.abs(dx),Math.abs(dy));set(cx+dx,cy+dy,k!=2&&k!=4)}});
if(v>0){const c=n-7;for(let dy=-2;dy<=2;dy++)for(let dx=-2;dx<=2;dx++)set(c+dx,c+dy,Math.max(Math.abs(dx),Math.abs(dy))!=1)}
const fmt=()=>{let q=8;for(let i=0;i<10;i++)q=(q<<1)^((q>>9)*0x537);const bt=((8<<10)|q)^0x5412,gb=i=>(bt>>i)&1;
for(let i=0;i<=5;i++)set(8,i,gb(i));set(8,7,gb(6));set(8,8,gb(7));set(7,8,gb(8));for(let i=9;i<15;i++)set(14-i,8,gb(i));
for(let i=0;i<8;i++)set(n-1-i,8,gb(i));for(let i=8;i<15;i++)set(8,n-15+i,gb(i));set(8,n-8,1)};
fmt();let k=0;
for(let rt=n-1;rt>=1;rt-=2){if(rt==6)rt=5;for(let vt=0;vt<n;vt++)for(let j=0;j<2;j++){const x=rt-j,y=((rt+1)&2)==0?n-1-vt:vt;if(!F[y][x]&&k<cw.length*8){m[y][x]=(cw[k>>3]>>(7-(k&7)))&1;k++}}}
for(let y=0;y<n;y++)for(let x=0;x<n;x++)if(!F[y][x]&&(x+y)%2==0)m[y][x]^=1;
fmt();return m}
function drawQR(){const t=$('ul').value.trim(),m=t&&qr(t);$('qe').textContent='';
if(!m){$('qr').innerHTML='';$('qe').textContent=t?'الرابط طويل جداً (الحد 106 حرفاً)؛ اكتبي رابطاً أقصر.':'';return}
const n=m.length,z=4;let p='';m.forEach((row,y)=>row.forEach((c,x)=>{if(c)p+=`M${x+z},${y+z}h1v1h-1z`}));
$('qr').innerHTML=`<svg viewBox="0 0 ${n+2*z} ${n+2*z}" xmlns="http://www.w3.org/2000/svg" shape-rendering="crispEdges"><rect width="100%" height="100%" fill="#fff"/><path d="${p}" fill="#000"/></svg><div><small>رمز استجابة سريع (QR) لرابط المنصة</small></div>`}
</script></body></html>
