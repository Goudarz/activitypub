# دنبال کردن یک شخص

برگرفته از https://www.w3.org/wiki/Socialwg/Social_API/User_stories#Following_a_person

> 1. دلانو (Delano) در یک جلسهٔ شرکت با بث (Beth) آشنا می‌شود. هر دو طراح رابط کاربری هستند. دلانو ایده‌های بث را جالب می‌یابد.
> 2. دلانو بث را در شبکهٔ اجتماعی شرکتشان دنبال می‌کند.
> 3. بث عکسی از یک جلسهٔ کار روی تخته‌سفید در اردوی کاری شرکت منتشر می‌کند.
> 4. دلانو عکس را در جریان صندوق ورودی‌اش می‌بیند.
> 5. تد (Ted)، همکار دلانو، می‌خواهد افراد جدیدی برای دنبال کردن پیدا کند. او به فهرست کسانی که دلانو دنبال می‌کند نگاه می‌کند. بث را در فهرست پیدا می‌کند، جریان او را می‌خواند، از آن لذت می‌برد و تصمیم می‌گیرد او را هم دنبال کند.
> 6. بث زیاد پست می‌گذارد. دلانو در خواندن جریان صندوق ورودی‌اش مشکل دارد، چون فعالیت‌های بث فعالیت‌های بقیه را تحت‌الشعاع قرار می‌دهد. او دنبال کردن بث را متوقف می‌کند.

## ۱ و ۲: آشنایی با یک شخص جدید و دنبال کردن او

در جلسهٔ عمومی شرکت Acme Game Corp، دلانو و بث متوجه می‌شوند که
هر دو به تاریخچهٔ رابط کاربری علاقه دارند و هر دو دلبستگی خاصی به
جعبه‌ابزارهای قدیمی مانند Motif دارند.  هر روز پیش نمی‌آید که کسی را
ببینید که بتوانید با او دربارهٔ اجزای قدیمی رابط گرافیکی گپ بزنید، پس دلانو می‌پرسد که
آیا می‌توانند بیشتر صحبت کنند.  بث کارت ویزیتش را به دلانو می‌دهد و
پیشنهاد می‌کند که شاید بتوانند در پروژهٔ آینده‌ای با هم همکاری کنند.

دلانو می‌بیند که نام او «Beth Marie Bost» است و نامش را
در جست‌وجوگر افراد شرکت وارد می‌کند.  در نهایت به نشانی زیر می‌رسد که
نیم‌رخ داخلی بث است:

  https://acmegamecorp.example/people/beth_m_bost/

خودش است!  تصمیم می‌گیرد با کلاینت رومیزی‌اش مشترک او شود.
دکمهٔ «اشتراک» (subscribe) را می‌زند و نشانی نیم‌رخ بث را
در کادر متنی می‌چسباند.

کلاینت دلانو پستی به صندوق خروجی‌اش ارسال می‌کند و اعلام می‌کند که دلانو
می‌خواهد مشترک بث شود.  بث به‌عنوان گیرنده در فیلد «to» آمده است
تا سرورش بداند که باید تحویل پیام‌ها به دلانو را آغاز کند، و
[مجموعهٔ عمومی](http://w3c.github.io/activitypub/#public-addressing)
ویژه نیز به‌عنوان Cc: افزوده شده است تا دیگران از این اشتراک باخبر شوند.


```
POST /api/user/delano_sota/feed HTTP/1.1
Host: acmegamecorp.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": "http://www.w3.org/ns/activitystreams",
  "@type": "Follow",
  "actor": {
    "@type": "Person",
    "@id": "https://acmegamecorp.example/people/delano_sota/",
    "displayName": "Delano Sota",
  },
  "object": {
    "@type": "Person",
    "@id": "https://acmegamecorp.example/people/beth_m_bost/"
  },
  "to": [{
      "@type": "Person",
      "@id": "https://acmegamecorp.example/people/beth_m_bost/"
  }],
  "cc": [{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@id": "http://activityschema.org/collection/public",
    "@type": "Collection"
  }]
}
```

در پشت صحنه، سرور دلانو نشانی نیم‌رخ بث را واکشی می‌کند و
بازنمایی ActivityStreams یک Person را که با Json-LD کدگذاری شده است
در صفحه می‌بیند:

```
GET /people/beth_m_bost/
Host: acmegamecorp.example
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <script type="application/ld+json">
      {
        "@context": [
          "http://www.w3.org/ns/activitystreams",
          "http://www.w3.org/ns/activitypub"
        ],
        "@type": "Person",
        "@id": "https://acmegamecorp.example/people/beth_m_bost/",
        "following": "https://acmegamecorp.example/api/user/beth_m_bost/following",
        "followers": "https://acmegamecorp.example/api/user/beth_m_bost/followers",
        "inbox": "https://acmegamecorp.example/api/user/beth_m_bost/inbox",
        "outbox": "https://acmegamecorp.example/api/user/beth_m_bost/outbox",
        "preferredUsername": "beth_m_bost",
        "displayName": "Beth M. Bost",
        "summary": "AcmeGameCorp UI designer.  Fan of embossed widgets and 1990s video game menus.",
        "icon": [
          "https://acmegamecorp.example/media/profile_pics/beth_m_bost.png"
        ]
      }
    </script>
  </head>
  <body>
    <!-- Content goes here! -->
  </body>
</html>
```

سرور «inbox» بث را یادداشت می‌کند؛ این اطلاعات را لازم دارد تا
بتواند این فعالیت را برای بث بفرستد.  اکنون که آن را دارد،
همان فعالیتی را که دلانو به صندوق خروجی‌اش ارسال کرده بود، به صندوق ورودی بث ارسال می‌کند.

```
POST /api/user/beth_m_bost/inbox HTTP/1.1
Host: acmegamecorp.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

/* [SAME DOCUMENT AS POSTED TO ACTOR'S INBOX ENDPOINT]
   [... YES I KNOW JSON DOESN'T HAVE COMMENTS WHAT'S UP WITH THAT] */
```

(توضیح نمونه: بدنهٔ درخواست همان سندی است که پیش‌تر ارسال شد؛ و بله،
می‌دانیم که JSON توضیح (comment) ندارد!)

سرور بث تشخیص می‌دهد که دلانو در حال مشترک شدن در بث است و
این اطلاعات را ثبت می‌کند تا فعالیت‌های آینده‌ای که به‌طور کلی خطاب به
دنبال‌کنندگان بث هستند به دلانو برسند.  بث نیز اعلانی در صندوق ورودی‌اش
دریافت می‌کند که نشان می‌دهد دلانو مشترک او شده است، و می‌تواند از آن
برای دنبال کردن متقابل استفاده کند.

از آنجا که این یک پست عمومی است، سرور دلانو این
پیام را برای همهٔ دنبال‌کنندگان دلانو نیز می‌فرستد.

## ۳ و ۴: دیدن محتوای شخص جدید

بث از اردوی کاری شرکت برگشته و جاافتاده است و تصمیم می‌گیرد
عکسی از یک جلسهٔ کار روی تخته‌سفید که در آن شرکت داشت منتشر کند.  کلاینت او
عکس را به نقطهٔ پایانی فایلِ کاربرش ارسال می‌کند
(یادداشت: این‌که نقطهٔ پایانی فایل کجا تعریف می‌شود هنوز
[باید روشن شود](https://github.com/w3c-social/activitypub/issues/23))
و یک شیء پاسخ حاوی شیء دریافت می‌کند.  سپس کلاینت
ارسال دومی انجام می‌دهد که نشانی‌دهیِ کسانی را که فایل باید برایشان
فرستاده شود در بر دارد.

```
POST /api/user/beth_m_bost/feed HTTP/1.1
Host: acmegamecorp.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": [
    "http://www.w3.org/ns/activitystreams",
    "http://www.w3.org/ns/activitypub"
  ],
  "@type": "Post",
  "actor": {
    "@type": "Person",
    "@id": "https://acmegamecorp.example/people/beth_m_bost/",
    "displayName": "Beth M. Bost"
  },
  "object": {
    "@id": "https://acmegamecorp.example/people/beth_m_bost/images/5150",
    "@type": "Image",
    "url": [{
        "@type": "Link",
        "href": "http://acmegamecorp.example/media/beth_m_bost/images/whiteboard_20150503.png",
        "mediaType": "image/png"
    }]
  },
  "to": [{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@id": "http://activityschema.org/collection/public",
    "@type": "Collection"
  }]
}
```

دفعهٔ بعد که دلانو به صندوق ورودی‌اش نگاه می‌کند، این شیء را در میان
موارد دیگر می‌بیند:

```
GET /api/user/delano_sota/inbox HTTP/1.1
Host: acmegamecorp.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": [
    "http://www.w3.org/ns/activitystreams",
    "http://www.w3.org/ns/activitypub"
  ],
  "@id": "https://acmegamecorp.example/api/user/delano_sota/inbox",
  "@type": "Collection",
  "itemsType": ["activity"]
  "items": [
    {
      "@context": [
        "http://www.w3.org/ns/activitystreams",
        "http://www.w3.org/ns/activitypub"
      ],
      "@type": "Post",
      "actor": {
        "@type": "Person",
        "@id": "https://acmegamecorp.example/people/beth_m_bost/",
        "displayName": "Beth M. Bost"
      },
      "object": {
        "@id": "https://acmegamecorp.example/people/beth_m_bost/images/5150",
        "@type": "Image",
        "url": [{
            "@type": "Link",
            "href": "http://acmegamecorp.example/media/beth_m_bost/images/whiteboard_20150503.png",
            "mediaType": "image/png"
        }]
      },
      "to": [{
        "@context": "http://www.w3.org/ns/activitystreams",
        "@id": "http://activityschema.org/collection/public",
        "@type": "Collection"
      }]
    }
    /* More objects here */
  ]
}
```

## ۵: دوستانی که دوستان را پیدا می‌کنند و دنبال می‌کنند

تد به خط زمانی غم‌انگیز و نسبتاً ساکت خود نگاه می‌کند و به این فکر می‌افتد که باید
کسی جالب را دنبال کند.  می‌بیند که دلانو اخیراً چند پستِ بث را
پسندیده و با دنبال‌کنندگانش به اشتراک گذاشته است.  او در رابط کلاینتش
روی نام بث کلیک می‌کند و «اشتراک» را انتخاب می‌کند.  در پشت صحنه،
همان فرایندی که هنگام مشترک شدن دلانو در بث رخ داد دوباره
رخ می‌دهد، اما این بار تد در بث مشترک می‌شود.

(به گمانم نمونهٔ بالا به اندازهٔ کافی گویاست و نیازی به تکرار
آن درخواست‌ها نیست.)


## ۶: لغو دنبال کردن کسی

دلانو درگیر یک پروژهٔ فوری است و باید با دقت به پیام‌های
هم‌تیمی‌های نزدیکش توجه کند، اما از پست‌های پرشمار بث احساس
سردرگمی می‌کند و همین کار مرتب کردن صندوق ورودی را برایش دشوار
کرده است.  تصمیم می‌گیرد اشتراکش را لغو کند، دست‌کم
فعلاً.  روی نام کاربری بث کلیک می‌کند و «لغو اشتراک» را انتخاب می‌کند.

کلاینت او فعالیتی از نوع «Unfollow» را به صندوق خروجی‌اش
ارسال می‌کند:

```
POST /api/user/delano_sota/feed HTTP/1.1
Host: acmegamecorp.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": "http://www.w3.org/ns/activitystreams",
  "@type": "Unfollow",
  "actor": {
    "@type": "Person",
    "@id": "https://acmegamecorp.example/people/delano_sota/",
    "displayName": "Delano Sota",
  },
  "object": {
    "@type": "Person",
    "@id": "https://acmegamecorp.example/people/beth_m_bost/"
  },
  "to": [{
      "@type": "Person",
      "@id": "https://acmegamecorp.example/people/beth_m_bost/"
  }],
  "cc": [{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@id": "http://activityschema.org/collection/public",
    "@type": "Collection"
  }]
}
```

سرور دلانو فعالیت بالا را به صندوق ورودی بث و همچنین
صندوق‌های ورودی دنبال‌کنندگان عمومی‌اش می‌فرستد.  سرور دلانو بث را
از مجموعهٔ «following» او حذف می‌کند و سرور بث دلانو را از
مجموعهٔ «followers» بث حذف می‌کند.  از این پس، سرور بث
فعالیت‌هایی را که خطاب به دنبال‌کنندگان عمومی او هستند، برای دلانو
نخواهد فرستاد.
