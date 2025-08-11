# LobbyMaster API 接口文档

## 接口调用说明

### 1. 请求头（Headers）说明
所有需要验证的接口都需要在请求头中携带以下参数：

| 参数名 | 类型 | 是否必填 | 说明 |
| :--- | :---: | :---: | :--- |
| Authorization | string | 是 | 登录后获取的用户Token。未登录时为空，Token有效期为10分钟 |

### 2. 响应格式说明
所有接口统一返回如下格式：

| 字段名 | 类型 | 说明 |
| :--- | :---: | :--- |
| code | int | 状态码，0表示正常返回，其他值表示失败 |
| message | string | 接口返回的提示信息 |
| data | T | 接口返回的具体数据，T代表不同接口返回的数据类型 |
| timestamp | int64 | 服务器响应的时间戳 |


### 3. 响应数据结构示例

**TypeScript 类型定义：**
```typescript
interface ApiResponse<T> {
  code: number;
  message: string;
  data: T;
  timestamp: number;
}
```

**Golang 类型定义：**
```golang
type ApiResponse[T any] struct {
  Code      int    `json:"code"`
  Message   string `json:"message"`
  Data      T      `json:"data"`
  Timestamp int64  `json:"timestamp"`
}
```

## 用户注册相关接口

### 手机号注册
> 通过手机号和验证码进行用户注册

- **接口路径：** `/api/user-register/phone`
- **请求方式：** POST
- **是否需要认证：** 否

#### 请求参数
| 参数名 | 类型 | 是否必填 | 说明 |
| :--- | :---: | :---: | :--- |
| username | string | 是 | 用户名，建议6-20个字符 |
| password | string | 是 | 密码，需要先进行hash64编码，再进行md5加密 |
| phone | string | 是 | 手机号，11位手机号码 |
| code | number | 是 | 手机验证码，通过短信获取的6位数字验证码 |
| picture_code | string | 是 | 图片验证码，注册页面显示的验证码 |

#### 响应数据
> 注册成功后返回空的data对象

## 用户登录相关接口

### 1. 账号密码登录
> 使用用户名和密码进行登录

- **接口路径：** `/api/user-login/password`
- **请求方式：** POST
- **是否需要认证：** 否

#### 请求参数
| 参数名 | 类型 | 是否必填 | 说明 |
| :--- | :---: | :---: | :--- |
| username | string | 是 | 用户名 |
| password | string | 是 | 密码，需要先hash64编码再md5加密 |
| picture_code | string | 是 | 图片验证码 |

#### 响应数据
| 参数名 | 类型 | 说明 |
| :--- | :---: | :--- |
| token | string | 用户认证Token，后续请求需要携带在Header中 |

### 2. 手机验证码登录
> 使用手机号和短信验证码进行登录

- **接口路径：** `/api/user-login/phone`
- **请求方式：** POST
- **是否需要认证：** 否

#### 请求参数
| 参数名 | 类型 | 是否必填 | 说明 |
| :--- | :---: | :---: | :--- |
| phone | string | 是 | 手机号 |
| code | number | 是 | 手机验证码 |
| picture_code | string | 是 | 图片验证码 |

#### 响应数据
| 参数名 | 类型 | 说明 |
| :--- | :---: | :--- |
| token | string | 用户认证Token |

### 3. Token自动登录
> 使用保存在Cookie中的登录Token进行自动登录（用于记住密码功能）

- **接口路径：** `/api/user-login/token`
- **请求方式：** POST
- **是否需要认证：** 否

#### 请求参数
| 参数名 | 类型 | 是否必填 | 说明 |
| :--- | :---: | :---: | :--- |
| login_token | string | 是 | 登录Token，从Cookie中获取 |

#### 响应数据
| 参数名 | 类型 | 说明 |
| :--- | :---: | :--- |
| token | string | 新的用户认证Token |

## 用户信息相关接口

### 获取用户个人信息
> 获取当前登录用户的基本信息

- **接口路径：** `/api/user/profile`
- **请求方式：** GET
- **是否需要认证：** 是

#### 请求参数
> 无需请求参数

#### 响应数据
| 参数名 | 类型 | 说明 |
| :--- | :---: | :--- |
| name | string | 用户名 |
| phone | string | 手机号，已脱敏处理 |