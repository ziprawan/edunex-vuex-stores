# User Store Definitions

## namespaced

true

## state

```typescript
interface IUserState {
  indicators: {
    loggingIn: boolean;
    loginState: "neutral" | "failed" | "succeed" | string;
    loginFailedReason: null | unknown;
    loggingOut: boolean;
    logoutState: "neutral" | "failed" | "succeed" | string;
    logoutFailedReason: null | unknown;
    userInfo: {
      status: "neutral" | "failed" | "succeed" | string;
      fetching: boolean;
      reason: null | unknown;
    };
  };
  badges: Array<{ type: "badge-king" | string }>;
  loginInfo: {
    accessToken: string;
    refreshToken: string;
    expirationDate: null | unknown;
    verified: boolean;
  };
  userInfo: {
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
  };
  user: {};
  list: [];
  pagination: {
    limit: number;
    offset: number;
  };
}

let state: IUserState = {
  indicators: {
    loggingIn: false,
    loginState: "neutral",
    loginFailedReason: null,
    loggingOut: false,
    logoutState: "neutral",
    logoutFailedReason: null,
    userInfo: { status: "neutral", fetching: false, reason: null },
  },
  badges: [{ type: "badge-king" }],
  loginInfo: {
    accessToken: "",
    refreshToken: "",
    expirationDate: null,
    verified: false,
  },
  userInfo: {},
  user: {},
  list: [],
  pagination: { limit: 10, offset: 0 },
};
```
