---
title: axios 모듈화 하기
parent: 프론트엔드
layout: default
---

# axios 를 모듈화 해서 재사용하는 과정


* **axios 설치**

```shell
npm install axios
```

* **현재 파일 구조**

```shell
❯ tree -L 11 .
.
├── favicon.ico
├── fonts
│   ├── GeistMonoVF.woff
│   └── GeistVF.woff
├── globals.css
├── layout.tsx
├── page.module.css
├── page.tsx
├── public
│   ├── file.svg
│   ├── globe.svg
│   ├── next.svg
│   ├── vercel.svg
│   └── window.svg
└── signin
    ├── (route)
    ├── _components
    │   └── user-auth-form.tsx
    ├── confirmemail
    │   ├── layout.tsx
    │   └── page.tsx
    ├── layout.tsx
    └── page.tsx
```

* `utils/api/instance.ts` 파일 추가 (인스턴스 모듈화)

```typescript
const axiosInstance = axios.create({
    baseURL: process.env.NEXT_PUBLIC_API_URL,
    timeout: 5000, // 요청 타임아웃 설정
    headers: {
        'Content-type': 'application/json',
    },
    withCredentials: true // 크로스 도메인 요청 시에도 인증 쿠키를 전송하기 위한 설정
});


export default axiosInstance;
```

* api 모듈화

```typescript
// api/userService.ts

const userService = {
    signup: (data: UserSignupRequest) => {
        return axiosInstance.post<ApiResponse<any>>('/users', data);
    }
}


export default userService;
```

* 공통 응답 코드 및 요청 인자 타입 설정

```typescript
// types/api.d.ts
export interface ApiResponse<T = any> {
    code: number;        // 응답 코드 (예: 200, 400, 401 등)
    message: string;     // 응답 메시지
    data: T;             // 실제 데이터
}


export interface UserSignupRequest {
    email: string;
}
```
