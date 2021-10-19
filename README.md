# 目次

- [モック API として使いたい場合の紹介](#モック-api-として使いたい場合の紹介)
- [REST API として使いたい場合の紹介](#rest-api-として使いたい場合の紹介)

# モック API として使いたい場合の紹介

## 手順

1. `db/database.json` を欲しい json データに書き換える

例としてTwitter API ([Tweet lookup](https://developer.twitter.com/en/docs/twitter-api/tweets/lookup/quick-start))のレスポンスデータを参考にしています。

```json:json-mock-api/package.json
{
  "data": [
    {
      "author_id": "2244994945",
      "created_at": "2020-02-14T19:00:55.000Z",
      "id": "1228393702244134912",
      "text": "What did the developer write in their Valentine’s card?\n  \nwhile(true) {\n    I = Love(You);  \n}"
    },
    {
      "author_id": "2244994945",
      "created_at": "2020-02-12T17:09:56.000Z",
      "id": "1227640996038684673",
      "text": "Doctors: Googling stuff online does not make you a doctor\n\nDevelopers: https://t.co/mrju5ypPkb"
    },
    {
      "author_id": "2244994945",
      "created_at": "2019-11-27T20:26:41.000Z",
      "id": "1199786642791452673",
      "text": "C#"
    }
  ]
}
```

2. 実際にサーバを起動してモック API を使ってみます

サーバを起動する

```bash
$ yarn json-server -p 5000

  \{^_^}/ hi!

  Loading db/database.json
  Done

  Resources
  http://localhost:5000/data

  Home
  http://localhost:5000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```

curl コマンドでモック API が取得できるか確認

```bash
curl -X GET http://localhost:5000/data

[
  {
    "author_id": "2244994945",
    "created_at": "2020-02-14T19:00:55.000Z",
    "id": "1228393702244134912",
    "text": "What did the developer write in their Valentine’s card?\n  \nwhile(true) {\n    I = Love(You);  \n}"
  },
  {
    "author_id": "2244994945",
    "created_at": "2020-02-12T17:09:56.000Z",
    "id": "1227640996038684673",
    "text": "Doctors: Googling stuff online does not make you a doctor\n\nDevelopers: https://t.co/mrju5ypPkb"
  },
  {
    "author_id": "2244994945",
    "created_at": "2019-11-27T20:26:41.000Z",
    "id": "1199786642791452673",
    "text": "C#"
  }
]
```

# REST API として使いたい場合の紹介

例して `db/database.json`にある`books`の json データで確認します。
サーバを起動させcurl コマンドを実行させ確認します。

### GET(LIST 表示)

```bash
$ curl -X GET http://localhost:5000/books
HTTP/1.1 200 OK
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 144
ETag: W/"90-wvsfD31/fpBASNE+ERjsBpuxM8U"
Date: Tue, 19 Oct 2021 07:46:33 GMT
Connection: keep-alive
Keep-Alive: timeout=5

[
  {
    "id": 1,
    "title": "title01",
    "author": "typicode"
  },
  {
    "id": 2,
    "title": "title02",
    "author": "typicode"
  }
]
```

### GET(詳細表示)

```bash
$ curl -X GET http://localhost:5000/books/1
HTTP/1.1 200 OK
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 59
ETag: W/"3b-PW916GTRjdRywA3+kqQpgOSrwFE"
Date: Sun, 17 Oct 2021 05:09:42 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{
  "id": 1,
  "title": "title01",
  "author": "typicode"
}
```

### POST

```bash
$ curl -i -X POST http://localhost:5000/books \
       --header 'content-type: application/json' \
       --data '{"title": "title03", "author": "typicode"}'
HTTP/1.1 201 Created
X-Powered-By: Express
Vary: Origin, X-HTTP-Method-Override, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
Access-Control-Expose-Headers: Location
Location: http://localhost:5000/books/3
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 59
ETag: W/"3b-1Ej1Mg/w6x3Q01b7rZteiJ0svxs"
Date: Sun, 17 Oct 2021 05:10:02 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{
  "title": "title03",
  "author": "typicode",
  "id": 3
}
```

### PUT

```bash
$ curl -i -X PUT http://localhost:5000/books/3 \
       --header 'content-type: application/json' \
       --data '{"title": "changed-title03", "author": "changed-author"}'
HTTP/1.1 200 OK
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 73
ETag: W/"49-ZtM4B7vSxsEg8F3yHiW/+6T2irc"
Date: Sun, 17 Oct 2021 05:10:32 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{
  "title": "changed-title03",
  "author": "changed-author",
  "id": 3
}
```

### DELETE

```bash
$ curl -X DELETE http://localhost:5000/books/3
HTTP/1.1 200 OK
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 2
ETag: W/"2-vyGp6PvFo4RvsFtPoIWeCReyIC8"
Date: Sun, 17 Oct 2021 05:11:05 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{}
```

### エラー回り

```bash
// 存在しないリソースをURIに指定した場合
$ curl -i -X GET http://localhost:5000/dummy
HTTP/1.1 404 Not Found
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 2
ETag: W/"2-vyGp6PvFo4RvsFtPoIWeCReyIC8"
Date: Sun, 17 Oct 2021 05:19:49 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{}

// books id=10 がdb/database.json にない場合
$ curl -i -X GET http://localhost:5000/books/10
HTTP/1.1 404 Not Found
X-Powered-By: Express
Vary: Origin, Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 2
ETag: W/"2-vyGp6PvFo4RvsFtPoIWeCReyIC8"
Date: Sun, 17 Oct 2021 05:20:55 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{}
```
