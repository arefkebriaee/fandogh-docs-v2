---
id: exec
title: دستورات exec
sidebar_label: دستورات exec
description: 'exec یک قابلیت عملیاتی است که با استفاده از آن می‌توان دستورات خاصی را بر روی یک پروسه در حال اجرا پیاده کرد.'
keywords:
  - "سکوی ابری"
  - داکر
  - service
  - container
  - سرویس
  - exec
  - "سرویس داکری"
  - docker
  - "سکوی ابری فندق"
  - "زیرساخت ابری"
image: /img/docs/thumbnails/services/service-exec-thumbnail.png
---

## exec چیست ؟

![EXEC](/img/docs/exec.svg "EXEC")

exec یک قابلیت عملیاتی است که با استفاده از آن می‌توان دستورات خاصی را بر روی یک پروسه در حال اجرا پیاده کرد. <br/>
بگذارید با یک مثال خیلی ساده و ابتدایی کاربرد آن را برایتان شفاف‌تر کنیم.<br/>
همانظور که می‌دانیم سرویس‌ها از ایمیج‌ها ساخته می‌شوند و هر سرویس مجموعه‌ای از کدها و منطق‌هایی است که برای آن طراحی شده است؛ تا هنگامی که سرویس ساخته شد، آن ها را انجام دهد.<br/><br/>
هر سرویس یک کانتینر داکری است که داخل خود یک سری فایل دارد و یکی از آن‌ فایل‌ها که مطمئن هستیم در دایرکتوری سرویس ما حضور دارد Dockerfile است.<br/><br/>
فرض کنید شما بنا به دلایلی Dockerfile را از حافظه محلی رایانه خود به اشتباه پاک کرده اید و نیاز دارید تا Dockerfile مربوط به سرویس خود را مشاهده کنید.<br/><br/>
خب همانطور که می‌دانیم دسترسی به کانتینرها در حالت عادی غیرممکن است، پس باید چه کار کرد؟<br/><br/>
در این مرحله exec می‌تواند یک راه حل بسیار کاربردی باشد. به مثال زیر توجه کنید:
فرض می‌کنیم نام سرویس شما SVC بوده و از طرفی هم می‌دانیم دستور مشاهده محتوای یک فایل **cat** است. حال با استفاده از نام سرویس و دستور ذکر شده می‌توان دستور exec را اجرا کرد.<br/><br/>

```bash
fandogh exec --service SVC "cat Dockerfile"
```

بعد از اجرای این دستور شما می‌توانید محتوای Dockerfile خود را در fandogh-cli مشاهده کنید.<br/>
توجه داشته باشید پارامتر `service--` نمایانگر نام سرویسی است که می‌خواهید دستور exec را بر روی آن اجرا کنید.<br/>
بعد از آنکه نام سرویس را نوشتید، باید دستور exec را بین دو **)" "(** قرار دهید تا exec به درستی اجرا شود و اگر علامت `"‍` را ابتدا و انتهای دستور exec قرار ندهید، fandogh-cli به شما خطا نشان خواهد داد.

### Interactive exec
مشکل اصلی که در اجرای دستورهای exec ممکن است آزار دهنده باشد این است که گاها شما نیاز خواهید داشت چند دستور پشت هم را بر روی سرویس‌های خود اجرا کنید و اجرای `fandogh exec` برای هر دستور باعث ناراحتی می‌شود.<br/><br/>
حال برای اینکه بتوانید داخل محیط سرویس بمانید تنها کافی است در انتهای دستور `fandogh exec` پارامتر `i-` را اضافه کنید؛ این پارامتر مخفف واژه‌ی **interactive** بوده و بدین معنی است که، مادامی که داخل سرویس هستید می‌توانید دستورات خود را وارد نمایید.<br/><br/>
برای مثال اگر بخواهیم دستوری که در بخش بالا بیان کردیم را به صورت interactive بنویسیم، بدین شکل عمل خواهیم کرد:<br/>

```bash
fandogh exec --service SVC "bash" -i
```
بعد از اجرای این دستور، فندق یک session جدید برای شما ساخته و این امکان را در اختیار شما قرار می‌دهد تا وارد سرویس شده و دستورات مورد نیاز را در آن وارد نمایید.<br/>
در آخر هم می‌توانید با نوشتن واژه **exit** یا زدن دکمه‌های **ctrl + c** از محیط سرویس خارج شوید.