# User Store getters

## user/isLoggedIn

- Get "auth" from local storage
- Logs into browser console: `authInfo {{state.loginInfo.verified}} {{state.loginInfo.expirationDate > Date.now()}}`
- Returns true if all of these requirements met:
  - "auth" is exists
  - `state.loginInfo.verified` is true
  - `state.loginInfo.expirationDate` is greater than the current date time

Used by:

- [web_js] app.\*.js
- [web_js] pages/login.\*.js

## user/getUserInfo

- Returns `state.userInfo`

Used by:

- [web_js] app.\*.js
- [web_js] assignment/detail-student.\*.js
- [web_js] assignment/group-between.\*.js
- [web_js] assignment/group-within.\*.js
- [web_js] components/content/reader/task-group-reader.\*.js
- [web_js] components/content/reader/task-reader.\*.js
- [web_js] course/list-lecturer~settings/profile-lecturer.\*.js
- [web_js] course/deleted-lecturer.\*.js
- [web_js] course/edit-learning-plan-lecturer.\*.js
- [web_js] course/edit-lecturer.\*.js
- [web_js] course/edit.\*.js
- [web_js] course/learning-plan.\*.js
- [web_js] course/preview-lecturer.\*.js
- [web_js] course/public.\*.js
- [web_js] course/read-student.\*.js
- [web_js] dashboard/lecturer.\*.js
- [web_js] dashboard/student.\*.js
- [web_js] exam/detail.\*.js
- [web_js] exam/edit.\*.js
- [web_js] exam/list-student-table.\*.js
- [web_js] home/lecturer.\*.js
- [web_js] home/student.\*.js
- [web_js] layouts/main.\*.js
- [web_js] logbook/list-lecturer.\*.js
- [web_js] message/list~message/sent.\*.js
- [web_js] message/detail.\*.js
- [web_js] message/list.\*.js
- [web_js] message/new.\*.js
- [web_js] message/sent.\*.js
- [web_js] settings/profile-lecturer~settings/profile-student.\*.js
- [web_js] settings/account-student.\*.js
- [web_js] settings/profile-lecturer.\*.js
- [web_js] settings/profile-student.\*.js
- [web_js] survey/detail.\*.js
- [web_js] survey/list-lecturer.\*.js
- [web_js] vlab/result-lecturer.\*.js

## user/getUserId

- Returns `state.userInfo.id`

Used by:

- [web_js] app.\*.js
- [web_js] course/edit-lecturer.\*.js
- [web_js] course/preview-lecturer.\*.js
- [web_js] course/read-student.\*.js
- [web_js] discussion/lecturer~discussion/reply-detail.\*.js
- [web_js] workspace/module-detail.\*.js

## user/getAccounts

- Get "auth" from local storage (Let's say it is stored into variable named `auth`)
- Returns `auth.accounts` if defined (not including empty array), otherwise returns empty array

Used by:

- [web_js] layouts/main.\*.js

## user/getUsersList

- Returns `state.list`

Used by:

- [web_js] course/edit-learning-plan-lecturer.\*.js
- [web_js] course/edit-lecturer.\*.js
- [web_js] course/learning-plan-lecturer.\*.js
- [web_js] message/list~message/sent.\*.js
- [web_js] message/detail.\*.js
- [web_js] message/new.\*.js

## user/getRole

- Returns `state.userInfo.role`

Used by:

- [web_js] app.\*.js

  This file exports a class that access this getters. This class is used by:
  - [web_js] agenda/list.\*.js
  - [web_js] announcement/list.\*.js
  - [web_js] assignment/detail-student.\*.js
  - [web_js] assignment/group-between.\*.js
  - [web_js] assignment/group-detail.\*.js
  - [web_js] assignment/group-within.\*.js
  - [web_js] assignment/list.\*.js
  - [web_js] course/deleted.\*.js
  - [web_js] course/edit-lecturer.\*.js
  - [web_js] course/edit.\*.js
  - [web_js] course/enroll.\*.js
  - [web_js] course/learning-plan.\*.js
  - [web_js] course/list.\*.js
  - [web_js] course/preview-lecturer.\*.js
  - [web_js] course/preview.\*.js
  - [web_js] course/public.\*.js
  - [web_js] course/read-student.\*.js
  - [web_js] course/read.\*.js
  - [web_js] course/settings.\*.js
  - [web_js] dashboard/lecturer.\*.js
  - [web_js] discussion/detail.\*.js
  - [web_js] discussion/list.\*.js
  - [web_js] exam/detail.\*.js
  - [web_js] exam/edit.\*.js
  - [web_js] exam/list.\*.js
  - [web_js] home/lecturer.\*.js
  - [web_js] home/student.\*.js
  - [web_js] logbook/detail.\*.js
  - [web_js] logbook/list-lecturer.\*.js
  - [web_js] logbook/list.\*.js
  - [web_js] notification/list.\*.js
  - [web_js] presence/detail.\*.js
  - [web_js] presence/detailstudents.\*.js
  - [web_js] presence/list.\*.js
  - [web_js] settings/account.\*.js
  - [web_js] settings/profile.\*.js
  - [web_js] survey/list-lecturer.\*.js
  - [web_js] survey/list.\*.js
  - [web_js] survey/take.\*.js
  - [web_js] take/list-lecturer.\*.js
  - [web_js] take/list.\*.js
  - [web_js] vlab/result.\*.js
  - [web_js] dashboard.\*.js
  - [web_js] home.\*.js
  - [web_js] note.\*.js
  - [web_js] agenda/vicon.\*.js

## user/isLecturer

- Returns true if `state.userInfo.role` is equals `LECTURER`

Used by:

- [web_js] course/edit-lecturer.\*.js
- [web_js] course/enroll.\*.js
- [web_js] course/preview-lecturer.\*.js
- [web_js] course/public.\*.js
- [web_js] course/read-student.\*.js
- [web_js] home/student.\*.js
- [web_js] workspace/module-detail.\*.js
- [web_js] app.\*.js
- [web_js] vicon.\*.js

## user/isStudent

- Returns true if `state.userInfo.role` is equals `STUDENT`

Used by:

- [web_js] exam/edit.\*.js
