# 프로그램 사용 현황판

## 개요

이 현황판은 회사에서 여러 프로그램의 사용 상태를 간단히 공유하기 위한 단일 HTML 웹페이지입니다.

- 프로그램 1~10개 등록 가능
- 프로그램 목록을 `1. MIDAS GEN`, `2. MIDAS SDS`처럼 번호로 표시
- 프로그램별 `사용시작`, `사용해제` 버튼을 한 줄 좌우 배치로 표시
- 사용 중이면 초록색 사용자명 표시
- 미사용이면 `미사용` 표시
- 프로그램 등록, 이름 변경, 삭제는 관리자 모드에서만 표시
- Firebase Realtime Database를 이용해 여러 PC에서 상태 동기화
- Firebase SDK 설정 없이 Database URL만 사용

## 적용된 Firebase Database URL

```text
https://gpt-dashboard-71602-default-rtdb.firebaseio.com/
```

`index.html` 안에 위 주소가 이미 반영되어 있습니다.

## 관리자 모드

기본 관리자 비밀번호는 다음과 같습니다.

```text
1234
```

배포 전에 `index.html`에서 아래 줄을 찾아 원하는 비밀번호로 바꾸세요.

```javascript
const ADMIN_PASSWORD = "1234";
```

관리자 모드에서만 가능한 기능은 다음과 같습니다.

- 프로그램 등록
- 프로그램명 변경
- 프로그램 삭제

일반 사용자는 프로그램별 `사용시작`, `사용해제`만 사용할 수 있습니다.

> 주의: 이 관리자 모드는 정적 HTML에서 버튼 표시를 제어하는 간단 방식입니다. Firebase Rules가 공개형이면 데이터베이스 자체의 강한 보안은 아닙니다. 외부 공개가 부담되는 경우 Firebase Authentication을 추가해야 합니다.

## Firebase Rules 설정

Firebase Console에서 다음 순서로 들어갑니다.

```text
Firebase Console → Realtime Database → Rules
```

아래 규칙을 붙여넣고 `Publish`를 누르세요.

```json
{
  "rules": {
    "programUsageDashboard": {
      ".read": true,
      ".write": true
    }
  }
}
```

## GitHub Pages 배포 방법

1. GitHub에서 새 Repository 생성
2. `index.html` 업로드
3. Repository의 `Settings` 이동
4. `Pages` 메뉴 이동
5. Branch를 `main`, folder를 `/root`로 선택
6. Save
7. 표시되는 GitHub Pages 주소로 접속

## 사용 방법

### 관리자

1. `관리자 로그인` 클릭
2. 관리자 비밀번호 입력
3. 프로그램명 입력 후 `프로그램 등록`
4. 필요한 경우 각 프로그램의 `이름변경`, `삭제` 사용
5. 관리가 끝나면 `관리자 해제` 클릭

### 일반 사용자

1. 사용할 프로그램 옆의 `사용시작` 클릭
2. 사용자명 입력
3. 사용이 끝나면 `사용해제` 클릭

## 데이터 저장 위치

Firebase Realtime Database에는 다음 경로로 저장됩니다.

```text
/programUsageDashboard/programs
```

각 프로그램은 다음 형태로 저장됩니다.

```json
{
  "name": "MIDAS GEN",
  "inUse": true,
  "userName": "홍길동 과장",
  "createdAt": 1790000000000,
  "updatedAt": 1790000000000
}
```
