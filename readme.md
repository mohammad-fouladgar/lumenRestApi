# Lumen with JWT Authentication
Basically this is a starter kit for you to integrate Lumen with [JWT Authentication](https://jwt.io/).
If you want to Lumen + Dingo + JWT for your current application.


## Quick Start

- Clone this repo or download it's release archive and extract it somewhere
- Run `composer install`
- Run `php artisan jwt:generate`
- Configure your `.env` file for authenticating via database
- Run `php artisan migrate --seed`

## A Live PoC

- Run a PHP built in server from your root project:

```sh
php -S localhost:8000 -t public/
```

To authenticate a user, make a `POST` request to `/api/auth/login` with parameter as mentioned below:

```
email: info@example.com
password: 1234
```

Request:

```sh
curl -X POST -F "email=info@example.com" -F "password=1234" "http://localhost:8000/api/auth/login"
```

Response:

```
{
  "success": {
    "message": "token_generated",
    "token": "a_long_token_appears_here"
  }
}
```

- With token provided by above request, you can check authenticated user by sending a `GET` request to: `/api/auth/user`.

Request:

```sh
curl -X GET -H "Authorization: Bearer a_long_token_appears_here" "http://localhost:8000/api/auth/user"
```

Response:

```
{
  "success": {
    "user": {
      "id": 1,
      "name": "mohammad",
      "email": "info@example.com",
      "created_at": null,
      "updated_at": null
    }
  }
}
```

- To refresh your token, simply send a `PATCH` request to `/api/auth/refresh`.
- Last but not least, you can also invalidate token by sending a `DELETE` request to `/api/auth/invalidate`.
- To list all registered routes inside your application, you may execute `php artisan route:list`

```
⇒  php artisan route:list
+--------+----------------------+---------------------+------------------------------------------+------------------+------------+
| Verb   | Path                 | NamedRoute          | Controller                               | Action           | Middleware |
+--------+----------------------+---------------------+------------------------------------------+------------------+------------+
| POST   | /api/auth/login      | api.auth.login      | App\Http\Controllers\Auth\AuthController | postLogin        |            |
| GET    | /api                 | api.index           | App\Http\Controllers\APIController       | getIndex         | jwt.auth   |
| GET    | /api/auth/user       | api.auth.user       | App\Http\Controllers\Auth\AuthController | getUser          | jwt.auth   |
| PATCH  | /api/auth/refresh    | api.auth.refresh    | App\Http\Controllers\Auth\AuthController | patchRefresh     | jwt.auth   |
| DELETE | /api/auth/invalidate | api.auth.invalidate | App\Http\Controllers\Auth\AuthController | deleteInvalidate | jwt.auth   |
+--------+----------------------+---------------------+------------------------------------------+------------------+------------+
```

