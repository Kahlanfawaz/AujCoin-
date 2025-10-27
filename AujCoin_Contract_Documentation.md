# توثيق العقد الذكي لعملة AujCoin

**المؤلف:** Manus AI
**التاريخ:** 28 أكتوبر 2025
**المستودع:** Kahlanfawaz/AujCoin-

---

## 1. نظرة عامة على العقد

يمثل هذا المستند توثيقًا شاملاً للعقد الذكي **AujCoin**، وهو رمز مميز (Token) يعمل وفق معيار **BEP-20** على شبكة **BNB Smart Chain (BSC)**. يهدف العقد إلى توفير وظائف أساسية لرمز مميز قابل للتداول مع آليات تحكم في العرض (Minting و Burning) مقيدة بالمالك.

| الخاصية | القيمة |
| :--- | :--- |
| اسم الرمز (`name`) | **AujCoin** |
| رمز الرمز (`symbol`) | **auj** |
| عدد المنازل العشرية (`decimals`) | **18** |
| المعيار | **BEP-20** |
| شبكة البلوكشين | **BNB Smart Chain (BSC)** |
| إصدار المترجم (Pragma) | `^0.8.14` |
| الترخيص (`SPDX`) | `MIT` |

---

## 2. تفاصيل العقد (AujCoin.sol)

العقد الذكي `AujCoin` هو عقد أساسي يطبق الوظائف الرئيسية لرمز BEP-20، بالإضافة إلى وظائف تحكم إضافية.

### 2.1. المتغيرات والتخزين (State Variables)

| المتغير | النوع | الوصف | الوصول |
| :--- | :--- | :--- | :--- |
| `name` | `string` | اسم الرمز ("AujCoin"). | `public` |
| `symbol` | `string` | رمز الرمز ("auj"). | `public` |
| `decimals` | `uint8` | عدد المنازل العشرية (18). | `public` |
| `totalSupply` | `uint256` | إجمالي المعروض من الرمز. | `public` |
| `balances` | `mapping` | خريطة لتخزين رصيد كل عنوان. | `private` |
| `allowances` | `mapping` | خريطة لتخزين المبالغ المسموح بإنفاقها من قبل عناوين أخرى. | `private` |
| `owner` | `address` | عنوان المالك الذي لديه صلاحيات إدارية. | `public` |

### 2.2. المُنشئ (Constructor)

```solidity
constructor(uint256 initialSupply)
```
*   **الوصف:** يتم تنفيذ هذه الدالة مرة واحدة عند نشر العقد.
*   **الوظيفة:**
    1.  تعيين المالك (`owner`) ليكون هو العنوان الذي قام بالنشر (`msg.sender`).
    2.  حساب إجمالي العرض الأولي (`totalSupply`) بضرب `initialSupply` في `10^18`.
    3.  تخصيص كامل العرض الأولي لرصيد المالك.
    4.  إصدار حدث `Transfer` للإشارة إلى إنشاء الرموز.

### 2.3. الدوال الأساسية (BEP-20 Standard Functions)

| الدالة | الوصف |
| :--- | :--- |
| `balanceOf(address account)` | إرجاع رصيد الرمز لعنوان معين. |
| `transfer(address recipient, uint256 amount)` | تحويل الرموز من رصيد المرسل (`msg.sender`) إلى عنوان المستلم. |
| `approve(address spender, uint256 amount)` | منح عنوان آخر (`spender`) صلاحية إنفاق مبلغ محدد من رموز المالك. |
| `allowance(address owner_, address spender)` | إرجاع المبلغ المتبقي الذي يمكن لـ `spender` إنفاقه من رصيد `owner_`. |
| `transferFrom(address sender, address recipient, uint256 amount)` | تحويل الرموز من رصيد `sender` إلى `recipient` نيابة عن `msg.sender` (المنفق)، مع خصم المبلغ من الصلاحية الممنوحة. |

### 2.4. وظائف المالك الإضافية (Owner Functions)

هذه الدوال مقيدة بالمالك فقط باستخدام المُعدِّل (`modifier`) `onlyOwner`.

| الدالة | الوصف |
| :--- | :--- |
| `mint(uint256 amount)` | **سك رموز جديدة:** زيادة إجمالي العرض (`totalSupply`) وزيادة رصيد المالك بالمبلغ المحدد. |
| `burn(uint256 amount)` | **حرق رموز:** تقليل إجمالي العرض (`totalSupply`) وتقليل رصيد المالك بالمبلغ المحدد. |

---

## 3. إعدادات التحقق على BscScan

لضمان الشفافية والقدرة على التفاعل مع العقد مباشرة عبر مستكشف الكتل (BscScan)، يجب استخدام الإعدادات التالية للتحقق من العقد الذي تم نشره.

**ملاحظة هامة:** الكود المصدري المرفق في ملف `AujCoin.sol` (الذي يحتوي على دالتي `mint` و `burn`) **يختلف** عن الكود المصدري الموجود في ملف `AujCoin_BscScan_Verification.json` و `AujCoin_Contract.docx`. الكود في ملفات التحقق يتبع معيار BEP-20 أكثر تطوراً ويحتوي على دوال مثل `increaseAllowance` و `decreaseAllowance` و `transferOwnership`، ولا يحتوي على دالتي `mint` و `burn`، ولكنه يستخدم متغيرات تخزين عامة (مثل `balanceOf` و `allowance`) بدلاً من المتغيرات الخاصة في `AujCoin.sol`.

**يجب نشر العقد الذي يتطابق مع إعدادات التحقق (النسخة الأكثر تطوراً) لضمان نجاح التحقق على BscScan.**

### 3.1. إعدادات المترجم (Compiler Settings)

| الإعداد | القيمة |
| :--- | :--- |
| إصدار المترجم (`pragma`) | **`^0.8.20`** (في كود التحقق) |
| إصدار EVM | `london` |
| التحسين (`optimizer`) | **مفعل (`enabled: true`)** |
| عدد مرات التشغيل (`runs`) | **200** |

### 3.2. وسائط المُنشئ (Constructor Arguments)

يتطلب المُنشئ قيمة واحدة: `uint256 _initialSupply`.

```solidity
constructor(uint256 _initialSupply)
```

عند التحقق على BscScan، يجب توفير القيمة المشفرة لـ `_initialSupply` التي تم استخدامها أثناء النشر، كجزء من عملية التحقق.

---

## 4. تحديثات المستودع

تم تحديث المستودع بإضافة هذا الملف التوثيقي لضمان توفر معلومات واضحة حول العقد الذكي.

| الملف | الوصف |
| :--- | :--- |
| `AujCoin_Contract_Documentation.md` | هذا الملف التوثيقي الجديد للعقد الذكي. |

---

## 5. المراجع

*   [AujCoin.sol](AujCoin-/AujCoin.sol) - الكود المصدري للعقد الذكي (النسخة الأساسية).
*   [AujCoin_BscScan_Verification.json](AujCoin-/AujCoin_BscScan_Verification.json) - إعدادات التحقق والكود المصدري (النسخة المتقدمة) المستخدمة للتحقق على BscScan.
*   [AujCoin_Project_Documentation.md](AujCoin-/AujCoin_Project_Documentation.md) - التوثيق العام للمشروع.
*   [BEP-20 Standard](https://docs.bnbchain.org/docs/chain-specs/BEP20) - معيار الرموز على BNB Smart Chain.
