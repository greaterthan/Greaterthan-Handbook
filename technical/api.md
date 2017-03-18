(from [Eugene](https://gist.github.com/data-doge/836798b12459bdae661d235cd889a77f), thanks Eugene!)

# cobudget auth

exchange **email and password** for **access-token, uid, client, and token-type**

```bash
$ curl -i https://cobudget-beta-api.herokuapp.com/api/v1/auth/sign_in -X POST -F email="eugene@enspiral.com" -F password="xxxxxxxx"
HTTP/1.1 100 Continue

HTTP/1.1 200 OK
Server: Cowboy
Date: Fri, 17 Mar 2017 15:35:08 GMT
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Access-Token: YHCPAE3o2bfErAOs7xevqw
Token-Type: Bearer
Client: cemCe_Xl8IJSgEfuRc-MQg
Expiry: 1490974509
Uid: eugene@enspiral.com
Content-Type: application/json; charset=utf-8
Vary: Origin
Etag: W/"bfc08006b8912a30f03f17c45f7f2945"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: 3474acf1-84a5-4164-867f-37852706d3b0
X-Runtime: 0.300109
Transfer-Encoding: chunked
Via: 1.1 vegur

{"data":{"id":220,"email":"eugene@enspiral.com","name":"Eugene Lynch","provider":"email","uid":"eugene@enspiral.com","utc_offset":-300,"joined_first_group_at":"2015-08-30T08:03:23.332Z","is_super_admin":true}}%
```

make request, with **access-token, uid, client, and token-type** included in the headers

```bash
$ curl -i https://cobudget-beta-api.herokuapp.com/api/v1/users/me -H 'access-token: YHCPAE3o2bfErAOs7xevqw' -H 'uid: eugene@enspiral.com' -H 'client: cemCe_Xl8IJSgEfuRc-MQg' -H 'token-type: Bearer'
HTTP/1.1 200 OK
Server: Cowboy
Date: Fri, 17 Mar 2017 15:42:19 GMT
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Access-Token: 2nvoih-eixWmb7RhqpJnrg
Token-Type: Bearer
Client: cemCe_Xl8IJSgEfuRc-MQg
Expiry: 1490974940
Uid: eugene@enspiral.com
Content-Type: application/json; charset=utf-8
Vary: Origin
Etag: W/"230effcbdac862d34fb788acd4281046"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: 848ff31b-d7a3-40f2-8253-e9200ec1c367
X-Runtime: 0.176118
Transfer-Encoding: chunked
Via: 1.1 vegur

{"subscription_trackers":[{"id":20,"subscribed_to_email_notifications":true,"email_digest_delivery_frequency":"weekly"}],"users":[{"id":220,"name":"Eugene Lynch","email":"eugene@enspiral.com","utc_offset":-300,"confirmed_at":"2016-01-23T02:49:55.496Z","joined_first_group_at":"2015-08-30T08:03:23.332Z","is_super_admin":true,"subscription_tracker_id":20}]}%
```

**try it again**

```bash
$ curl -i https://cobudget-beta-api.herokuapp.com/api/v1/users/me -H 'access-token: YHCPAE3o2bfErAOs7xevqw' -H 'uid: eugene@enspiral.com' -H 'client: cemCe_Xl8IJSgEfuRc-MQg' -H 'token-type: Bearer'
HTTP/1.1 401 Unauthorized
Server: Cowboy
Date: Fri, 17 Mar 2017 15:42:46 GMT
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Vary: Origin
Cache-Control: no-cache
X-Request-Id: b84564c6-4adb-48fc-9812-03577cda454e
X-Runtime: 0.103573
Transfer-Encoding: chunked
Via: 1.1 vegur

{"errors":["Authorized users only."]}%
```

_huh, that's odd, why didn't it work?_

with every authenticated request we make, we send an `access-token` over to the server. when the server gets it, it destroys the token and returns a new one in the response headers. 

so, looking back, in our last successful authenticated request, we got a new token in the response headers: `Access-Token: 2nvoih-eixWmb7RhqpJnrg`

let's try the same request, with that token instead

```bash
$ curl -i https://cobudget-beta-api.herokuapp.com/api/v1/users/me -H 'access-token: 2nvoih-eixWmb7RhqpJnrg' -H 'uid: eugene@enspiral.com' -H 'client: cemCe_Xl8IJSgEfuRc-MQg' -H 'token-type: Bearer'
HTTP/1.1 200 OK
Server: Cowboy
Date: Fri, 17 Mar 2017 15:44:08 GMT
Connection: keep-alive
Strict-Transport-Security: max-age=31536000
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Access-Token: 7wgKt9BdIB8VScAnp0NEEw
Token-Type: Bearer
Client: cemCe_Xl8IJSgEfuRc-MQg
Expiry: 1490975049
Uid: eugene@enspiral.com
Content-Type: application/json; charset=utf-8
Vary: Origin
Etag: W/"230effcbdac862d34fb788acd4281046"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: 69478a8a-6605-4dc4-a79c-9c3872135ff6
X-Runtime: 0.165583
Transfer-Encoding: chunked
Via: 1.1 vegur

{"subscription_trackers":[{"id":20,"subscribed_to_email_notifications":true,"email_digest_delivery_frequency":"weekly"}],"users":[{"id":220,"name":"Eugene Lynch","email":"eugene@enspiral.com","utc_offset":-300,"confirmed_at":"2016-01-23T02:49:55.496Z","joined_first_group_at":"2015-08-30T08:03:23.332Z","is_super_admin":true,"subscription_tracker_id":20}]}%
```

_success_
