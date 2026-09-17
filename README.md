# Sphinx Pack — موقع بيع مستلزمات التغليف

## 1) إعداد Firebase (مهم قبل أي حاجة)
1. روح على https://console.firebase.google.com واعمل مشروع جديد.
2. من القائمة الجانبية: **Build > Firestore Database > Create database** — اختار **Start in test mode** (تقدر تظبط الصلاحيات بعدين).
3. من **Project settings ⚙️ > Your apps** اضغط أيقونة **</>** (Web) وسجّل تطبيق ويب جديد.
4. هيديك كائن `firebaseConfig` — انسخه وحطه في ملف `js/firebase-config.js` بدل القيم اللي مكتوب فيها PASTE_YOUR...
5. لو عايز الأمان أعلى، بعد التجربة غيّر Firestore rules لحاجة زي دي (تسمح بقراءة المنتجات للكل، وكتابة الأوردرات للكل، ومنتجات وتحديث الأوردرات محتاج تتأكد منه إنت بنفسك لأن مفيش تسجيل دخول حقيقي بالسيرفر):
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /products/{doc} { allow read: if true; allow write: if true; }
       match /orders/{doc} { allow read, write: if true; }
     }
   }
   ```
   **ملحوظة مهمة**: الباسورد بتاع صفحة الأدمن (`abbasino10`) شغال من جوه المتصفح بس — مش حماية حقيقية على مستوى السيرفر. أي حد يعرف رابط `/admin` ويفتح كود الصفحة يقدر يتخطاه. ده كويس كبداية، لكن لو الموقع هيبقى فيه بيانات مهمة، الأفضل بعدين نستخدم **Firebase Authentication** بدل الباسورد الثابت — قولّي لو عايز أضيفها.

## 2) تجربة الموقع محلياً
افتح `index.html` بأي متصفح، أو استخدم إضافة زي Live Server في VS Code.

## 3) إضافة المنتجات
- افتح `admin/index.html`، سجل دخول بالباسورد `abbasino10`.
- من تاب "المنتجات" ضيف كل منتج (الاسم، القسم، الكمية، السعر، رابط صورة اختياري، وممكن تمسح باركود المنتج بالكاميرا).
- المنتج هيظهر فوراً في صفحة قسمه على الموقع.

## 4) استقبال الأوردرات
- لما عميل يعمل طلب من السلة ويدوس "إرسال الطلب عبر واتساب"، الطلب بيتسجل في Firebase وبيتفتحله واتساب برسالة جاهزة لرقم **01028735709**.
- سيبي صفحة `admin/index.html` مفتوحة في تابة أو جهاز مخصص — هتسمع صوت تنبيه تلقائي لحظة ما أي أوردر جديد يوصل، وهيظهر فوق في قائمة "الأوردرات الجديدة".
- اضغط "تأكيد استلام الأوردر" بعد ما تتواصل مع العميل وتأكد الطلب — هينقل لقائمة "الأوردرات المؤكدة".

## 5) الرفع على GitHub Pages
1. اعمل مستودع (repository) جديد على GitHub وارفع كل ملفات المجلد ده فيه.
2. من إعدادات المستودع **Settings > Pages**، اختار **Deploy from a branch**، وبرانش `main`، والمجلد `/root`.
3. بعد دقيقة أو اتنين هيديك رابط زي:
   `https://username.github.io/repo-name/`
4. صفحة الأدمن هتبقى على: `https://username.github.io/repo-name/admin/`

## 6) تعديلات سهلة
- تغيير أسماء ووصف الأقسام: `js/categories.js`
- تغيير رقم الواتساب: `js/firebase-config.js` (متغير `SHOP_WHATSAPP_NUMBER`)
- تغيير الألوان: أول الملف `css/style.css` (`:root`) و`css/admin.css`
- تغيير اللوجو: استبدل `assets/logo.png` بنفس الاسم

## هيكل الملفات
```
sphinxpack/
├── index.html          الصفحة الرئيسية
├── category.html        قالب صفحة القسم (?cat=plastic-containers ... إلخ)
├── admin/index.html     لوحة التحكم
├── css/style.css        تنسيق الموقع العام
├── css/admin.css        تنسيق لوحة التحكم
├── js/firebase-config.js  إعدادات Firebase + رقم الواتساب
├── js/categories.js     قائمة الأقسام
├── js/cart.js           منطق السلة + إرسال الطلب
├── js/admin.js          منطق لوحة التحكم بالكامل
└── assets/logo.png      اللوجو
```
