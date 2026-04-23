# User mutations

## user/USER_LOGOUT_REQUEST

```typescript
state.indicators.loggingOut = true;
state.indicators.logoutState = "neutral";
state.indicators.logoutFailedReason = null;
```

Used by:

- [action] user/logout

## user/USER_SET

Accepts 1 argument:

- arg_0: unknown

```typescript
state.user = arg_0;
```

Used by: **none**

## user/USER_LOGOUT_FAILED

Accepts 1 argument:

- arg_0: unknown

```typescript
state.indicators.loggingOut = false;
state.indicators.logoutState = "failed";
state.indicators.logoutFailedReason = arg_0;
```

Used by: **none**

## user/USER_LOGOUT_SUCCESS

```typescript
state.indicators.loggingOut = false;
state.indicators.logoutState = "succeed";
state.indicators.logoutFailedReason = null;
```

Used by:

- [action] user/logout

## user/USER_LOGIN_REQUEST

```typescript
state.indicators.loggingIn = true;
state.indicators.loginState = "neutral";
state.indicators.loginFailedReason = null;
```

Used by:

- [action] user/login
- [action] user/switchUser
- [action] user/login_sso

## user/USER_LOGIN_FAILED

Accepts 1 argument:

- arg_0: unknown

```typescript
state.indicators.loggingIn = false;
state.indicators.loginState = "failed";
state.indicators.loginFailedReason = arg_0;
```

Used by: **none**

## user/USER_LOGIN_SUCCESS

```typescript
state.indicators.loggingIn = false;
state.indicators.loginState = "succeed";
state.indicators.loginFailedReason = null;
```

Used by:

- [action] user/login
- [action] user/switchUser
- [action] user/login_sso
- [web_js] pages/login.js

## user/USER_SET_AUTH_INFO

Accepts 1 argument:

- arg_0:
  ```typescript
  interface IArg0 {
    accessToken: string;
    refreshToken: string;
    verified: boolean;
    expirationDate: unknown | null;
  }
  ```

```typescript
state.loginInfo = {
  ...state.loginInfo,
  verified: arg_0.verified,
  refreshToken: arg_0.refreshToken,
  accessToken: arg_0.accessToken,
  expirationDate: arg_0.expirationDate,
};
```

Used by:

- [action] user/initialize
- [action] user/login
- [action] user/switchUser
- [action] user/login_sso
- [action] user/logout

## user/USER_SET_USER_INFO

Accepts 1 argument:

- arg_0: unknown

```typescript
state.userInfo = {
  ...state.userInfo,
  payload: arg_0,
};
```

Used by:

- [action] user/logout

## user/USER_INFO_FETCH_REQUEST

```typescript
state.indicators.userInfo = {
  status: "neutral",
  fetching: true,
  reason: null,
};
```

Used by:

- [action] user/fetchUserInfo

## user/USER_INFO_FETCH_FAILED

Accepts 1 argument:

- arg_0: any (possibly `instanceof Error`)

```typescript
state.indicators.userInfo = {
  status: "failed",
  fetching: false,
  reason: arg_0,
};
```

Used by:

- [action] user/fetchUserInfo
- [action] user/fetchUniversities
- [action] user/fetchProdi
- [action] user/fetchStudent
- [action] user/fetchLecturerDikti
- [action] user/postForm

## user/USER_INFO_SET

Accepts 1 argument:

- arg_0:
  ```typescript
  // Types based on /login/me, /stats/user/*, and /public/lptk responses
  interface IArg0 {
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

```typescript
state.userInfo = arg_0;
```

Used by:

- [action] user/fetchUserInfo
- [action] user/fetchUserStats
- [action] user/fetchProdi
- [web_js] layouts/main.js
