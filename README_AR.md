# نظام التسوق عبر الإنترنت باستخدام الخدمات المصغّرة

منصة تجارة إلكترونية مبنية على معمارية الخدمات المصغّرة (Microservices) باستخدام .NET 7 وخدمة RabbitMQ للاتصالات غير المتزامنة.

---

## متطلبات التشغيل

1. **نظام التشغيل:** Windows 64-bit
2. **خدمة RabbitMQ:**  
   - إمّا يتم تثبيتها محليًا أو تشغيلها عبر **Docker Desktop**
3. **مشغل .NET:** يجب تثبيت [NET 7.0](https://dotnet.microsoft.com/en-us/download/dotnet/7.0)

---

## محتوى المجلدات

```
OnlineShoppingSystem
├── For Win-64              # ملفات التنفيذ للأجهزة (نسخة ويندوز 64)
├── Source Code             # كود جميع الخدمات المصغّرة
├── README.txt             # هذا الملف بنسخته النصية الأصلية
├── 📄 تقارير المشروع         # ملفات التقرير بصيغة DOCX و PDF
```

---

## خطوات التشغيل

### 1. تشغيل RabbitMQ

- **باستخدام Docker (موصى به):**
  ```bash
  docker run -d rabbitmq
  ```

- **محليًا (إذا كانت مثبتة):**
  ```cmd
  net start RabbitMQ
  ```

>  تأكد من إضافة رابط الخدمة إلى متغيرات البيئة إذا كنت تعمل محليًا.

---

### 2. تشغيل الخدمات

افتح مجلد `For Win-64` ثم **شغّل الملفات التنفيذية بالترتيب التالي** (انقر مرتين على كل ملف):

1. `UsersService.exe`
2. `ProductsService.exe`
3. `ProductsService2.exe`
4. `OrdersService.exe`
5. `PaymentsService.exe`
6. `ApiGateway.exe`

---

## 🔗 استخدام النظام

استخدم أدوات مثل **Postman** أو **Thunder Client** لإرسال الطلبات عبر **بوابة API**:

```
http://localhost:7000/gateway/
```

> يجب أن تحتوي جميع الطلبات على الهيدر:
```http
x-api-key: <your_api_key>
```

### مفاتيح الوصول حسب الدور

| الدور     | المفتاح | الصلاحيات                       |
|-----------|---------|----------------------------------|
| أدمن      | 99999   | صلاحيات كاملة                   |
| مستخدم    | 12345   | صلاحيات محدودة                 |
| زائر      | 00000   | تسجيل وعرض محدود فقط           |

---

## الخدمات والمسارات

### 1. **إدارة المستخدمين**
- **المسار:** `/users`
- **الطلبات:**
  - `GET` – عرض المستخدمين *(للمستخدم والأدمن)*
  - `POST` – تسجيل مستخدم جديد *(للزائر أو الأدمن)*
  - `DELETE` – حذف مستخدم *(للأدمن فقط)*
  - `PUT` – تعديل بيانات مستخدم *(للأدمن فقط)*

---

### 2. **إدارة المنتجات**
- **المسار:** `/products`
- **الطلبات:**
  - `GET` – عرض المنتجات *(لجميع الأدوار)*
  - `DELETE` – حذف منتج *(للأدمن فقط)*
  - `PUT` – تعديل منتج *(للأدمن فقط)*

---

### 3. **إدارة الطلبات**
- **المسار:** `/orders`
- **الطلبات:**
  - `GET` – عرض الطلبات *(للمستخدم والأدمن)*
  - `POST` – إنشاء طلب *(للمستخدم والأدمن)*
  - `DELETE` – حذف طلب *(للأدمن فقط)*
  - `PUT` – تعديل طلب *(للأدمن فقط)*

---

### 4. **إدارة المدفوعات**
- **المسار:** `/payments`
- **الطلبات:**
  - `GET` – عرض المدفوعات *(للمستخدم والأدمن)*
  - `DELETE` – حذف سجل دفع *(للأدمن فقط)*
  - `PUT` – تعديل بيانات دفع *(للأدمن فقط)*

---

##  ملاحظات مهمة

- لا يمكن تنفيذ الطلبات بدون RabbitMQ لأنه يستخدم لنظام الاتصال غير المتزامن (نشر/اشتراك).
- الطلبات بدون x-api-key صحيح ستحصل على خطأ 401 Unauthorized.
- جميع الطلبات تمر عبر بوابة API Gateway ولا حاجة للتعامل مع الخدمات مباشرة.
- عند تنفيذ طلب حجز منتج ستظهر رسالة في خدمة الدفع تشير إلى استلام الطلب (تأكيد على الاتصال غير المتزامن).
- الأدمن يمكنه عرض كل المستخدمين، أما المستخدم العادي يمكنه فقط عرض معلوماته.
- في خدمة عرض المنتجات، قد تختلف النتائج بين المنتجات الإلكترونية والعطور بسبب موازنة الأحمال بين ProductService و ProductService2.

---

> المشروع من تنفيذ:  
> 
> Developed by Reem (321116), Sarah (326852), and Elias (335295) – MWS_WDC_HW1_F24
