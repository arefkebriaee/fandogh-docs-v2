---
id: docker-registry-secret
title: docker registry secret
sidebar_label: Docker Registry
description: 'برای اینکه فندق بتواند به رجیستری‌های خصوصی کاربران متصل شده و Imageهای مورد نیاز را Pull کند، کاربران از پیش باید اطلاعات هویتی مورد نیاز را در اختیار فندق قرار داده باشند.'
keywords:
  - "سکوی ابری"
  - داکر
  - secret
  - env
  - "docker registry"
  - "private registry"
  - docker
  - registry
  - "image registry"
  - سکرت
  - فضانام
  - "سکوی ابری فندق"
  - "زیرساخت ابری"
image: /img/docs/thumbnails/secrets/registry-secret-thumbnail.png
---

![Docker Secret](/img/docs/secret_docker.svg "Docker Secret")

## Docker-registry
Registryهای داکر می‌توانند **public** یا **private** باشند؛ در صورتی که یک رجیستری خصوصی یا private باشد، برای pull کردن تصاویر از روی آن نیاز به داشتن نام کاربری و رمز عبور است.<br/><br/>
برای اینکه فندق بتواند به رجیستری‌های خصوصی کاربران متصل شده و Imageهای مورد نیاز را Pull کند، کاربران از پیش باید اطلاعات هویتی مورد نیاز را در اختیار فندق قرار داده باشند.<br/><br/>
Secretهایی که از نوع docker-registry هستند برای همین موضوع طراحی شده‌اند؛ این نوع از Secret ها حاوی **سه فیلد اجباری** با نام‌های<br/><br/>
- **server** )آدرس URL رجیستری(<br/>
- **username** )نام‌کاربری برای احراز هویت در رجیستری(<br/>
- **password** )رمزعبور برای احراز هویت در رجیستری(<br/>
-  و یک فیلد اختیاری به نام email  است.<br/>

در صورتی که شما نیاز داشته باشید که فندق مستقیما از رجیستری خصوصی شما تصاویر را دریافت و سرویس‌ها را ایجاد کند لازم است از قبل Secret مورد نیاز برای رجیستری خود را ساخته باشید.<br/>
فرض کنید شما از سرویس‌های [Canister] [canister_link] استفاده می‌کنید و Image های خود را به طور مداوم روی رجیستری‌ای که در Canister ساخته‌اید قرار می‌دهید.<br/>
برای اینکه فندق بتواند از رجیستری‌های خصوصی شما از روی Canister به تصاویر دسترسی داشته باشد نیاز دارید که یک Secret به شکل زیر ایجاد کنید:

```bash
fandogh  secret create  \
          --name canister \
          -t docker-registry \
          -f server=cloud.canister.io:5000 \
          -f username=JohnKane \
          -f password=J0hnKane
```

پارامتر‌هایی که برای این دستور استفاده شده است عبارتند از:

**name--**<br/>
برای مشخص کردن نام Secret، که از طریق این نام مشخص می‌کنید هنگام دریافت تصاویر از یک رجیستری از کدام Secret باید استفاده شود.

**t-** یا **type**<br/>
که نوع Secret را مشخص می‌کند، در اینجا برای نیاز بخصوصی که داریم باید `docker-registry` را قرار دهیم.

**f-**<br/>
که فیلد‌های داخل Secret را مشخص می‌کنند. هر Secret فیلد‌های خاص خودش را دارا می‌باشد که باید با توجه به مستندات مقادیر مورد نظر خود را مشخص کنید.<br/>
بعد از اجرای دستور create می‌توانید لیست Secret های خود را بررسی کنید تا مطمئن شوید Secret به درستی ساخته شده است:

```bash
fandogh secret list
```

این دستور لیست تمام secret های شما را نمایش می دهد.

[canister_link]: https://canister.io
