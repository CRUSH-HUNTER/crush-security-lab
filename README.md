# 🔐 Security Research Notes

مستندات و گزارش‌های شخصی من از یادگیری وب سکیوریتی و باگ‌بانتی.

این ریپو بیشتر جنبه‌ی یادگیری دارد تا گزارش رسمی و بیشتر برای مستندسازی روند کار هست.

⚠️ **Disclaimer**

> تمام تست‌ها و تحلیل‌های این مستندات روی محیط آزمایشگاهی محلی (Kali Linux, VMware) و اپلیکیشن عمداً آسیب‌پذیر [OWASP Juice Shop](https://github.com/juice-shop/juice-shop) انجام شده‌اند.
هیچ سیستم واقعی یا متعلق به شخص ثالث تست نشده است. 
این محتوا صرفاً برای یادگیری و مستندسازی شخصی تهیه شده است.

---

## 📑 فهرست گزارش‌ها

| گزارش | توضیح |
|---|---|
| [🛡️ یافته‌های تست امنیتی — Juice Shop](https://maryla200.github.io/juice-shop-security-research/security-test-findings.html) | مستندسازی کامل ۸ تست دستی روی Juice Shop: CSRF، Information Disclosure، IDOR، Clickjacking، Directory Traversal و بای‌پس کامل احراز هویت با JWT Algorithm None |
| [⚡ SQL Injection](https://maryla200.github.io/juice-shop-security-research/sql-injection.html) | دور زدن لاگین ادمین و استخراج/کرک هش پسورد از طریق SQL Injection |
| [🔑 JWT Deep Dive](https://maryla200.github.io/juice-shop-security-research/jwt_deep_dive.html) | بررسی عمیق ساختار JWT، رمزگشایی Payload و تست عملی روی توکن‌ها |
| [📟 TOTP Secret](https://maryla200.github.io/juice-shop-security-research/totp-secret-explained.html) | مفهوم و کاربرد TOTP Secret در احراز هویت دومرحله‌ای |
| [🔓 Password Hash Cracking](https://maryla200.github.io/juice-shop-security-research/password-hash-cracking.html) | مفاهیم هش پسورد و روش‌های کرک آن با hashcat |
| [🍪 XSS Session Hijacking](https://maryla200.github.io/juice-shop-security-research/blob/main/xss_session_hijacking.html) | سناریوی عملی دزدیدن نشست کاربر از طریق تزریق XSS و سرقت کوکی‌های جلسه |
| [📋 Full penetration test report](https://maryla200.github.io/juice-shop-security-research/blob/main/full-penetration-test-report.html) | گزارش کامل تست نفوذ با هدف دسترسی غیرمجاز به حساب کاربر Bender با استفاده از SQL Injection - Authentication Bypass (✅ موفق) |
| [⚙️ Hashcat Errors Report](https://maryla200.github.io/juice-shop-security-research/hashcat-errors-report.html) | خطاها و راه‌حل‌های عملی هنگام کار با ابزار hashcat |
| [🌐 HTTP & URL Reference](https://maryla200.github.io/juice-shop-security-research/http%26url.html) | مرجع مفاهیم HTTP و ساختار URL |
| [📦 Juice Shop Findings (Public)](https://maryla200.github.io/juice-shop-security-research/juice_shop_findings_public.html) | نسخه‌ی خلاصه‌شده‌ی یافته‌های تست روی Juice Shop |
| [🧩 APT Lock Challenge](https://maryla200.github.io/juice-shop-security-research/apt-lock-challenge.html) | عیب‌یابی و حل چالش قفل شدن APT در محیط Kali |
| [🖥️ Kali Disk Expansion Guide](https://maryla200.github.io/juice-shop-security-research/kali_disk_expansion_guide.html) | راهنمای افزایش فضای دیسک ماشین مجازی کالی لینوکس |
| [💽 VMware Disk Lock Troubleshooting](https://maryla200.github.io/juice-shop-security-research/vmware-disk-lock-troubleshooting.html) | عیب‌یابی و رفع خطای قفل شدن دیسک در VMware |

---

## 🎯 هدف

یادداشت‌برداری شخصی در مسیر یادگیری pentesting و باگ‌بانتی — شامل تحلیل آسیب‌پذیری‌های وب (OWASP Top 10)، عیب‌یابی محیط آزمایشگاهی (Kali / VMware)، و تمرین عملی روی پلتفرم‌های آموزشی مثل OWASP Juice Shop می باشد.

## 🧰 ابزارهای استفاده‌شده

`Burp Suite` · `DevTools` · `hashcat` · `sqlmap concepts` · `jwt.io` · `crackstation` · `Kali Linux` · `VMware Workstation`
