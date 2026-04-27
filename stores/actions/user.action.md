# User Store actions

## user/initialize

Rough explanation:

- Get "auth" from local storage (`n`)
- If exists, commit mutation user/USER_SET_AUTH_INFO with data:

  ```typescript
  {
    accessToken: n.accessToken,
    refreshToken: n.refreshToken,
    expirationDate: n.expirationDate,
    verified: n.verified
  }
  ```

- If does not exists, remove "auth" from local storage

Used by:

- [web_js] app.js

## user/checkUserInfo (async)

Rough explanation:

- If user.state.userInfo [is empty](../../utils/app.md#2b03---isempty), dispatch action user/fetchUserInfo. Otherwise, do nothing

Used by:

- [web_js] app.js

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

- Fetch GET "/course/students?filter[user_id][is]=`{{arg_0.userId}}`&filter[period_id][in]=`{{arg_0.periods}}`"
- Internal generic parser

Used by: **none**

## user/fetchLecturer (async)

Accepts 1 argument:

- arg_0: any (possibly `string`)

Rough explanation:

- Fetch GET "/course/lecturers/`{{arg_0}}`"
- Internal generic parser

Used by:

- [web_js] course/public.js
- [web_js] exam/start.js
- [web_js] survey/list-lecturer.js

## user/fetchAccessToken (async)

Rough explanation:

- `throw new Error()`

Used by: **none**

## user/login (async)

Accepts 1 argument:

- arg_0:

  ```typescript
  interface IArg0 {
    username: string;
    password: string;
  }
  ```

Rough explanation:

- Commit mutation user/USER_LOGIN_REQUEST
- Fetch POST "/user/auth/login" with body:
  ```typescript
  {
    username: arg_0.username,
    password: arg_0.password,
  }
  ```
- Let `response` is the JSON response
- Let `var_0` contains this object:

  ```typescript
    {
      accessToken: response.access_token,
      refreshToken: response.refresh_token,
      expirationDate: (/* moment.js */)(1e5 * response.expires_in),
      verified: true,
    }
  ```

- Commit mutation user/USER_SET_AUTH_INFO with arg_0: `var_0`
- Set into local storage with key "auth" and value `var_0`

Used by:

- [web_js] webhook-sso.js
- [web_js] pages/login.js

## user/switchUser (async)

Accepts 1 argument:

- arg_0:

  ```typescript
  interface IArg0 {
    id: string;
  }
  ```

Rough explanation:

- Commit mutation user/USER_LOGIN_REQUEST
- Let `var_0` is a base64 encoded of `arg_0.id`
- Fetch GET "https://sso-edunex.itb.ac.id/switch/ `{{var_0}}`"
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

- Commit mutation user/USER_SET_AUTH_INFO with arg_0: `var_1`
- Set into local storage with key "auth" and value `var_1`
- Dispatch action user/fetchUserInfo
- Commit mutation user/USER_LOGIN_SUCCESS

Used by:

- [web_js] main.js

## user/login_sso (async)

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

- Commit mutation user/USER_LOGIN_REQUEST
- Let `var_0` contains this object:
  ```typescript
  {
    accessToken: arg_0.access_token,
    refreshToken: arg_0.refresh_token,
    expirationDate: ( /* moment.js */)(1e5 * arg_0.expires_in),
    accounts: arg_0.accounts,
    verified: !0,
  };
  ```
- Commit mutation user/USER_SET_AUTH_INFO with data: `var_0`
- Set into local storage with key "auth" and value `var_0`
- Commit mutation user/USER_LOGIN_SUCCESS

Used by:

- [web_js] webhook-sso.js
- [web_js] pages/login.js

## user/logout (async)

Rough explanation:

- Commit mutation user/USER_LOGOUT_REQUEST
- Clear local storage
- Commit mutation user/USER_SET_AUTH_INFO with data:
  ```typescript
  {
    accessToken: "",
    refreshToken: "",
    expirationDate: 0,
    verified: false,
  }
  ```
- Commit mutation user/USER_SET_USER_INFO with data: `{}`
- Commit mutation user/USER_LOGOUT_SUCCESS

Used by:

- [web_js] app.js

## user/fetchUserInfo (async)

Rough explanation:

- Commit mutation user/USER_INFO_FETCH_REQUEST
- Fetch GET "/login/me"
- Dispatch action lptk/fetchLptkById
- If the task throw error, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value
- If success, commit mutation user/USER_INFO_SET with data:
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

Used by:

- [action] user/checkUserInfo
- [action] user/switchUser

## user/fetchUserStats (async)

Accepts 1 arguments:

- arg_0:
  ```typescript
  interface IArg0 {
    userId: string;
  }
  ```

Rough explanation:

- Fetch GET "/stats/user/`{{arg_0.userId}}`"
- Let `response` is a JSON parsed of the fetch response
- Commit mutation user/USER_INFO_SET with data:
  ```typescript
  { ...state.userInfo, stats: response.data };
  ```

Used by:

- [web_js] settings/profile-lecturer.js
- [web_js] settings/profile-student.js

## user/fetchUniversities (async)

Accepts 1 argument:

- arg_0: `string | number`

Rough explanation:

- Make a GET request to "https://api.kemdikbud.go.id:8243/pddikti/1.1/pt?per-page=10&q= `{{arg_0}}`" with header `{ Authorization: "Bearer 43a045c5-9c9e-3d1c-b75e-becb239fc8b5" }`
- Let `response` is a JSON parsed of the fetch response
- Return mapped array of `response.data`, which is:

  ```typescript
  {
    name: data.name,
    propinsi: data.propinsi.nama,
    code: data.id
  }
  ```

  where `data` is each element in the `response.data` array

- If the request or JSON parsing fails, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value

Used by:

- [web_js] course/public.js

## user/fetchProdi (async)

Accepts 1 argument:

- arg_0: `{ q: string; university: string }`

Rough explanation:

- Make a GET request to "https://api.kemdikbud.go.id:8243/pddikti/1.1/pt/ `{{arg_0.university}}`/prodi??per-page=10&q=`{{arg_0.q}}`" with header `{ Authorization: "Bearer 43a045c5-9c9e-3d1c-b75e-becb239fc8b5" }`
- Let `response` is a JSON parsed of the fetch response
- Return mapped array of `response.data`, which is:

  ```typescript
  {
    name: `${data.jenjang_didik.nama}-${data.nama}`,
    code: data.id
  }
  ```

  where `data` is each element in the `response.data` array

- If the request or JSON parsing fails, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value

Used by:

- [web_js] course/public.js

## user/fetchStudent (async)

Accepts 1 argument:

- arg_0: `{ nim: string; university: string; prodi: string; }`

Rough explanation:

- Make a GET request to "https://api.kemdikbud.go.id:8243/pddikti/1.1/pt/ `{{arg_0.university}}`/prodi/`{{arg_0.prodi}}`/mahasiswa/`{{arg_0.nim}}`" with header `{ Authorization: "Bearer 43a045c5-9c9e-3d1c-b75e-becb239fc8b5" }` and timeout `5e3`
- Let `response` is a JSON parsed of the fetch response
- Return mapped array of `response.data`, which is:

  ```typescript
  {
    name: data.nama,
  }
  ```

  where `data` is each element in the `response.data` array

- If the request or JSON parsing fails, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value

Used by:

- [web_js] course/public.js

## user/fetchLecturerDikti (async)

Accepts 1 argument:

- arg_0: `{ nidn: string }`

Rough explanation:

- Make a GET request to "https://api.kemdikbud.go.id:8243/pddikti/1.1/dosen?nidn= `{{arg_0.nidn}}`" with header `{ Authorization: "Bearer 43a045c5-9c9e-3d1c-b75e-becb239fc8b5" }` and timeout `5e3`
- Let `response` is a JSON parsed of the fetch response
- Return mapped array of `response.data`, which is:

  ```typescript
  {
    name: data.nama,
  }
  ```

  where `data` is each element in the `response.data` array

- If the request or JSON parsing fails, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value

Used by:

- [web_js] course/public.js

## user/postForm (async)

Accepts 1 argument:

- arg_0:
  ```typescript
  interface IArg0 {
    courseId: string;
    campus: unknown;
    roles: unknown;
    code: unknown;
    name: unknown;
    province: unknown;
    is_verify: unknown;
    ip_address: unknown;
  }
  ```

Rough explanation:

- Fetch POST "/public/view/course/`{{arg_0.courseId}}`" with body: `arg_0`
- Let `response` is a JSON parsed of the fetch response
- Return mapped array of `response.data`, which is:

  ```typescript
  {
    name: data.nama,
  }
  ```

  where `data` is each element in the `response.data` array

- If the request or JSON parsing fails, commit mutation user/USER_INFO_FETCH_FAILED with data: thrown error value

Used by:

- [web_js] course/public.js

## user/updateUserThumbnal (async)

Accepts 1 argument:

- arg_0: `string`

Rough explanation:

- Fetch PATCH /course/users/`{{state.userInfo.id}}` with data:

  ```typescript
  {
    data: {
      attributes: {
        thumbnail: arg_0;
      }
    }
  }
  ```

- If server responded with 401, remove "auth" from local storage and redirect into /pages/login
- Commit mutation user/USER_SET_AVATAR with data: `arg_0`

Used by:

- [web_js] settings/profile-lecturer~settings/profile-student.js

## user/updateNotifyEmail

Accepts 1 argument:

- arg_0: `string`

Rough explanation:

- Fetch PATCH /course/users/`{{state.userInfo.id}}` with data:

  ```typescript
  {
    data: {
      attributes: {
        notification_email: arg_0;
      }
    }
  }
  ```

- If server responded with 401, remove "auth" from local storage and redirect into /pages/login
- Commit mutation user/USER_SET_NOTIFICATION_EMAIL with data: `arg_0`

Used by:

- [web_js] settings/profile-lecturer~settings/profile-student.js

## user/fetchList

Accepts 1 arguments:

- arg_0:

  ```typescript
  interface IArg1 {
    filter: string;
    sort: string;
    include: string;
    pagination: {
      limit: number;
      offset: number;
    };
  }
  ```

Rough explanation:

- Fetch GET /course/users with these queries:
  - filter`arg_0.filter` (`arg_0.filter` contains `=` and is not sanitized)
  - include with value `arg_0.include` (`arg_0.include` is not sanitized)
  - sort with value `arg_0.sort` (`arg_0.sort` is not unsanitized)
  - page[limit] with value `arg_0.pagination.limit` if defined
  - page[offset] with value `arg_0.pagination.offset` if defined
- If server responded with 401, remove "auth" from local storage and redirect into /pages/login
- Let `response` is the JSON response
- Commit mutation user/USER_SET_LIST with data: `response.data`
- Returns `response`

Used by:

- [web_js] course/edit-learning-plan-lecturer.js
- [web_js] course/edit-lecturer.js
- [web_js] course/learning-plan-lecturer.js
- [web_js] message/list~message/sent.js
- [web_js] message/detail.js
- [web_js] message/new.js

## user/fetchById

Accepts 1 argument:

- arg_0: `number | string`

Rough explanation:

- Fetch GET /course/users/`{{arg_0}}`
- If server responded with 401, remove "auth" from local storage and redirect into /pages/login
- Returns the JSON response

Used by:

- [web_js] course/learning-plan-lecturer.js
- [web_js] message/detail.js

## user/updateProfile

Rough explanation:

- Let `var_0` is a JSON parsed value from a local storage with key "auth"
- Fetch GET `{{state.lptk.detail.update_url}}{{var_0.accessToken}}`
- If server responded with 401, remove "auth" from local storage and redirect into /pages/login
- Returns the JSON response

Used by: **none**
