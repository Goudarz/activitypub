# کاربر یک یادداشت منتشر می‌کند

برگرفته از https://www.w3.org/wiki/Socialwg/Social_API/User_stories#User_posts_a_note

> 1. اریک (Eric) یادداشت کوتاهی می‌نویسد تا با دنبال‌کنندگانش به اشتراک بگذارد.
> 2. پس از انتشار یادداشت، متوجه یک غلط املایی می‌شود. یادداشت را ویرایش می‌کند و دوباره منتشر می‌کند.
> 3. بعدها اریک به این نتیجه می‌رسد که اطلاعات یادداشت نادرست است. یادداشت را حذف می‌کند.

## ۱. انتشار یک یادداشت

اریک در کلاینت خود پستی می‌نویسد و کلاینت چیزی شبیه به این را به صندوق خروجی (خوراک) او ارسال می‌کند::

```
POST /api/user/eric/feed HTTP/1.1
Host: example.se
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@type": "Post",
    "object": {
        "@type": "Note",
        "content": "Helo World!"
    }
    "to": [
        {
            "id": "http://example.se/api/user/eric/feed",
            "@type": "Collection"
        }
    ]       
}
```

سرور در پاسخ، فعالیت کامل و شیئی را که هم‌اکنون ایجاد کرده است
به شما بازمی‌گرداند::

```
{
    "@id": "http://example.se/api/activity/1"
    "@context": "http://www.w3.org/ns/activitystreams",
    "@type": "Post",
    "author": "http://example.se/~erik",
    "published": "2015-05-04T19:01:55Z",
    "object": {
        "@id": "http://example.se/api/note/1"
        "@type": "Note",
        "author": "http://example.se/~erik",
        "content": "Helo World!",
        "published": "2015-05-04T19:01:55Z",
    }
    "to": [
        {
            "id": "http://example.se/api/user/eric/feed",
            "@type": "Collection"
        }
    ]       
}
```

## ۲. به‌روزرسانی یک یادداشت

متوجه می‌شود که غلط تایپی داشته و یادداشت را با مجموعهٔ خود به‌روزرسانی می‌کند::
```
POST /api/user/eric/feed HTTP/1.1
Host: example.se
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@type": "Update",
    "object": {
        "@id": "http://example.se/api/note/1"
        "@type": "Note",
        "content": "Hello World!"
    }
    "to": [
        {
            "id": "http://example.se/api/user/eric/feed",
            "@type": "Collection"
        }
    ]       
}
```

این بار نیز سرور صرفاً فعالیتی را که هم‌اکنون ارسال کردید به شما بازمی‌گرداند

```
{
    "@id": "http://example.se/api/activity/2"
    "@context": "http://www.w3.org/ns/activitystreams",
    "@type": "Update",
    "author": "http://example.se/~erik",
    "published": "2015-05-04T19:02:55Z",
    "object": {
        "@id": "http://example.se/api/note/1"
        "@type": "Note",
        "author": "http://example.se/~erik",
        "content": "Hello World!",
        "published": "2015-05-04T19:01:55Z",
        "updated": "2015-05-04T19:02:55Z"
    }
    "to": [
        {
            "id": "http://example.se/api/user/eric/feed",
            "@type": "Collection"
        }
    ]       
}
```

## ۳. حذف یک یادداشت

تصمیم می‌گیرد که نمی‌خواهد به دنیا سلام کند و پستش را حذف می‌کند::
```
POST /api/user/eric/feed HTTP/1.1
Host: example.se
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
    "@context": "http://www.w3.org/ns/activitystreams",
    "@type": "Delete",
    "object": {
        "@id": "http://example.se/api/note/1"
        "@type": "Note",
    }    
}
```

پوسته‌ای از یادداشت باقی می‌ماند که در فعالیت‌های پیشینی که قبلاً به این شیء
ارجاع داده‌اند، به آن ارجاع داده می‌شود. سرور همچنین ملزم خواهد بود هنگام
واکشی شناسه، وضعیت HTTP 410 GONE را بازگرداند. این پوسته
چنین شکلی دارد::
```
{
    "@id": "http://example.se/api/note/1",
    "@type": "Note",
    "published": "2015-02-10T15:04:55Z",
    "updated": "2015-02-10T15:04:55Z",
    "deleted": "2015-02-10T15:04:55Z"
}
```
