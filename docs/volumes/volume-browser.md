---
id: volume-browser
title: Volume Browser
sidebar_label: Volume Browser 
description: 'یکی از نیازمندی‌های کاربران در استفاده از فضاهای ذخیره سازی اختصاصی یا همان Dedicated Volumes دسترسی راحت به این فضاها برای مدیریت و آپلود و یا دانلود فایل‌ها است...'
keywords:
  - "سکوی ابری"
  - داکر
  - volume
  - volumes
  - "dedicated volume"
  - فضا
  - "فضای ذخیره سازی"
  - "ذخیره سازی"
  - storage
  - file
  - upload
  - manager
  - docker
  - "سکوی ابری فندق"
  - "زیرساخت ابری"
image: /img/docs/thumbnails/volumes/volume-browser-thumbnail.png
---

یکی از نیازمندی‌های کاربران در استفاده از فضاهای ذخیره سازی اختصاصی یا همان **Dedicated Volumes** دسترسی راحت به این فضاها برای مدیریت و آپلود و یا دانلود فایل‌ها است؛<br/>
یکی از روش‌های مدیریت فضاهای ذخیره‌سازی ایجاد [سرویس مدیریت فایل] [file_manager] است که می‌توانید این سرویس را به فضای ذخیره‌سازی اختصاصی متصل کرده و داده‌های موجود بر روی فضا را مدیریت کنید.<br/><br/>
مشکل اصلی زمانی است که شما فضای ذخیره‌سازی که در حال حاضر به سرویس دیگری متصل شده است را مدیریت کنید!<br/><br/>
همانطور که در بخش [فضای ذخیره‌سازی اختصاصی] [dedicated_volumes] توضیح داده شده است، هر فضای ذخیره‌سازی اختصاصی تنها می‌تواند به یک سرویس متصل شود و در صورت تلاش برای اتصال دو سرویس به یک فضا با خطا سرور مواجه خواهید شد!

![Volume Attachment Limit](/img/docs/volume-attachment-limit.svg "Volume Attachment Limit")

حال راه حل چیست؟<br/>
برای این مشکل سکوی ابری فندق قابلیت جدیدی را با نام **Volume Browser**  به سرویس‌های خود اضافه کرده است.

## نحوه کارکرد Volume Browser
با این قابلیت هر سرویس که به **هر تعداد Dedicated Volume** متصل شده باشد در کنار خود یک سرویس مدیریت فایل هم خواهد داشت که با استفاده از آن می‌توانید تمامی مسیر‌های موجود در آن فضاها را مشاهده کرده و مدیریت کنید.<br/>
برای درک بهتر به شکل زیر دقت نمایید.

![Volume Browser Containers](/img/docs/volume-browser-containers.svg "Volume Browser Containers ")

همانطور که در شکل بالا مشاهده می کنید هر سرویس یک کانتینر اصلی خواهد داشت که توسط ایمیجی که آدرس آن را داده اید ساخته و مستقر می شود. در کنار کانتینر اصلی یک کانتینر دیگر در همان سرویس ساخته می شود که به Dedicated Volume متصل است و از آن برای مشاهده و مدیریت محتواهای درون فضای ذخیره‌ سازی استفاده می شود.

## نحوه فعالسازی Volume Browser
برای آنکه بتوانید قابلیت **File Browser** را برای سرویس خود فعال کنید، کافی است مقدار `volume_browser_enabled` را داخل [مانیفست سرویس] [service_manifest] فعال کنید.

:::note توجه
توجه داشته باشید برای آنکه بتوانید از این قابلیت استفاده کنید، سرویس باید حداقل به یک Dedicated Volume متصل شده باشد، در غیر این صورت با خطای سرور مواجه خواهید شد.
:::

به صورت پیشفرض مقدار `volume_browser_enabled` در تمام سرویس‌ها **false** است و در صورتی که این مقدار را برابر با **true** قرار دهید، این کانتینر برای شما فعال شده و از طریق مرورگر با لینکی به ساختار زیر در دسترس خواهد بود:

```http
SERVICE_NAME-volume-browser-NAMESPACE_NAME.fandogh.cloud
```

در آدرس فوق، منظور از **SERVICE_NAME** همان نامی است که در ابتدا برای سرویس خود انتخاب می‌کنید؛ فندق از همین اسم برای ساخت لینک کانتینر File Browser استفاده می‌کند.

:::tip راهنمایی
مقدار username و password برای ورود به پنل مدیریت فایل یا همان File Browser مقادیر پیشفرش admin/admin هستند و بعد از ورود میتوانید از بخش settings سرویس password را تغییر دهید.
:::

## مانیفست آزمایش Volume Browser
برای آنکه با این ویژگی بهتر آشنا شوید می‌توانید از مانیفست زیر استفاده کنید.<br/>
این مانیفست یک سرویس سبک از ایمیج nginx نسخه alpine ایجاد کرده و بعد از استقرار آن را به یک Dedicated Volume متصل می‌کند که باید از قبل آن را داخل فضانام خود ساخته و نام آن را جای ENTER_VOLUME_NAME قرار داده باشید.

```yaml title="svc_deployment.yml"
kind: ExternalService
name: nginx
spec:
  image: _/nginx:alpine
  image_pull_policy: IfNotPresent
  volume_browser_enabled: true
  volume_mounts:
  - mount_path: /test_dir
    volume_name: ENTER_VOLUME_NAME
  resources:
      memory: 200Mi
```

بعد از اینکه سرویس به درستی مستقر شد یک لینک برای شما نمایش داده خواهد شد که از طریق آن می‌توانید با استفاده از مرورگر به کانتینر **File Browser** دسترسی داشته باشید.

```http
nginx-volume-browser-NAMESPACE_NAME.fandogh.cloud
```

در لینک بالا همانطور که مشاهده می‌کنید جای **SERVICE_NAME** که پیشتر توضیح داده شد، نام **nginx** قرار داده شده است و جای **NAMESPACE_NAME** هم نام فضانام شما قرار خواهد گرفت.

[file_manager]: /docs/managed-services/file-browser-managed-service
[service_manifest]: /docs/services/service-manifest
[dedicated_volumes]:/docs/volumes/dedicated-volume