# 2025-03-28~

### [ Spring 숙련 2주차 강의]

1. Cookie
2. Session
3. Token
4. JWT
5. Filter
6. Servlet Filter

<hr>

<details>

<summary style="font-size: 16px;">
<strong>1. Cookie</strong>
</summary>

**1. 개념**
- 사용자의 웹 브라우저에 저장되는 작은 데이터 조각
- 클라이언트와 서버 간의 상태 정보를 유지하는 데 사용됨

**2. 특징**
- 키-값 쌍으로 저장됨 (name=value)
- 만료 기간 설정 가능 (Max-Age, Expires)
- 브라우저가 자동으로 요청 시 쿠키 포함 (HttpOnly, Secure 설정 가능)

**3. 쿠키 종류**
- 세션 쿠키: 브라우저 종료 시 삭제됨
- 영속 쿠키: 설정된 만료 시간까지 유지됨

**4. 쿠키 생성**

  ```java
  Cookie cookie = new Cookie("token", "abcd1234");
  cookie.setHttpOnly(true);
  cookie.setMaxAge(60 * 60);
  response.addCookie(cookie);
  ```

  ```java
  @GetMapping("/read-cookie")
  public String readCookie(@CookieValue("token") String token) {
      return "Token: " + token;
  }
  ```

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>2. Session</strong>
</summary>

**1. 개념**
- 사용자의 상태를 서버에서 관리하는 방식
- 클라이언트가 요청 시 세션 ID를 포함하여 서버에 전송
- 서버가 세션 ID를 기반으로 사용자 정보 조회

**2. 특징**
- 서버에서 사용자 정보를 저장하므로 보안이 뛰어남
- 서버 메모리를 사용하므로 많은 사용자 처리 시 부담이 큼
- 기본적으로 쿠키(JSESSIONID)를 사용하여 세션을 유지함

**3. 세션 활용( 저장, 조회 )**

  ```java
  @PostMapping("/login")
  public String login(HttpSession session) {
      session.setAttribute("user", "admin");
      return "Session Created";
  }
  ```

  ```java
  @GetMapping("/user")
  public String getSessionUser(HttpSession session) {
      return "User: " + session.getAttribute("user");
  }
```

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>3. Token</strong>
</summary>

**1. 개념**
- 서버에서 발급하는 인증 정보 조각
- 세션을 사용하지 않고 클라이언트가 자체적으로 보관 및 전송
- Stateless(상태 유지 없이 요청마다 독립적으로 처리 가능)

**2. 특징**
- 자체적으로 서명(Signature)이 포함될 수 있음 (ex. JWT)
- 서버 확장성이 뛰어남 (세션 저장 부담 없음)
- 보안 위험 존재 (토큰 탈취 시 인증 가능)

**3. 토큰 사용 예시**
- JWT, OAuth 2.0 Access Token, API Key 등

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>4. JWT</strong>
</summary>

**1. 개념**
- JSON 형식으로 사용자 정보를 포함한 토큰
- 자체적으로 서명을 포함하여 위변조 방지 기능 제공

**2. 구조**
- Header: 타입(JWT), 알고리즘 정보
- Payload: 사용자 정보 (sub, exp, role 등)
- Signature: 서명 (무결성 검증)

**3. 장점**
- 세션 없이 인증 가능 (Stateless)
- 확장성이 뛰어남 (분산 서버에서 사용 가능)

**4. 단점**
- 보안 위험 (탈취 시 유효 기간 내 사용 가능)
- Payload에 정보 포함 (기밀 정보 저장 금지)

</details>

<hr>

<details>

<summary style="font-size: 16px;">
<strong>5. Filter</strong>
</summary>

**1. 개념**
- HTTP 요청/응답을 가로채서 처리하는 Spring Security의 핵심 개념
- 요청을 변경하거나 인증/인가 처리 가능

**2. Spring Security에서 필터 동작 방식**
- 요청이 들어오면 필터 체인을 통과
- 각 필터가 인증/인가, 로깅 등을 수행
- 최종적으로 컨트롤러로 요청 전달

**3. 주요 필터 종류**
- `UsernamePasswordAuthenticationFilter` : 로그인 처리
- `BasicAuthenticationFilter` : Basic 인증 처리
- `JwtAuthenticationFilter` : JWT 인증 처리 (커스텀 필터)

</details>

<hr>


<details>

<summary style="font-size: 16px;">
<strong>6. Servlet Filter</strong>
</summary>

**1. 개념**
- 서블릿 요청을 가로채어 사전/사후 작업을 수행하는 필터
- 인증, 로깅, 압축 등의 기능을 수행 가능

**2. 동작 방식**
   1. 클라이언트가 요청을 보냄
   2. Filter가 요청을 가로채 처리
   3. 서블릿(Controller)에서 요청을 처리
   4. 응답을 다시 Filter에서 가공 후 클라이언트에게 전달

| 구분       | Servlet Filter       | Spring Security Filter |
|------------|----------------------|------------------------|
| 적용 범위  | 전체 서블릿 요청     | Security 관련 요청    |
| 관리 방식  | 웹 애플리케이션 레벨 | Spring Security 관리  |
| 설정 방법  | `@WebFilter`, `FilterRegistrationBean` | `SecurityFilterChain` |

</details>

<details>

<summary style="font-size: 16px;">
<strong>핵심요약</strong>
</summary>

1. Cookie
    - 웹 브라우저에 저장되는 데이터
    - 서버가 클라이언트의 상태를 기억하도록 도와준다.
        - 로그인 상태 유지 등에 활용된다.
    - 보안에 취약하다.
        - 민감한 정보를 저장하지 않아야한다.
        - 사용자 임의 수정이 가능하다.
2. Session
    - 서버에서 중요한 정보를 보관하며 로그인을 유지하는 방법
        - SessionId를 탈취하여도 민감한 정보가 없다.
    - 만료 시간을 설정해서 탈취 문제를 최소화한다.
        - HttpSession은 최근 Session을 요청한 시간을 기준으로 만료 시간을 유지한다.
3. Token
    - 인증/인가 과정에서 사용되며 사용자 또는 시스템의 신원과 권한을 증명하고 요청의 유효성을 검증하는 데 사용되는 디지털 문자열
    - Session과는 다르게 Client가 데이터(Token)를 저장하고 있다.
        - Stateless를 기반으로 하여 확장성이 뛰어나다.
    - Mobile과 같이 Cookie를 사용할 수 없는 경우에도 사용할 수 있다.
    - Payload는 암호화되지 않는다.
    - 만료 시간으로 Token 탈취를 대비한다.
4. JWT
    - 인증에 필요한 정보들을 암호화시킨 JSON 형태의 Token
    - Signature를 통해 Token을 안전하게 관리한다.
    - JWT의 목적은 정보 보호가 아닌, 위조 방지에 있다.
5. Filter
    - 공통 관심 사항을 하나의 입구에서 처리할 수 있게 만들어준다.

</details>