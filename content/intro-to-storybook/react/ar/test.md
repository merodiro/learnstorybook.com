---
title: 'إختبر مكونات واجهة المستخدم'
tocTitle: 'الإختبار'
description: 'تعلم الطرق لإختبار مكونات واجهة المستخدم'
---

<div style="direction: rtl">

لا يكتمل درس ستوريبوك بدون القيام بإختبارات. الإختبار أساسي لإنشاء واجهات ذات جودة عالية. في الأنظمة المركبة, التعديلات المصغرة قد تنتج نتائج كبيرة. تعرفنا إلى الآن على ثلاث أنواع من الإختبارات:

- **إختبارات يدوية** تعتمد على المطورين بأن يقوموا يدويا بالتأكد من صحة المكون. هذا يساعدنا على التأكد من نظافة مظهر المكون خلال عملية البناء
- **إختبارات اللمحة** تلتقط نص مكون الظاهر. تساعدنا على مراقبة تغيرات النص التي قد تسبب أخطاء أو إنذارات.
- **إختبارات الوحدة** مع Jest تأكد لنا أن ناتج المكون يبقى كما هو بناء عن إدخال معطى ثابت. و هي ممتازة لإختبار الصفات الوظيفية للمكون.

## “و لكن هل مظهرها صحيح؟”

للأسف, طرق الإختبار وحدها لا تكفي لمنع أخطاء واجهية. من الصعب إختبار واجهة المستخدم لأن التصميم ذاتي و دقيق. الإختبارات اليدوية, كما يشير الإسم, يدوية. إختبارات اللمحة تثير الكثير من المؤشرات المزيفة عند إستخدامها مع واجهة المستخدم. إختبارات الوحدة على مستوى البكسل تعتبر ذات قيمة متدنية. الإستراتيجية الكاملة لستوريبوك تتضمن أيضا إختبار تراجع مظهري.

## الإختبار المظهري لستوريبوك

صممت إختبارات التراجع المظهرية, و التي تسمى أيضا بالإختبارات المظهرية لإلتقاط أي تغيرات في المظهر. تعمل هذه الإختبارات عن طريق إلتقاط screenshots لكل ستوري و مقارنتهم من commit إلى commit للكشف عن التغيرات. هذا مثالي للتأكد من العناصر الرسومية مثل النسق, الألوان, الحجم و التباين.

<video autoPlay muted playsInline loop style="width:480px; margin: 0 auto;">
  <source
    src="/intro-to-storybook/visual-regression-testing.mp4"
    type="video/mp4"
  />
</video>

ستوريبوك أداة رائعة لأداء إختبار تراجع مظهري لأن كل ستوري عبارة عن مواصفات إختبار في الأساس. كلما نكتب أو نغير في ستوري, نحصل على مواصفات مجانا!

توجد العديد من الأدوات لإختبار التراجع المظهري. ننصح بإستخدام [**Chromatic**](https://www.chromatic.com/) أداة نشر مجانية صنعت من قِبل مشرفي ستوريبوك و التي يمكنك عن طريقها من إجراء إختبارات واجهية في سحابة متوازية. تسمح لك علاوة على ذلك بنشر ستوريبوك اونلاين كما رأينا في [الفصل السابق](/intro-to-storybook/react/en/deploy/).

## إلتقط التغيرات في واجهة المستخدم

يعتمد إختبار التراجع المظهري على مقانة صور من الواجهات الظاهرة حديثا مع الصور الأساسية. سيتم إعلامنا إذا تم إلتقاط أي تغيير.

لنرى طريقة عمله بتعديل خلفية مكون `Task`

إبدأ بإنشاء فرع git جديد من أجل هذا التغيير:

<div style="direction: ltr">

```bash
git checkout -b change-task-background
```

</div>

قم بتغيير `src/components/Task.js` إلى الآتي:

<div style="direction: ltr">

```diff:title=src/components/Task.js
<div className="title">
  <input
    type="text"
    value={title}
    readOnly={true}
    placeholder="Input title"
+   style={{ background: 'red' }}
  />
</div>

```

</div>

هذا سيٌنتج لون خلفية جديد للعنصر

![تغير خلفية المهمة](/intro-to-storybook/chromatic-task-change.png)

أضف الملف:

<div style="direction: ltr">

```bash
git add .
```

</div>

نفذه:

<div style="direction: ltr">

```bash
git commit -m "change task background to red"
```

</div>

و إدفع التغييرات إلى المستودع البعيد:

<div style="direction: ltr">

```bash
git push -u origin change-task-background
```

</div>

أخيرا, إفتح مستودع Github و قم بطلب سحب pull request لفرع `change-task-background`

![إنشاء طلب PR في Github للـ task](/github/pull-request-background.png)

أضف نص وصف في طلب السحب و إضغط `Create pull request`. إضغط على إختيار "🟡 UI Tests" في أسفل الصفحة.

![إنشاء طلب PR في Github للـ task](/github/pull-request-background-ok.png)

هذا سيٌظهر لك التغيرات في الواجهة التي إلتقطت من تنفيذتك

![Chromatic إلتقط التغييرات](/intro-to-storybook/chromatic-catch-changes.png)

يوجد الكثير من التغييرات! السلسلة الهرمية حيث `Task` عنصر تابع ل `TaskList` و `Inbox` يعني تغيير بسيط يكبر ليصبح تراجع عملاق. هذا الظرف بالتحديد هو السبب الذي يجعل المطورين في حاجة إلى إختبار تراجع مظهري إضافة إلى سبل إختبار أخرى.

![تغييرات واجهة بسيطة و تراجع عملاق](/intro-to-storybook/minor-major-regressions.gif)

## راجع التغييرات

يضمن إختبار التراجع الظاهري أن المكونات لا تتغير خطأً. و لكن لا يزال الأمر متروكا لنا لتحديد ما إذا كانت هذه التغييرات مقصودة أم لا.

إذا كان التغيير مقصود سنحتاج إلى تبديل الأساس بحيث أن الإختبارات المستقبلية يتم مقارنتها مع أخر نسخة من الستوري. إذا كان التغيير غير مقصود فيجب إصلاحه.

<video autoPlay muted playsInline loop style="width:480px; margin: 0 auto;">
  <source
    src="/intro-to-storybook/website-workflow-review-merge-optimized.mp4"
    type="video/mp4"
  />
</video>

بما أن التطبيقات الحديثة مبنية من مكونات فإنه من المهم اجراء إختبارات على مستوى المكون. هذا سيساعدنا على تحديد السبب الجذري للتغير, المكون عوضا عن الإستجابة لأعراض التغيير, الشاشات و المكونات المركبة.

## إدمج التغييرات

بعد مراجعتنا للتغييرات نصبح جاهزين لدمج التغييرات الواجهية بثقة -- بمعرفة أن التحديثات لن تنتج أخطاء بشكل غير مقصود. إذا أعجبتك الخلفية `red` الجديدة قم بقبول التغييرات, وإلا إرجع إلى الحالة السابقة.

![ـغييرات جاهزة للدمج](/intro-to-storybook/chromatic-review-finished.png)

ستوريبوك يساعدنا على **بناء** المكونات; الإختبار يساعدنا على **إدارتهم**. أنواع الإختبار الأربعة التي تم تغطيتها في هذا الدرس هي الإختبارات اليدوية, و اللمحية, و الوحدة, و التراجع المظهري. الأخيرة الثلاث يمكن ميكنتهم بإضافة تكامل مستمر كما فعلنا. هذا يساعد شحن مكونات دون القلق من أخطاء. مسار العمل كامل موضح في الصورة أسفله.

![مسار عمل إختبار التراجع المظهري](/intro-to-storybook/cdd-review-workflow.png)

</div>