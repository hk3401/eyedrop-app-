# 안약체크 (Android 앱)

안약 점안 스케줄 관리 + 알람(진동·소리·무음) + 달력 체크 안드로이드 앱입니다.
웹앱을 **Capacitor**로 감싸서, 안드로이드 네이티브 알림(AlarmManager)을 사용합니다.
→ **앱을 꺼둬도 점안 시간에 알람이 울립니다.**

- 앱 이름: 안약체크
- 패키지(appId): `kr.co.hdps.eyedrop`
- 기록은 폰 내부에만 저장 (서버 전송 없음)

---

## 빌드 방법 A — GitHub 자동 빌드 (PC 설치 불필요, 추천)

PC에 개발 도구를 깔 필요 없이, GitHub가 클라우드에서 APK를 만들어 줍니다.

1. github.com 에 로그인 → **New repository** 로 새 저장소 생성 (Private 가능).
2. 이 폴더의 **모든 파일**을 그 저장소에 올립니다.
   (`node_modules` 폴더는 올리지 않아도 됩니다 — `.gitignore`에 이미 제외돼 있음.)
   - 웹 업로드: 저장소 페이지 → **Add file → Upload files** 로 폴더째 끌어다 놓기.
   - 또는 git 사용:
     ```
     git init && git add . && git commit -m "init"
     git branch -M main
     git remote add origin https://github.com/<아이디>/<저장소>.git
     git push -u origin main
     ```
3. 올리면 자동으로 빌드가 시작됩니다. 저장소 상단 **Actions** 탭으로 이동.
4. 가장 최근 실행(초록 체크 ✓)을 클릭 → 아래 **Artifacts** 의 **eyedrop-apk** 다운로드.
5. 받은 zip을 풀면 `app-debug.apk` 가 있습니다. 이 파일을 휴대폰으로 옮겨 설치하세요.
   - 설치 시 "출처를 알 수 없는 앱 허용"을 한 번 켜야 합니다.

> 빌드는 보통 3~6분 걸립니다. 수동으로 다시 돌리려면 Actions → Build Android APK → **Run workflow**.

---

## 빌드 방법 B — 내 PC에서 직접 (Android Studio)

직접 빌드하고 싶을 때만 사용하세요.

준비물: Node.js 20+, Android Studio(안드로이드 SDK 포함)

```
npm install
npx cap add android

# 알림 아이콘 + 무음(진동전용) 사운드 복사
mkdir -p android/app/src/main/res/raw android/app/src/main/res/drawable
cp android-res/raw/silence.wav   android/app/src/main/res/raw/silence.wav
cp android-res/drawable/ic_stat_drop.png android/app/src/main/res/drawable/ic_stat_drop.png

npx cap sync android
npx cap open android
```

Android Studio가 열리면 상단의 **Run ▶** (연결된 폰/에뮬레이터) 또는
**Build → Build App Bundle(s)/APK(s) → Build APK(s)** 로 APK를 만듭니다.

---

## 설치 후 꼭 확인할 것 (알람이 잘 울리게)

1. **알림 허용** — 첫 실행 때 뜨는 권한 요청을 허용하세요 (Android 13 이상 필수).
2. **정확한 알람 권한** — 일부 기기는 설정 > 앱 > 안약체크 > **알람 및 리마인더**를 켜야 합니다.
3. **배터리 절전 제외** — 제조사 절전 기능이 알람을 막을 수 있습니다.
   설정 > 배터리 > 안약체크 > **제한 없음 / 백그라운드 허용** 으로 두세요.
   (특히 삼성·샤오미 기기는 권장)
4. 앱 안 **설정 탭**에서 진동 / 소리 / 무음을 고르면 알림 방식이 바뀝니다.

---

## 앱 사용법

- **오늘** — 시간대별 점안 목록, ＋ 버튼으로 점안 완료 체크 (넣은 시각 기록).
- **달력** — 날짜별 점안 상태 한눈에, 빠뜨린 날 눌러서 보충 체크.
- **안약** — 약 추가/편집/삭제, 약별 알람 켜기/끄기. 하루 횟수와 시간 또는 간격 설정.
- **설정** — 알람 켜기, 진동/소리/무음, 미점안 시 재알림, 백업/초기화.

기본으로 타프로탄(1회)·코솝(2회)·알파간(3회)이 등록돼 있습니다. 실제 시간에 맞게 수정하세요.

---

## 참고

- 이 APK는 디버그(자체 서명) 빌드로 개인 사용에 충분합니다.
  플레이스토어 배포는 별도 서명 키가 필요합니다(이 프로젝트 범위 밖).
- 앱 이름·패키지명을 바꾸려면 `capacitor.config.json` 의 `appName`, `appId` 를 수정하세요.
- 정확한 점안 순서·간격·횟수는 담당 안과 처방을 우선하세요.
