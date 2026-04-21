# User Store actions

## user/initialize

Rough explanation:

- Get "auth" from local storage (`n`)
- If exists, call USER_SET_AUTH_INFO with data:

  ```typescript
  {
    accessToken: n.accessToken,
    refreshToken: n.refreshToken,
    expirationDate: n.expirationDate,
    verified: n.verified
  }
  ```

Used by:

- [dispatch] app.\*.js
- If not exists, remove "auth" from local storage

## user/checkUserInfo (async)

Rough explanation:

- If user.state.userInfo [is empty](../../utils/app.md#2b03---isempty), dispatch action user/fetchUserInfo. Otherwise, do nothing

Used by:

- [dispatch] app.\*.js

## user/fetchStudents (async)

Accepts 1 argument:

- arg_0:
  ```typescript
  interface IArg0 {
    userId: string;
    periods: string;
  }
  ```

Rough explanation:

- Fetch GET /course/students?filter[user_id][is]=`{{arg_0.userId}}`&filter[period_id][in]=`{{arg_0.periods}}`
- Internal generic parser

Used by: **none**

## user/fetchLecturer

Accepts 1 argument:

- arg_0: any (possibly `string`)

Rough explanation:

- Fetch GET /course/lecturers/`{{arg_0}}`
- Internal generic parser

Used by:

- [dispath] course/public.\*.js
- [dispath] exam/start.\*.js
- [dispath] survey/list-lecturer.\*.js

## user/fetchAccessToken

Rough explanation:

- `throw new Error()`

Used by: **none**

## user/login

Accepts 1 argument:

- arg_0:

  ```typescript
  interface IArg0 {
    username: string;
    password: string;
  }
  ```

Rough explanation:

- Call mutation user/USER_LOGIN_REQUEST
- Fetch POST /user/auth/login with body:
  ```typescript
  {
    username: arg_0.username,
    password: arg_0.password,
  }
  ```
- Let `response` is the JSON response
- Let `var_1` contains this object:

  ```typescript
    {
      accessToken: response.access_token,
      refreshToken: response.refresh_token,
      expirationDate: (/* moment.js */)(1e5 * response.expires_in),
      verified: true,
    }
  ```

- Call mutation user/USER_SET_AUTH_INFO with arg_0: `var_1`
- Set into local storage with key "auth" and value `var_1`

Used by:

- [dispath] webhook-sso.\*.js
- [dispath] pages/login.\*.js

## user/switchUser

Accepts 1 argument:

- arg_0:

  ```typescript
  interface IArg0 {
    id: string;
  }
  ```

Rough explanation:

- Call mutation user/USER_LOGIN_REQUEST
- Let `var_1` is a base64 encoded of `arg_0.id`
- Fetch GET https://sso-edunex.itb.ac.id/switch/`{{var_1}}`
- Let `response` is the JSON response
- Let `var_1` contains this object:

  ```typescript
    {
      accessToken: response.access_token,
      refreshToken: response.refresh_token,
      expirationDate: (/* moment.js */)(1e5 * response.expires_in),
      verified: true,
    }
  ```

- Call mutation user/USER_SET_AUTH_INFO with arg_0: `var_1`
- Set into local storage with key "auth" and value `var_1`
- Dispatch action user/fetchUserInfo
- Call mutation user/USER_LOGIN_SUCCESS

Used by:

- [dispath] main.\*.js

## user/login_sso

Accepts 1 argument:

- arg_0:
  ```typescript
  interface IArg0 {
    authInfo: {
      access_token: string;
      refresh_token: string;
      expires_in: number;
      accounts: any[];
    };
  }
  ```

Rough explanation:

- Call mutation user/USER_LOGIN_REQUEST
- Let `var_1` contains this object:
  ```typescript
  {
    accessToken: arg_0.access_token,
    refreshToken: arg_0.refresh_token,
    expirationDate: ( /* moment.js */)(1e5 * arg_0.expires_in),
    accounts: arg_0.accounts,
    verified: !0,
  };
  ```
- Call mutation user/USER_SET_AUTH_INFO with data: `var_1`
- Set into local storage with key "auth" and value `var_1`
- Call mutation user/USER_LOGIN_SUCCESS

Used by:

- [dispatch] webhook-sso.\*.js
- [dispatch] pages/login.\*.js

## user/logout

Rough explanation:

- Call mutation user/USER_LOGOUT_REQUEST
- Clear local storage
- Call mutation user/USER_SET_AUTH_INFO with data:
  ```typescript
  {
    accessToken: "",
    refreshToken: "",
    expirationDate: 0,
    verified: false,
  }
  ```
- Call mutation user/USER_SET_USER_INFO with data: `{}`
- Call mutation user/USER_LOGOUT_SUCCESS

Used by:

- [dispatch] app.\*.js

## user/fetchUserInfo

Rough explanation

- Call mutation user/USER_INFO_FETCH_REQUEST
- Fetch GET /login/me
- Dispatch action lptk/fetchLptkById
- If the task throw error, call mutation user/USER_INFO_FETCH_FAILED with data: thrown error value
- If success, call mutation user/USER_INFO_SET with data:
  ```typescript
  // Types based on /login/me, /stats/user/*, and /public/lptk responses
  interface ISuccessReturn {
    id: number | undefined;
    username: string | undefined;
    fullname: string | undefined;
    email: string | undefined;
    avatar: string | undefined;
    university: string | undefined;
    universityId: string | undefined;
    lastOnline: string | Date | undefined;
    nidn: string | undefined;
    nim: string | undefined;
    lecturer_id: number | undefined; // Assuming same as student_id
    student_id: number | undefined;
    student_ids: Array<number> | undefined;
    period_id: number | undefined;
    notification_email: string | null | undefined;
    stats:
      | {
          user: Record<string, any>;
          course: number;
          class: number;
          major: string;
          faculty: string;
        }
      | undefined;
  }
  ```
