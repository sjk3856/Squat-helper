🏋️‍♂️ AI Smart Squat Master (AI 스마트 스쿼트 마스터)MediaPipe Pose 기반 실시간 AI 모션 비전 & 맞춤형 스쿼트 피드백 시스템사용자의 키와 몸무게를 기반으로 최적의 스쿼트 스탠스를 계산하고, 웹캠을 통해 실시간 가동 범위(ROM)와 깊이(Depth) 가이드라인을 투영하여 완벽한 스쿼트 루틴을 보조합니다.📌 목차주요 기능기술 스택프로젝트 구조로컬 실행 가이드안드로이드 모바일 빌드 및 PWA 배포자세 판정 및 알고리즘 상세브라우저 및 하드웨어 권장 환경✨ 주요 기능1. 📏 신체 스펙 맞춤형 스탠스 진단 엔진키 & 몸무게 기반 비율 추정: 신장 대비 대퇴골(허벅지뼈) 길이 비중 및 지렛대 모멘트 암을 계산.맞춤 권장치 도출:180cm 이상 (대퇴골 긴 체형): 1.3배 와이드 스탠스, 25°~30° 외회전, 목표 82° 패러럴 권장 (요추 과부하 완화).165cm 이하 (컴팩트 체형): 1.0배 표준 스탠스, 10°~15° 외회전, 목표 75° 딥 스쿼트 권장.166cm ~ 179cm (표준 체형): 1.1배 스탠스, 15°~20° 외회전, 목표 80° 권장.2. 🎯 화면 실시간 높이 가이드라인 & ROM 게이지Canvas Overlay 목표 높이선: 무릎 관절의 수평면을 기준으로 허벅지가 지면과 수평이 되는 최적 타겟 높이를 점선으로 투영.실시간 ROM(Range of Motion) 프로그레스 바: 현재 무릎 각도(160° ~ 60°)에 따라 실시간 하강률을 동적 시각화.3. 🛡️ 상체 단독 노출 오인식 방지 (하체 미검출 방어막)골반(Hip, 23/24), 무릎(Knee, 25/26), 발목(Ankle, 27/28) 관절의 가시성 점수(visibility > 0.65)를 필수 검증.노트북을 들고 있거나 상체만 비추어질 경우 경고 레이어를 표시하고 카운팅을 원천 차단.고관절 굴곡 각도(Shoulder-Hip-Knee)와 무릎 각도가 동시에 충족될 때만 스쿼트 1회로 판정.4. 💯 0~100점 실시간 동적 채점 & 다감각 피드백감점 알고리즘:상체 과숙임 / 엉덩이만 뒤로 빠지는 굿모닝 스쿼트 (-15~35점)무릎이 발끝보다 과도하게 쏠리는 현상 (-20점)목표 깊이 미달 (-15점) 및 과도한 딥/벗윙크 위험 (-10점)Web Speech API (TTS): 매 횟수 달성 시 점수 구간별 맞춤형 한국어 음성 브리핑 제공.Haptic Vibration: 모바일 환경에서 하강 안착 시 1회, 반복 완료 시 2회 진동 피드백 지원.🛠 기술 스택분류기술 / 라이브러리FrontendHTML5, JavaScript (ES6+), Tailwind CSS (CDN)AI Vision EngineGoogle MediaPipe Pose (@mediapipe/pose, @mediapipe/camera_utils)RenderingHTML5 Canvas 2D Context (Skeleton & Guide Overlay)Feedback APIsWeb Speech API (SpeechSynthesis), Web Vibration APIMobile PackagingCapacitor (Android Native WebView) or Progressive Web App (PWA)📁 프로젝트 구조├── index.html                # 데스크톱/태블릿용 와이드 대시보드 인터페이스
├── mobile_squat_coach.html   # 모바일 뷰포트 최적화 & 카메라 전환/햅틱 지원 에디션
└── README.md                 # 프로젝트 문서
🚀 로컬 실행 가이드본 프로젝트는 별도의 백엔드 설치나 빌드 과정 없이 정적 파일만으로 실행 가능합니다. 단, 웹캠 접근 권한을 브라우저에서 허용해야 하므로 로컬 서버(http://localhost) 환경에서 구동해야 합니다.방법 1. VS Code Live Server 확장 프로그램VS Code에서 프로젝트 폴더를 엽니다.index.html 또는 mobile_squat_coach.html 우클릭 후 [Open with Live Server] 클릭.방법 2. Python 기본 웹서버 구동# Python 3.x 환경
python -m http.server 8000
브라우저 주소창에 http://localhost:8000 입력 후 접속.방법 3. Node.js (npx serve)npx serve .
📱 안드로이드 모바일 빌드 및 PWA 배포1. PWA (가장 빠른 웹앱 설치 방식)GitHub Pages, Vercel, Netlify 등의 HTTPS 호스팅 환경에 소스코드를 업로드합니다.안드로이드 기기의 크롬(Chrome) 또는 삼성 인터넷으로 해당 URL에 접속합니다.브라우저 옵션 메뉴(⋮)에서 [홈 화면에 추가] 또는 [앱 설치]를 탭합니다.독립 실행형(Standalone) 전체화면 앱으로 구동됩니다.2. Capacitor를 통한 정식 안드로이드 APK 빌드웹 코드를 네이티브 APK 설치 파일로 패키징할 수 있습니다.# 1. 새 디렉터리 생성 및 초기화
mkdir squat-app && cd squat-app
npm init -y

# 2. Capacitor 패키지 설치
npm install @capacitor/core @capacitor/cli @capacitor/android

# 3. Capacitor 프로젝트 설정
npx cap init "AI Squat Coach" "com.fitness.squatcoach" --web-dir "."
npx cap add android

# 4. mobile_squat_coach.html 파일을 index.html로 이름 변경하여 루트에 배치 후 동기화
cp ../mobile_squat_coach.html ./index.html
npx cap copy android

# 5. 안드로이드 스튜디오 실행
npx cap open android
Android Studio 오픈 후 AndroidManifest.xml에 카메라 권한이 정상 선언되었는지 확인합니다:<uses-permission android:name="android.permission.CAMERA" />
상단 메뉴의 [Build] → [Build Bundle(s) / APK(s)] → [Build APK(s)]를 실행하여 .apk 파일을 생성합니다.📐 자세 판정 및 알고리즘 상세1. 각도 계산 공식 (Three-Point Angle)세 개의 2차원 관절 좌표 $A(x_1, y_1)$, $B(x_2, y_2)$, $C(x_3, y_3)$에 대해 중심점 $B$의 각도 $\theta$는 다음과 같이 도출됩니다:$$\theta = \vert{}\text{atan2}(y_3 - y_2, x_3 - x_2) - \text{atan2}(y_1 - y_2, x_1 - x_2)\vert{} \times \frac{180}{\pi}$$$$\text{If } \theta > 180^\circ \implies \theta = 360^\circ - \theta$$2. 상태 머신 (FSM) 전이 로직UP (대기 상태):전신 가시성 확인 (Hip, Knee, Ankle Visibility $\ge 0.65$)무릎 각도 $\ge 155^\circ$DOWN (하강 도달):무릎 각도 $\le (\text{TargetAngle} + 15^\circ)$ AND 고관절 굴곡 각도 $\le 110^\circ$하강 궤적 중 최저 점수(currentRepLowestScore)를 지속 트래킹반복 완료 (Repetition Count):무릎 각도 $\ge 155^\circ$ 복귀 AND 고관절 각도 $\ge 140^\circ$카운트 $+1$, 점수 등급화, 음성 브리핑 및 진동 발생💻 브라우저 및 하드웨어 권장 환경카메라 위치: 사용자 측면 $45^\circ \sim 90^\circ$ 각도, 머리부터 발목까지 프레임 안에 완전히 담기도록 $2.5\text{m} \sim 3\text{m}$ 거리를 권장합니다.권장 브라우저: Google Chrome, Microsoft Edge, Safari (iOS 15.0+), Samsung Internet.성능 사양: WebGL 가속을 지원하는 GPU 내장 기기 권장 (MediaPipe Pose WebGL 가속 사용).📄 라이선스This project is open-sourced under the MIT License.
