# خواندن پست‌های اخیر یک کاربر

برگرفته از https://www.w3.org/wiki/Socialwg/Social_API/User_stories#Reading_a_user.27s_recent_posts

> 1. آیریس (Iris) نظری را که سم (Sam) زیر یکی از عکس‌هایش نوشته خنده‌دار می‌یابد. او دوست دارد پست‌های بیشتری از سم بخواند.
> 2. آیریس آخرین یادداشت‌های سم را می‌خواند. او همچنین آخرین عکس‌های سم را مرور می‌کند.

## ۱. یافتن محتوای بیشتر از یک نویسنده

آیریس نظری را که سم زیر یکی از عکس‌هایش نوشته خنده‌دار می‌یابد.

```
GET /api/user/iris/inbox
Host: example.se
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": [
    "http://www.w3.org/ns/activitystreams",
    "http://www.w3.org/ns/activitypub"
  ],
  "@id": "https://example.se/~iris/my-stream",
  "@type": "Collection",
  "itemsType": ["activity"]
  "items": [
    {
      "@context": "http://www.w3.org/ns/activitystreams",
      "actor": "http://samsplace.example/sam/",
      "verb": "Post",      
      "object": {
        "@id": "https://samsplace.example/samsthoughts/note/41",
        "@type": "Note",
        "actor": "http://samsplace.example/sam/",
        "content": "That dog is as tired as a dog!",
        "inReplyTo": {
          "@type": "Image",
          "@id": "https://example.se/~iris/photos/long-run-with-dog/",
          "url" [
            {
              "href": "http://example.se/~iris/media/tired_dog.png",
              "mediaType": "image/png"
            }
          ]
        }
      }
    }
  ]
}
```

## ۲. خواندن محتوای اخیر یک نویسنده

آیریس دوست دارد نظرهای بیشتری از این دست از سم دربارهٔ حیوانات خانگی‌اش ببیند.  اگر آیریس
از یک رابط وب استفاده می‌کرد، کافی بود به وب‌سایت سم در
http://samsplace.example/sam/ سر بزند که از فیلد `actor` به آن پیوند داده شده است؛ در آنجا
می‌توانست آخرین شوخی‌ها و لطیفه‌های سم را در مرورگرش به‌صورت HTML ساده بخواند.

اما در حال حاضر او از یک کلاینت تلفن همراه استفاده می‌کند.  روی دکمهٔ «مشاهدهٔ
آخرین پست سم» می‌زند.  در پشت صحنه، کلاینت او یک درخواست GET
به http://samsplace.example/sam/ می‌فرستد و
[json-ld جاسازی‌شده](http://www.w3.org/TR/json-ld/#embedding-json-ld-in-html-documents)
را می‌بیند که یک نوع Actor از ActivityStreams را توصیف می‌کند.  (یادداشت: هنوز باید
دربارهٔ این روش به توافق رسید؛ برای جزئیات
[مسئلهٔ ۲۴](https://github.com/w3c-social/activitypub/issues/24)
را ببینید)


```
GET /sam/
Host: samsplace.example
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
        "@id": "https://samsplace.example/sam/",
        "following": "https://samsplace.example/following.json",
        "followers": "https://samsplace.example/followers.json",
        "inbox": "https://samsplace.example/inbox.json",
        "outbox": "https://samsplace.example/feed.json",
        "preferredUsername": "sam",
        "displayName": "Sam",
        "summary": "Sam's here to make you laugh.",
        "icon": [
          "https://samsplace.example/pics/sam_smirk.png"
        ]
      }
    </script>
  </head>
  <body>
    <h1>Sam's Jokes and Japes</h1>
    <!-- Content goes here ;) -->
  </body>
</html>
```

کلاینت آیریس صندوق خروجی سم را می‌آورد

```
GET https://samsplace.example/feed.json
Host: samsplace.example
Content-Type: application/activitystreams+json
Authorization: Bearer xx-bearer-token-here-xx

{
  "@context": [
    "http://www.w3.org/ns/activitystreams",
    "http://www.w3.org/ns/activitypub"
  ],
  "@id": "https://samsplace.example/~sam/my-stream",
  "@type": "Collection",
  "itemsType": ["activity"]
  "items": [
    {
      "@context": "http://www.w3.org/ns/activitystreams",
      "actor": "http://samsplace.example/sam/",
      "verb": "Post",      
      "object": {
        "@id": "https://samsplace.example/samsthoughts/note/41",
        "@type": "Note",
        "actor": "http://samsplace.example/sam/",
        "content": "That dog is as tired as a dog!",
        "inReplyTo": {
          "@type": "Image",
          "@id": "https://example.se/~iris/photos/long-run-with-dog/",
          "url" [
            {
              "href": "http://example.se/~iris/media/tired_dog.png",
              "mediaType": "image/png"
            }
          ]
        }
      }
    },
    {
      "@context": "http://www.w3.org/ns/activitystreams",
      "actor": "http://samsplace.example/sam/",
      "verb": "Post",      
      "object": {
        "@type": "Note",
        "@id": "https://samsplace.example/samsthoughts/note/40",
        "actor": "http://samsplace.example/sam/",
        "content": "Why'd the chicken cross the road?  To get away from the wolf!",
      }
    }
  ]
}
```

کلاینت تلفن همراه آیریس پست‌های اخیر سم را در یک زبانهٔ جدید نمایش می‌دهد.  آن
شوخی مرغ واقعاً بامزه بود!
