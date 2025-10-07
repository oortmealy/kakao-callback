# KangarooTine 카카오 로그인 콜백 페이지

이 폴더는 GitHub Pages를 통해 호스팅되어 카카오 로그인의 콜백 처리를 담당합니다.

## 🚀 GitHub Pages 설정 방법

### 1. GitHub 레포지토리 생성
1. GitHub에 새 레포지토리 생성: `kakao-callback`
2. **Public**으로 설정 (GitHub Pages 무료 사용을 위해)

### 2. 파일 업로드
이 폴더의 모든 파일을 GitHub 레포지토리에 업로드:
```
kakao-callback/
├── index.html    # 카카오 로그인 콜백 처리 페이지
└── README.md     # 이 파일
```

### 3. GitHub Pages 활성화
1. 레포지토리 → **Settings** → **Pages**
2. **Source**: Deploy from a branch
3. **Branch**: main (또는 master)
4. **Folder**: / (root)
5. **Save** 클릭

### 4. 도메인 확인
GitHub Pages 활성화 후 다음과 같은 URL이 생성됩니다:
```
https://YOUR_GITHUB_USERNAME.github.io/kakao-callback/
```

## ⚙️ 설정 방법

### 1. 카카오 클라이언트 ID 설정
`index.html` 파일의 19번째 줄 수정:
```javascript
// 실제 카카오 클라이언트 ID로 변경
const KAKAO_CLIENT_ID = 'YOUR_KAKAO_CLIENT_ID';
```

### 2. 앱의 AuthService 업데이트
앱의 `services/auth/authService.ts` 파일에서 redirectUri 수정:
```typescript
static getKakaoWebViewConfig() {
  return {
    clientId: process.env.EXPO_PUBLIC_KAKAO_CLIENT_ID || 'YOUR_KAKAO_CLIENT_ID',
    redirectUri: 'https://YOUR_GITHUB_USERNAME.github.io/kakao-callback/', // 실제 GitHub Pages URL로 변경
  };
}
```

### 3. 카카오 디벨로퍼스 설정
1. [카카오 디벨로퍼스](https://developers.kakao.com/) 접속
2. 애플리케이션 선택
3. **플랫폼** → **Web 플랫폼 등록**
   - 사이트 도메인: `https://YOUR_GITHUB_USERNAME.github.io`
4. **카카오 로그인** → **Redirect URI**
   - URI: `https://YOUR_GITHUB_USERNAME.github.io/kakao-callback/`

## 🔧 로컬 테스트

로컬에서 테스트하려면:
```bash
# 간단한 HTTP 서버 실행
cd kakao-callback
python -m http.server 8000

# 브라우저에서 접속
# http://localhost:8000
```

## 📱 동작 원리

1. **앱에서 카카오 로그인 시작** → 웹뷰에서 카카오 로그인 페이지 열림
2. **사용자 로그인 완료** → 카카오가 이 콜백 페이지로 리다이렉트
3. **콜백 페이지에서 코드 추출** → 카카오 API를 통해 액세스 토큰 교환
4. **앱으로 토큰 전달** → React Native WebView를 통해 앱에 결과 전송
5. **앱에서 백엔드 로그인** → 받은 토큰으로 백엔드 `/auth/login` 호출

## 🛠️ 문제 해결

### GitHub Pages가 404 에러를 보여줄 때
- 레포지토리가 Public인지 확인
- 파일 이름이 `index.html`인지 확인
- GitHub Pages 설정에서 branch가 올바른지 확인

### 카카오 로그인이 실패할 때
- 카카오 디벨로퍼스의 Redirect URI가 정확한지 확인
- `index.html`의 클라이언트 ID가 올바른지 확인
- 브라우저 개발자 도구에서 콘솔 로그 확인

### CORS 에러가 발생할 때
- GitHub Pages는 HTTPS를 사용하므로 CORS 문제가 거의 없음
- 카카오 API는 CORS를 허용하므로 정상 동작해야 함

## 📄 라이센스
이 프로젝트는 KangarooTine 서비스의 일부입니다.