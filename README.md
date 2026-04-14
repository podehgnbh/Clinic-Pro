# 🏥 Clinic Pro

نظام إدارة العيادات المتكامل - منصة شاملة لإدارة الأطباء والمرضى والتحاليل والأشعات

**المطور**: عبد الله مصطفى © 2024

---

## 📋 نظرة عامة

**Clinic Pro** هو نظام متطور لإدارة العيادات والمستشفيات يوفر:

- ✅ **لوحة تحكم المدير**: إضافة وإدارة حسابات الأطباء
- 👨‍⚕️ **بوابة الطبيب**: نظام شامل لإدارة ملفات المرضى
- 📋 **إدارة المرضى**: تسجيل وتتبع بيانات المرضى الكاملة
- 🔬 **طلبات التحاليل**: إدارة طلبات التحاليل الطبية
- 🩻 **طلبات الأشعات**: تنظيم طلبات الأشعات والفحوصات
- 📊 **الإحصائيات**: تقارير شاملة عن الأطباء والمرضى

---

## 🗂️ الملفات

| الملف | الوظيفة |
|-------|---------|
| `index.html` | لوحة تحكم المدير - إضافة وإدارة الأطباء |
| `dr-login.html` | صفحة دخول الطبيب - تسجيل الدخول الآمن |
| `doctor.html` | نظام العيادة الرئيسي - إدارة المرضى والطلبات |
| `1000068207.png` | شعار البرنامج (اختياري) |
| `README.md` | توثيق المشروع |

---

## 🚀 البدء السريع

### المتطلبات
- متصفح حديث (Chrome, Firefox, Safari, Edge)
- اتصال بالإنترنت
- حساب Supabase (قاعدة البيانات)

### خطوات الإعداد

#### 1. إعداد قاعدة البيانات (Supabase)

1. انتقل إلى [Supabase](https://supabase.com)
2. أنشئ مشروعاً جديداً
3. انسخ **Project URL** و **Anon Key**
4. اذهب إلى **SQL Editor** وقم بتشغيل الكود التالي:

```sql
-- جدول الأطباء
create table doctors (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  name text not null,
  specialty text,
  username text unique not null,
  password text not null,
  slug text unique not null,
  active boolean default true
);

-- جدول المرضى
create table patients (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  doctor_id uuid references doctors(id) on delete cascade,
  name text not null,
  age integer,
  chronic_diseases text,
  diagnosis text,
  last_visit date,
  next_visit date,
  notes text
);

-- جدول طلبات التحاليل
create table lab_requests (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  doctor_id uuid references doctors(id) on delete cascade,
  patient_name text not null,
  patient_age integer,
  test_type text,
  lab_name text,
  status text default 'pending'
);

-- جدول طلبات الأشعة
create table radiology_requests (
  id uuid default gen_random_uuid() primary key,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null,
  doctor_id uuid references doctors(id) on delete cascade,
  patient_name text not null,
  patient_age integer,
  scan_type text,
  center_name text,
  status text default 'pending'
);
```

#### 2. نشر المشروع

يمكنك نشر المشروع على:
- **Vercel**: https://vercel.com
- **Netlify**: https://netlify.com
- **GitHub Pages**: https://pages.github.com
- أو أي خادم ويب آخر

#### 3. الاستخدام

1. افتح `index.html` (لوحة المدير)
2. أضف طبيباً جديداً بملء البيانات
3. انسخ الرابط المُنشأ وأرسله للطبيب
4. الطبيب يستخدم الرابط للدخول إلى نظامه

---

## 📖 دليل الاستخدام

### لوحة المدير (index.html)

**الميزات:**
- عرض إحصائيات الأطباء (إجمالي، فعال، موقف)
- إضافة طبيب جديد بسهولة
- إنشاء رابط دخول فريد لكل طبيب
- تفعيل/إيقاف اشتراك الطبيب
- حذف حساب الطبيب

**الخطوات:**
1. ملء بيانات الطبيب (الاسم، التخصص، اسم المستخدم، كلمة المرور)
2. الضغط على "إنشاء حساب ورابط"
3. نسخ الرابط وإرساله للطبيب

### بوابة الطبيب (dr-login.html)

**الميزات:**
- تسجيل دخول آمن
- عرض بيانات الطبيب
- إظهار/إخفاء كلمة المرور

**الخطوات:**
1. إدخال اسم المستخدم
2. إدخال كلمة المرور
3. الضغط على "دخول"

### نظام العيادة (doctor.html)

**الأقسام:**

#### 👥 ملفات المرضى
- عرض جميع مرضى الطبيب
- إضافة مريض جديد
- عرض تفاصيل المريض
- حذف ملف المريض
- البحث عن المرضى

**بيانات المريض:**
- الاسم والعمر
- الأمراض المزمنة
- التشخيص الحالي
- آخر موعد مراجعة
- الموعد القادم
- ملاحظات إضافية

#### 🔬 التحاليل
- عرض جميع طلبات التحاليل
- إضافة طلب تحليل جديد
- تتبع حالة الطلب (بانتظار/مكتمل)

**بيانات التحليل:**
- اسم المريض والعمر
- نوع التحليل
- اسم المختبر

#### 🩻 الأشعات
- عرض جميع طلبات الأشعات
- إضافة طلب أشعة جديد
- تتبع حالة الطلب

**بيانات الأشعة:**
- اسم المريض والعمر
- نوع الأشعة (X-Ray, CT, MRI, Echo)
- مركز الأشعة

#### 📊 التقارير
- قريباً (تحت التطوير)

---

## 🔐 الأمان

- ✅ المصادقة الآساسية (اسم المستخدم وكلمة المرور)
- ✅ تخزين البيانات في Supabase (قاعدة بيانات آمنة)
- ✅ جلسات آمنة (sessionStorage)
- ✅ فصل البيانات حسب الطبيب

**ملاحظة**: للإنتاج، يُنصح بتطبيق:
- تشفير كلمات المرور (Bcrypt)
- مصادقة متقدمة (OAuth, JWT)
- HTTPS إلزامي
- حماية CORS

---

## 🛠️ التعديلات والإصلاحات الأخيرة

### ✅ تم إصلاحه:
1. **تحديث مفاتيح Supabase** - تم استبدال المفاتيح بالمفاتيح الصحيحة
2. **إصلاح أخطاء البرمجة** - تم إصلاح الأخطاء في ملف `doctor.html`
3. **إضافة معالجة الأخطاء** - تم إضافة try-catch لجميع العمليات
4. **إضافة حقوق الملكية** - تم إضافة © عبد الله مصطفى في جميع الملفات
5. **تحسين الواجهة** - تحسينات طفيفة في التصميم والأداء

---

## 📱 التوافق

- ✅ سطح المكتب (Desktop)
- ✅ الأجهزة اللوحية (Tablet)
- ✅ الهواتف الذكية (Mobile)

---

## 🌐 الروابط

- **لوحة المدير**: `https://clinic-pro-three.vercel.app`
- **دخول الطبيب**: `https://clinic-pro-three.vercel.app/dr-login.html?slug=USERNAME`

---

## 📞 الدعم والمساعدة

إذا واجهت أي مشاكل أو لديك استفسارات، يرجى التواصل مع المطور.

---

## 📄 الترخيص

جميع الحقوق محفوظة © عبد الله مصطفى 2024

---

**آخر تحديث**: 14 أبريل 2026
