# LobbyMaster API List.

### 请求结构
Headers:
| Key | Type | Description |
| :--- | :---: | :---: |
| Authorization | string | 用户Token，若未登录则为空（有效期10分钟） |


### 响应结构
| Key | Type | Description |
| :--- | :---: | :---: |
| code | int | 状态码 |
| message | string | 状态描述 |
| data | T | 业务数据 |
| timestamp | int64 | 响应时间戳 |


Golang示例
```golang
type ApiResponse[T any] struct {
  Code      int    `json:"code"`
  Message   string `json:"message"`
  Data      T      `json:"data"`
  Timestamp int64  `json:"timestamp"`
}
```

TypeScript示例
```typescript
interface ApiResponse<T> {
  code: number;
  message: string;
  data: T;
  timestamp: number;
}
```

## **用户注册**

**用户手机号注册** (101)
- URL: /api/user-register/phone
- Method: POST
- Request (json):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | username | string | 用户名 |
  | password | string | 密码 -> hash64 -> md5 |
  | phone | string | 手机号 |
  | code | int | 验证码 |
  | picture_code | string | 图片验证码 |
- Response (ApiRequest):

## **用户登录**

**用户密码登录** (201)
- URL: /api/user-login/password
- Method: POST
- Request (json):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | username | string | 用户名 |
  | password | string | 密码 -> hash64 -> md5 |
  | picture_code | string | 图片验证码 |
- Response (ApiResponse[json]):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | token | string | 用户Token |

**用户手机登录** (202)
- URL: /api/user-login/phone
- Method: POST
- Request (json):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | phone | string | 手机号 |
  | code | int | 验证码 |
  | picture_code | string | 图片验证码 |
- Response (ApiResponse[json]):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | token | string | 用户Token |

**用户登录Token登录** (203)
- URL: /api/user-login/token
- Method: POST
- Request (json):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | login_token | string | 登录Token（不是用户Token，存于Cookie中） |
- Response (ApiResponse[json]):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | token | string | 用户Token |

## **用户信息**

**获取用户信息**
- URL: /api/user/profile
- Method: GET
- Response (ApiResponse[json]):
  | Key | Type | Description |
  | :--- | :---: | :---: |
  | name | string | 用户名 |
  | phone | string | 手机号 |