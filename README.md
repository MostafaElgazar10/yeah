# yeahالفرق الأساسي:
- Model: مسؤول عن التعامل مع البيانات وقاعدة البيانات.
- Controller: يستقبل الطلب، وينفّذ منطق التطبيق، ويحدد هيرجع للمستخدم إيه.
مثال من مشروعنا:
- Product هو الـ Model اللي بيمثل جدول products.
- ProductController هو الـ Controller اللي بيتحكم في عرض المنتجات وإضافتها وتعديلها وحذفها.
نبدأ الدرس الثاني، وهنمشي على ملفات مشروعك الموجود بالفعل، مش هنعمل مشروع جديد.
الدرس الثاني: Laravel Project Structure
1. شكل المشروع من الخارج
افتح مشروع Laravel عندك من خلال VS Code:

C:\laragon\www\my-laravel

هتلاقي مجموعة مجلدات وملفات. كل مجلد ليه وظيفة محددة، ومش مطلوب تحفظهم كلهم مرة واحدة؛ المهم تفهم إمتى تستخدم كل واحد.
2. أهم المجلدات في Laravel
app/
مهم جدًا

ده قلب التطبيق، وفيه الكود الأساسي اللي بتكتبه.
- app/Models/ — هنا بتحط الـ Models، زي Product.php وCategory.php.
- app/Http/Controllers/ — هنا بتحط الـ Controllers، زي ProductController.php.
- app/Http/Requests/ — هنا بتحط Form Requests المسؤولة عن التحقق من البيانات، زي StoreProductRequest.php.
- app/Http/Middleware/ — هنا بتحط Middleware، ودي طبقة بتفحص الطلب قبل ما يوصل للـ Controller أو قبل ما يرجع الرد.

routes/
مهم جدًا

هنا بتحدد عناوين الصفحات والطلبات، وكل Route بتربطه بوظيفة.
- web.php — مسارات صفحات الموقع، زي عرض المنتجات وإضافة منتج.
- console.php — لتعريف أوامر Artisan المخصصة للتشغيل من سطر الأوامر.
- api.php — مسارات الـ API، لو ملفه موجود أو أنشأته في مشروعك.

resources/
مهم جدًا

هنا ملفات الواجهة اللي بتظهر للمستخدم.
- resources/views/ — ملفات Blade، زي products/index.blade.php وproducts/create.blade.php.
- resources/css/ — ملفات CSS.
- resources/js/ — ملفات JavaScript.

database/

كل ما يخص إنشاء وتجهيز قاعدة البيانات.
- migrations/ — ملفات إنشاء وتعديل الجداول.
- seeders/ — لإضافة بيانات تجريبية أو أساسية.
- factories/ — لإنشاء بيانات وهمية للاختبار.

public/

ده المجلد اللي بيكون نقطة الدخول العامة للموقع، وأهم ملف فيه index.php. وبتحط فيه الملفات العامة زي الصور والملفات المبنية للواجهة.

storage/

Laravel بيخزن فيه ملفات التطبيق، زي Logs وملفات الكاش والملفات المرفوعة في بعض الإعدادات.

config/

إعدادات التطبيق، زي إعدادات قاعدة البيانات والبريد والجلسات.

bootstrap/

ملفات تشغيل وتهيئة Laravel الأساسية. ملف app.php مهم لأنه بيجهز التطبيق ويحدد إعدادات أساسية زي تسجيل الـ Routes والـ Middleware.

vendor/

فيه المكتبات والحزم اللي Composer نزّلها. غالبًا مش بتعدل ملفات المجلد ده بنفسك.

3. أهم الملفات الموجودة في جذر المشروع
الملف	وظيفته
.env	إعدادات البيئة، مثل بيانات اتصال قاعدة البيانات
.env.example	نموذج لإعدادات البيئة
artisan	أداة تنفيذ أوامر Laravel
composer.json	تعريف مكتبات PHP واعتماديات المشروع
composer.lock	تثبيت إصدارات المكتبات المحددة
package.json	تعريف حزم JavaScript وأوامر الواجهة
vite.config.js	إعداد Vite لبناء ملفات CSS وJavaScript

خلي بالك: ملف .env ممكن يحتوي على كلمة مرور قاعدة البيانات ومفاتيح سرية؛ ما ترفعهش على GitHub بشكل عام.
4. نربط الكلام بمشروع المنتجات عندنا
خلينا نراجع رحلة طلب عرض المنتجات:
1. routes/web.php

يستقبل عنوان /products ويحدد الـ Controller

2. ProductController.php

ينفذ دالة index ويطلب المنتجات

3. Product.php (Model)

يتعامل مع جدول products عن طريق Eloquent

4. products/index.blade.php

يعرض المنتجات للمستخدم


مثال الكود اللي ممكن تلاقيه في routes/web.php:
use App\Http\Controllers\ProductController;
use Illuminate\Support\Facades\Route;

Route::get('/products', [ProductController::class, 'index']);


نفهم كل جزء:
- Route: الكلاس المسؤول عن تعريف المسارات.
- get(): معناها إن المسار بيستقبل طلب HTTP GET، وغالبًا بنستخدمه لعرض صفحة أو جلب بيانات.
- '/products': عنوان المسار.
- ProductController::class: بيحدد الكلاس المسؤول عن التعامل مع الطلب.
- 'index': اسم الدالة اللي Laravel هيشغلها داخل الـ Controller.
وفي ProductController:
public function index()
{
    $products = Product::with('category')->get();

    return view('products.index', compact('products'));
}


- Product:: بنستدعي الـ Model.
- with('category') بتطلب تحميل علاقة التصنيف مع المنتجات مسبقًا، وده اسمه Eager Loading.
- get() بتنّفذ الاستعلام وترجع مجموعة من النتائج.
- view() بترجع صفحة Blade.
- compact('products') بتحول المتغير $products إلى بيانات نقدر نستخدمها داخل الصفحة.
ملحوظة مهمة: with('category') هنا مختلفة عن ->with('success', ...) اللي بنستخدمها مع redirect()؛ الأولى لتحميل العلاقات، والثانية لتخزين رسالة مؤقتة في Session.
5. تطبيق عملي: استكشف مشروعك
افتح VS Code ونفّذ الآتي بنفسك:
1. افتح routes/web.php وابحث عن Route المنتجات.
2. افتح app/Http/Controllers/ProductController.php وابحث عن دالة index().
3. افتح app/Models/Product.php وشوف علاقة category().
4. افتح resources/views/products/index.blade.php وشوف إزاي المنتجات بتتعرض.
6. اختبر فهمك للدرس الثاني
جاوب من غير ما ترجع للشرح:
اختبار Project Structure
0 / 4 مجاب

1. عايز أعمل Route جديد لعرض المنتجات، أعدل أنهي ملف؟

A. app/Models/Product.php

B. routes/web.php

C. resources/views/products/index.blade.php




2. عايز أضيف دالة destroy لحذف منتج، أحطها فين؟

A. ProductController.php

B. Product.php

C. database/migrations




3. عايز أعدل شكل جدول المنتجات HTML، أفتح أنهي ملف؟

A. .env

B. ProductController.php

C. products/index.blade.php




4. عايز أضيف عمود category_id لجدول products، أستخدم إيه؟

A. Migration

B. Middleware

C. Blade



صحح إجاباتي
بعد ما تجاوب، هنصحح مع بعض ونبدأ الدرس الثالث: Routes بالتفصيل — أنواع HTTP Methods، والفرق بين get وpost وput وpatch وdelete، والـ Named Routes والـ Route Parameters.
