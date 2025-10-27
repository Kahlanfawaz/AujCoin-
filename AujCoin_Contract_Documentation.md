# توثيق العقد الذكي لعملة AujCoin

**المؤلف:** Manus AI
**التاريخ:** 28 أكتوبر 2025
**المستودع:** Kahlanfawaz/AujCoin-

---

## 1. نظرة عامة على العقد

يمثل هذا المستند توثيقًا شاملاً للعقد الذكي **AujCoin**، وهو رمز مميز (Token) يعمل وفق معيار **BEP-20** على شبكة **BNB Smart Chain (BSC)**. تم تحديث العقد ليتوافق مع النسخة المستخدمة للتحقق على BscScan، والتي توفر وظائف BEP-20 قياسية وميزات إدارة الملكية (Ownable) ووظائف مساعدة لإدارة الصلاحيات (Allowance).

| الخاصية | القيمة |
| :--- | :--- |
| اسم الرمز (`name`) | **AujCoin** |
| رمز الرمز (`symbol`) | **auj** |
| عدد المنازل العشرية (`decimals`) | **18** |
| المعيار | **BEP-20** |
| شبكة البلوكشين | **BNB Smart Chain (BSC)** |
| إصدار المترجم (Pragma) | **`^0.8.20`** (تم التحديث للتوافق مع التحقق) |
| الترخيص (`SPDX`) | `MIT` |

---

## 2. تفاصيل العقد (AujCoin.sol)

العقد الذكي `AujCoin` هو عقد BEP-20 يطبق الوظائف الرئيسية لرمز قابل للتداول، بالإضافة إلى وظائف إدارة الملكية وصلاحيات الإنفاق الموسعة.

### 2.1. المتغيرات والتخزين (State Variables)

| المتغير | النوع | الوصف | الوصول |
| :--- | :--- | :--- | :--- |
| `name` | `string` | اسم الرمز ("AujCoin"). | `public` |
| `symbol` | `string` | رمز الرمز ("auj"). | `public` |
| `decimals` | `uint8` | عدد المنازل العشرية (18). | `public` |
| `totalSupply` | `uint256` | إجمالي المعروض من الرمز. | `public` |
| `balanceOf` | `mapping` | خريطة لتخزين رصيد كل عنوان. | `public` |
| `allowance` | `mapping` | خريطة لتخزين المبالغ المسموح بإنفاقها من قبل عناوين أخرى. | `public` |
| `owner` | `address` | عنوان المالك الذي لديه صلاحيات إدارية. | `public` |

### 2.2. المُنشئ (Constructor)

```solidity
constructor(uint256 _initialSupply)
```
*   **الوصف:** يتم تنفيذ هذه الدالة مرة واحدة عند نشر العقد.
*   **الوظيفة:**
    1.  تعيين المالك (`owner`) ليكون هو العنوان الذي قام بالنشر (`msg.sender`).
    2.  حساب إجمالي العرض الأولي (`totalSupply`) بضرب `_initialSupply` في `10^18`.
    3.  تخصيص كامل العرض الأولي لرصيد المالك.
    4.  إصدار حدث `Transfer` للإشارة إلى إنشاء الرموز.

### 2.3. الدوال الأساسية (BEP-20 Standard Functions)

| الدالة | الوصف |
| :--- | :--- |
| `balanceOf(address account)` | إرجاع رصيد الرمز لعنوان معين. **(تم تغيير اسم الدالة في التوثيق إلى `balanceOfAccount` لتعكس التسمية في الكود المحدث، ولكنها تؤدي نفس الوظيفة)** |
| `transfer(address to, uint256 value)` | تحويل الرموز من رصيد المرسل (`msg.sender`) إلى عنوان المستلم. |
| `approve(address spender, uint256 value)` | منح عنوان آخر (`spender`) صلاحية إنفاق مبلغ محدد من رموز المالك. |
| `allowance(address ownerAddress, address spenderAddress)` | إرجاع المبلغ المتبقي الذي يمكن لـ `spenderAddress` إنفاقه من رصيد `ownerAddress`. **(تم تغيير اسم الدالة في التوثيق إلى `allowanceOf` لتعكس التسمية في الكود المحدث، ولكنها تؤدي نفس الوظيفة)** |
| `transferFrom(address from, address to, uint256 value)` | تحويل الرموز من رصيد `from` إلى `to` نيابة عن `msg.sender` (المنفق)، مع خصم المبلغ من الصلاحية الممنوحة. |

### 2.4. وظائف إدارة الملكية والصلاحيات (Ownership and Allowance Management)

هذه الدوال مقيدة بالمالك فقط أو توفر وظائف مساعدة لإدارة الصلاحيات.

| الدالة | المُعدِّل | الوصف |
| :--- | :--- | :--- |
| `transferOwnership(address newOwner)` | `onlyOwner` | تحويل ملكية العقد إلى عنوان جديد. |
| `increaseAllowance(address spender, uint256 addedValue)` | `public` | زيادة المبلغ المسموح لـ `spender` بإنفاقه. |
| `decreaseAllowance(address spender, uint256 subtractedValue)` | `public` | تقليل المبلغ المسموح لـ `spender` بإنفاقه. |

---

## 3. إعدادات التحقق على BscScan (النسخة الموحدة)

تم توحيد الكود المصدري في `AujCoin.sol` ليتطابق تمامًا مع متطلبات التحقق على BscScan. لضمان نجاح التحقق، يجب استخدام الإعدادات التالية:

### 3.1. إعدادات المترجم (Compiler Settings)

| الإعداد | القيمة |
| :--- | :--- |
| إصدار المترجم (`pragma`) | **`0.8.20`** |
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

تم تحديث المستودع لتوحيد الكود المصدري وتحديث التوثيق.

| الملف | التغيير |
| :--- | :--- |
| `AujCoin.sol` | تم تحديثه ليتطابق مع نسخة التحقق (إصدار 0.8.20 وإضافة وظائف الملكية والصلاحيات الموسعة). |
| `AujCoin_Contract_Documentation.md` | تم تحديثه ليعكس التغييرات في العقد الموحد. |

---

## 5. المراجع

*   [AujCoin.sol](AujCoin-/AujCoin.sol) - الكود المصدري الموحد للعقد الذكي.
*   [AujCoin_BscScan_Verification.json](AujCoin-/AujCoin_BscScan_Verification.json) - إعدادات التحقق المستخدمة على BscScan.
*   [AujCoin_Project_Documentation.md](AujCoin-/AujCoin_Project_Documentation.md) - التوثيق العام للمشروع.
*   [BEP-20 Standard](https://docs.bnbchain.org/docs/chain-specs/BEP20) - معيار الرموز على BNB Smart Chain.
