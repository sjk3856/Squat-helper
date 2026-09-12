[README.md](https://github.com/user-attachments/files/32141184/README.md)
# **🏋️‍♂️ AI Smart Squat Master (AI 스마트 스쿼트 마스터)**

> **MediaPipe Pose 기반 실시간 AI 모션 비전 & 맞춤형 스쿼트 피드백 시스템**

> 사용자의 키와 몸무게를 기반으로 최적의 스쿼트 스탠스를 계산하고, 웹캠을 통해 실시간 가동 범위(ROM)와 깊이(Depth) 가이드라인을 투영하여 완벽한 스쿼트 루틴을 보조합니다.

## **📌 목차**

1. [주요 기능](#bookmark=id.7ez0vvc0e9gl)  
2. [기술 스택](#bookmark=id.vckreb5a0hpr)  
3. [프로젝트 구조](#bookmark=id.b6x0q0wdgbdl)  
4. [로컬 실행 가이드](#bookmark=id.gebb4athevgf)  
5. [안드로이드 모바일 빌드 및 PWA 배포](#bookmark=id.1xqx4fo3w9m0)  
6. [자세 판정 및 알고리즘 상세](#bookmark=id.2o63c5geldin)  
7. [브라우저 및 하드웨어 권장 환경](#bookmark=id.uof2hncwlops)

## **✨ 주요 기능**

### **1\. 📏 신체 스펙 맞춤형 스탠스 진단 엔진**

* **키 & 몸무게 기반 비율 추정**: 신장 대비 대퇴골(허벅지뼈) 길이 비중 및 지렛대 모멘트 암을 계산.  
* **맞춤 권장치 도출**:  
  * **180cm 이상 (대퇴골 긴 체형)**: 1.3배 와이드 스탠스, 25°\~30° 외회전, 목표 82° 패러럴 권장 (요추 과부하 완화).  
  * **165cm 이하 (컴팩트 체형)**: 1.0배 표준 스탠스, 10°\~15° 외회전, 목표 75° 딥 스쿼트 권장.  
  * **166cm \~ 179cm (표준 체형)**: 1.1배 스탠스, 15°\~20° 외회전, 목표 80° 권장.

### **2\. 🎯 화면 실시간 높이 가이드라인 & ROM 게이지**

* **Canvas Overlay 목표 높이선**: 무릎 관절의 수평면을 기준으로 허벅지가 지면과 수평이 되는 최적 타겟 높이를 점선으로 투영.  
* **실시간 ROM(Range of Motion) 프로그레스 바**: 현재 무릎 각도(160° \~ 60°)에 따라 실시간 하강률을 동적 시각화.

### **3\. 🛡️ 상체 단독 노출 오인식 방지 (하체 미검출 방어막)**

* 골반(Hip, 23/24), 무릎(Knee, 25/26), 발목(Ankle, 27/28) 관절의 가시성 점수(visibility \> 0.65)를 필수 검증.  
* 노트북을 들고 있거나 상체만 비추어질 경우 경고 레이어를 표시하고 카운팅을 원천 차단.  
* 고관절 굴곡 각도(Shoulder-Hip-Knee)와 무릎 각도가 동시에 충족될 때만 스쿼트 1회로 판정.

### **4\. 💯 0\~100점 실시간 동적 채점 & 다감각 피드백**

* **감점 알고리즘**:  
  * 상체 과숙임 / 엉덩이만 뒤로 빠지는 굿모닝 스쿼트 (-15\~35점)  
  * 무릎이 발끝보다 과도하게 쏠리는 현상 (-20점)  
  * 목표 깊이 미달 (-15점) 및 과도한 딥/벗윙크 위험 (-10점)  
* **Web Speech API (TTS)**: 매 횟수 달성 시 점수 구간별 맞춤형 한국어 음성 브리핑 제공.  
* **Haptic Vibration**: 모바일 환경에서 하강 안착 시 1회, 반복 완료 시 2회 진동 피드백 지원.

## **🛠 기술 스택**

| 분류 | 기술 / 라이브러리 |
| :---- | :---- |
| **Frontend** | HTML5, JavaScript (ES6+), Tailwind CSS (CDN) |
| **AI Vision Engine** | Google MediaPipe Pose (@mediapipe/pose, @mediapipe/camera\_utils) |
| **Rendering** | HTML5 Canvas 2D Context (Skeleton & Guide Overlay) |
| **Feedback APIs** | Web Speech API (SpeechSynthesis), Web Vibration API |
| **Mobile Packaging** | Capacitor (Android Native WebView) or Progressive Web App (PWA) |

## **📁 프로젝트 구조**

├── index.html                \# 데스크톱/태블릿용 와이드 대시보드 인터페이스  
├── mobile\_squat\_coach.html   \# 모바일 뷰포트 최적화 & 카메라 전환/햅틱 지원 에디션  
└── README.md                 \# 프로젝트 문서

## **🚀 로컬 실행 가이드**

본 프로젝트는 별도의 백엔드 설치나 빌드 과정 없이 정적 파일만으로 실행 가능합니다. 단, **웹캠 접근 권한**을 브라우저에서 허용해야 하므로 로컬 서버(http://localhost) 환경에서 구동해야 합니다.

### **방법 1\. VS Code Live Server 확장 프로그램**

1. VS Code에서 프로젝트 폴더를 엽니다.  
2. index.html 또는 mobile\_squat\_coach.html 우클릭 후 **\[Open with Live Server\]** 클릭.

### **방법 2\. Python 기본 웹서버 구동**

\# Python 3.x 환경  
python \-m http.server 8000

브라우저 주소창에 http://localhost:8000 입력 후 접속.

### **방법 3\. Node.js (npx serve)**

npx serve .

## **📱 안드로이드 모바일 빌드 및 PWA 배포**

### **1\. PWA (가장 빠른 웹앱 설치 방식)**

1. GitHub Pages, Vercel, Netlify 등의 HTTPS 호스팅 환경에 소스코드를 업로드합니다.  
2. 안드로이드 기기의 **크롬(Chrome)** 또는 **삼성 인터넷**으로 해당 URL에 접속합니다.  
3. 브라우저 옵션 메뉴(⋮)에서 **\[홈 화면에 추가\]** 또는 \[앱 설치\]를 탭합니다.  
4. 독립 실행형(Standalone) 전체화면 앱으로 구동됩니다.

### **2\. Capacitor를 통한 정식 안드로이드 APK 빌드**

웹 코드를 네이티브 APK 설치 파일로 패키징할 수 있습니다.

\# 1\. 새 디렉터리 생성 및 초기화  
mkdir squat-app && cd squat-app  
npm init \-y

\# 2\. Capacitor 패키지 설치  
npm install @capacitor/core @capacitor/cli @capacitor/android

\# 3\. Capacitor 프로젝트 설정  
npx cap init "AI Squat Coach" "com.fitness.squatcoach" \--web-dir "."  
npx cap add android

\# 4\. mobile\_squat\_coach.html 파일을 index.html로 이름 변경하여 루트에 배치 후 동기화  
cp ../mobile\_squat\_coach.html ./index.html  
npx cap copy android

\# 5\. 안드로이드 스튜디오 실행  
npx cap open android

* Android Studio 오픈 후 AndroidManifest.xml에 카메라 권한이 정상 선언되었는지 확인합니다:  
  \<uses-permission android:name="android.permission.CAMERA" /\>

* 상단 메뉴의 \[Build\] → \[Build Bundle(s) / APK(s)\] → \[Build APK(s)\]를 실행하여 .apk 파일을 생성합니다.

## **📐 자세 판정 및 알고리즘 상세**

### **1\. 각도 계산 공식 (Three-Point Angle)**

세 개의 2차원 관절 좌표 ![][image1], ![][image2], $C(x\_3, y\_3)$에 대해 중심점 ![][image3]의 각도 ![][image4]는 다음과 같이 도출됩니다:

### **![][image5]![][image6]2\. 상태 머신 (FSM) 전이 로직**

* **UP (대기 상태)**:  
  * 전신 가시성 확인 (Hip, Knee, Ankle Visibility ![][image7])  
  * 무릎 각도 ![][image8]  
* **DOWN (하강 도달)**:  
  * 무릎 각도 ![][image9] AND 고관절 굴곡 각도 ![][image10]  
  * 하강 궤적 중 최저 점수(currentRepLowestScore)를 지속 트래킹  
* **반복 완료 (Repetition Count)**:  
  * 무릎 각도 ![][image8] 복귀 AND 고관절 각도 ![][image11]  
  * 카운트 ![][image12], 점수 등급화, 음성 브리핑 및 진동 발생

## **💻 브라우저 및 하드웨어 권장 환경**

* **카메라 위치**: 사용자 측면 ![][image13] 각도, 머리부터 발목까지 프레임 안에 완전히 담기도록 ![][image14] 거리를 권장합니다.  
* **권장 브라우저**: Google Chrome, Microsoft Edge, Safari (iOS 15.0+), Samsung Internet.  
* **성능 사양**: WebGL 가속을 지원하는 GPU 내장 기기 권장 (MediaPipe Pose WebGL 가속 사용).

## **📄 라이선스**

This project is open-sourced under the [MIT License](http://docs.google.com/LICENSE).

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAE8AAAAaCAYAAAD2dwHCAAAEiElEQVR4Xu2YXYhVVRTHz8VJCgvtYxycj7vvnYkGRyhlKggsQlCYhzIKeknsQcgehECRQnsQJAjUApuioJCUmBIjISsfpASDIl8SjIIQMQTpoYIgyUTr95+992XPmnPPvXc+kgv3D4t9zlp7r73WOmuts8/Jsg466KCDDmaE0dHRmwYHBxdbfjugu7v71kqlcrPlzxrOuWXQDpQvsbIIBQ35O9Vq9UErawdgNy66IxqtbDYooXQ3dLGvr6/fCgO6kL9J8J63gnbCwMDAw+Vy+eicVQ/KRgnMXyJdW7kAfy3yk3O26Y2DEmUcf160gpbR399/C8oOoewXxquMq/PmwD+OfIuVtSPw4yHoR6hqZS0BBU9CeyjHlxj/JUiP2TkhM3+GlltZOwJ/bseX09AzVtY0hoaGlqLgY8YBpbGCl6eQwG6Gf2p4ePg2KxPok3cq6JT0PdyWenp6FtGU15Gxd9u5s4H2wY4x2ZvyZ3ICQM+70AdclqysKbB4O05v0LWcD5k3rRcQvPdFli/Af4J1Ewo69DX0Fjo+ZNwCnUf+qF0zE9Do70ffJ9AO6BwPZzjKuN8HfdtKAEOy1E2IQrB4BYsP6ewT7ieDB72SzpMc3knLF4KOcT35cC+DrjjfU/ZLH04/bte1itBz31YmSx96L2sPyZISbCmLgr8XoGVWVgg5K6f12o48lK2Gd9VmWAxeXkbCezrV4XwpnJZDKmF0PSvHkyULkI/BX5nwGkJlyrqdwW5leS3LuF4O/abWksxfig0bKwUH4hC8S4yDVlaIsj92XHM+0yx9lm5aFLwUMQMYD2Q5GeB8uX0KXZDhVt4MWKtT7kXs25Xw1C4u8xAfUCmHPY7J5lhVeQjB+5M191lZXeiJsWjCNnPnvzCUxlM2bSF48aw47YUTkeiaUfBCyV5RlUSe89n+Q29v712RFwLTTPBaK1smb01TPOHH4KkZ9yQifVkckZEJbxJ6o6LrsNaGN3LtkB361OsqoTi/QfAWaG6DUlNPrTkcs132cduVzGsYPOcz1vpaFyU9MRZ8T/aVrTBk5DepcRHON/8p5azjCLwT0K8YcK/zb8LaWsanoJezpISLggd/k/NtYyJLApEC2dacPa7ZZGgyeGojU3zKhZo6E/8IxonUKEeinPvXoH8Sua7fGxkZWRjWq1zOpqUBSuo98L9E11HGndAXgdTU9yvAyfzC4AWH/4bOuTqlVPUf9me0B3t/5LxP0z4pmwjeZDUpk61gzuF8o/5JmWtEJR1cEyPt/RQUBU9QFiB/o04plcKZrEvlTVu4Az0HXM75rlHwgj/6PJs87sw34l+XcV1bYbNoFLzg1N4sp2zhb4OuQ2PhfhX0eyXnL0+j4CHfoMyN59N5h35VYdBXbLzCypqBnIYOOv9GVm/dbvqNzoB70b824dXgwhlSdlQ8vtP8NADwlkCvBv3a52DFnCnDJ97n1f/7n6Q2xJjDtkzmAs6/sddndTJbh+6y/7NzCjqBLY9kdeYWIPbpbbq2wnkHG6/BiRcsvx1AwNdh+3PZjQhcBx20hP8A2YVXGl2qHzIAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFAAAAAaCAYAAAAg0tunAAAFCUlEQVR4Xu2Ye2hdRRDGT2gExfcjhuZx9uahoYioBAtqFClVLFJBRa0GQShVQUUsPjAoVTSIYkWKby0iEhUJFJGKimBF0eAfgtDaogiJhEoREYoWArb6+7K7yWRvzr25sTFcuB8Muzuzr5mdmd1zsqyBBhpooIGjhv7+/mO6u7tPTvn1gGXfuxYvlUqvdnV1rU5l9QAZMM/z551z16WyBYGBFzPBL5T/GPoT2h/qh6GdGOrsdCxoRvYiBrwzFdQT2tvbT0ePj/6TEzDBNuhQZ2fnhZbf0dFxFvyfob0s1GFlGP4K+LuWNQSOEnCCq6BPW1paTkhlVaFBMgS0p62t7YxUzsRvyRsx2PrIw7DH0f4Y/t22b71CToAuX6PrhlRWFRiim8G/Ur5Js8nKjHGnoIsin779tH+CVtn+9Qx0GYZGqTansoogbK+Rh2H9O1IZvJucz4MvZGZi9YX3ZV9f34mm+wyUnMkpl4lUh7VCRocGVE/7LxZxHc2dzZ23Cd6pCa8i0GcdNJGmqqoIlpeHaYKVIjbl2MAW6gcw1s1ZshGFtcjyIjQW7GD87ZTbKL+gfB16gPonlFvTMYtBCLsR6H7oB2hjlLG3y2n/4UzUVEOIqsn0HqgIE6K/KYQpXwu0HdoDPdnb23tSwZhhyxdCbnyFslftsKm/UOgx6mupH9E6WZIqFoMQBYOs1U45rjWiTHuTMWrxJuedZyI3ub4q8gr5jw2VnL+Bv+vp6Tkz8qMBGfOQ7S/IcAy7L7ad9+op+g60trYeL680Sq1Adr3zBzaUHlQV6An1qJRmvQ2Uh1zwNtrHUt8pUl3rOW/QlxTuWcHhaS5oAhpMZYWolP+EUriBoXWRV8mAKcLGx+UlqUxryrCZN+Qj0Fd6k6X9qkCGfBcai8+p6BTQkIxHe4siQ08y6j/CvyWdRHCzBtycygrhCt5/gnmqKOzWRv5CDWhCvexmM7I31A5K6/E+c1ALQQxfZ9KJ5oAOa8/QeuoH8bzzgkwH+pmiYXaWmXG1hXC19x9GvRTZVKn8galTH43KW4QTfy/3j+xV0O/W0NQ3ocyVqqP8ubpwVA9999uDElj7FJHlWeQhx1qlg5GUlrpD2rgkvASiw0yH9uwsHjUfYlQQGsnm5gWFlE5Rt9juqKRF0UbgbXY+5Dciu031qFy4nd+eL0xl5PSg6N9H/wPQJNRl+0fIs5AdtGvkPkzL9kYe75Q+SluWH6EodD7nV37bBs9SrEtRkd55k7n/JtZm/4bGad+jME7HCyF37k49lzED8L+HRnJ/qz/h/G2+HYU+cPNsDqVXw3+HtU6z/JD8NdcR62EW4WfAy/TZqzWgfc4f2pz0En8aQDdkxZfIILRrUZ9ztYKF5E77ZLBUpg0EL5veaNq2wKjnM8dTCjUZ0N72Ecg3uYKw0jgZR2GusfS71pkbWZCc9hC0Rm1dJln518b0ZVQyT6GlRpPz3qUvlDLDLAQhpJ/h9swpV2KoW+e5zKTYszqwhK9DvMD5NDOdi3VQSgO0PzSRo33eJQ/WGkoL9HlY/NmZ/PML+Vh8v/4vCCH2OZs7J5VVg/ITY3e4ub/Ryh6+Jf+X5OlsnkNyPk/r19vVMhjlc/T91s5R8l8kSlF2nSE7D2ii3+Po8aDqiWxpofzF4u8v0S+tZpS6sWhuhS/G2Ap9A43lFXJ2JYQ7YbRonSUHi69h8/em/HpAeEcOL5vxGmigZvwLZSSAT/fZa6UAAAAASUVORK5CYII=>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABEAAAAZCAYAAADXPsWXAAABNElEQVR4XmNgGAVDBMjLy1vJyck9AtL/kfAXIH4GZf8F4q1KSkpq6HoxAFDhJCD+Jisra4osLiMjowoUvwvE16WlpWWQ5VCAqKgoD1DRASC+KiUlJYIur6CgsBDkKqCLfdHl4AAoqQRU9BxIzwdyGZHlkCz4CcSWyHIoAOgFP5BNQBvT0eWAYuHQcJkC5LKgy8MBUEEr1CZPIJYEYUVFRXmgy+qB7JdAgyKBypjR9cEBknNfg7wDpGdB8VwgvgrELSoqKnzo+lAAPq+AACxQQa5El4MDeRxRCwLA6OUEum4HUP4fkHZBlwcDQlELNNgWKPcT6JpdILXo8mAAVKAJxG+BeCkDatQyg5wPxO+B+AookJHkIABqw0OoX0EYFIVPoMn/CRD/AeIHQH4uyEvo+kfBiAAA2qRbNqPq0PcAAAAASUVORK5CYII=>

[image4]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAbCAYAAABFuB6DAAABK0lEQVR4XmNgGOaAUUlJSU1RUdFNXFycG10SDGRkZDjl5OSmy8vLzwHSuUD6uLS0tAyKImNjY1agxCyggvkgtoKCAgeQvxXIL0dRCJQIB0q8lJWV1YEKMQL5S0EYxAaLAI0XBio8BRRcDuSygMRERUV5gPwDIAxiw0yLAAr8A9FQ0xiAfEkgfohsIgvIJCB+DnSPEkwh0AmmQLFvQDwJLAAMBnEg5y4Q/wIqfATDQP4XIP4PxNGkKQQKGgM5X+FWQADIOWuA+C0Qa8IU+qLoZAB7RBGIn8gjhQJM4TeQ42EKoaEAErOFiYEU2gAFX4KcAOJDo3EHEE9ngJkGAlDPnAFpYIDERg4Q7wYmDH64IhgASgQD8UV5iAe2Aq2WQFcDB6BoAioQQBcf2QAA7m5TxIbZtuYAAAAASUVORK5CYII=>

[image5]: <data:image/png;base64,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>

[image6]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAmwAAAAuCAYAAACVmkVrAAAFCklEQVR4Xu3cz2tcRRwA8A2poPirojWk+fFMGixF0UNAEVRUVFpEKOhB0ZP+AQoepOKhUHqQ4kW9KIJ4kIKIvVQp6CHQi1BRK+pBvCi1Qg8eRMVaMH6/6bzyOsnabDaxIp8PDDNvZnZmdjawX+a9Ta8HAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABuqaZod09PTD9f1aWxs7PLZ2dkbozhSt10sN4RY8+FIj9Rt/wWxrl113cU0OTl5Wazpw7m5uavqtiGMzszM3D0/P39J3QAADCmDiUgLneuDW7ZsuSLyQxMTE9d2uvbiC/m2iI1uj2Du/kjfdtvWKgPDmOvHSL916+P6/UjjMeetkT/Xqf+mrG9fBI5XZ12saWfmWR/lS9u+F1sGkJG+LuWFXF/dZ1C5X2VP9kX6o63PQDo/s2i/KfJPsy76RbE5nnsS+a78/LZu3XpdlF/I9sifjrSjHWOt8nOIOfaUsb+q2wGAIcUX/Hx82b7dXscX7vfd9lb0ezLafi/lDLIW6z5R93Kkw70BT99ivOebKmCLNb3UlqPt85I/kest1SNNCYKi77NZH9cH2tespxj/nhj7xbr+AnJ9JyLN5EW+v87a16QEXodyPRFMT+b4EYSNlfe+tH+lfn+WI/8p97Z9fdtnamrqrkg3x/VDbdtaTU5OzsU4J7Mc+XjT5+8HABhCN2ArAUGedo3Xp0FR90mm8poMsJaCt1qeykXb0SiO1m39rBSwxfVinhqV8gclf6Ub9JSAZDbLGbhs1O24HLcbQK5GrC2Ptw5GcVO5PpHBVNVtYPkZRTYS+c4Y868ob4ry3iifqU8Ys70K2M4F2VEeb8vDKHOfznI3cAQA1lE3YEtNnxOS/LKPdCr6/1DKx+o+XRlsRd9not9TdVutT8C2o8yzmM9aldugC1XANvCpVbzXx/M9rCXFfMfr8fqJvm9GOlledyrSe70SvLWi7Ug9Rzd1+3bFWEcjfZa3Rsv1Qu5TFDdFfm+kX0r94nSfgG295FyRfi3v889IZ+o+AMCQplcRsJVg6Vj0vab0OR3pjrrfSvJ0Kvp+F3PsrttaGVQ0nYAtH4pvA41yi28p0Mh15nrbfvma7vW/IeZ7K+b9qK6vlX1sT9cONuXW6HrZvn37lTHm0VjPA5F/3O5fU25LlnzggC32eHP0e6NfqvtH3ZmY485SPp0nblUXAGBYAwRs70RxpDxg/u5qbj+WXyN+kXnd1lUHbOWh+nMPw0f7kcyjbn8VsA18mzFvGTZng5mBU8z95Grnazr7mOvsrfBcX7l9vGyeNtX9y97v7ZWxSgCbe5eneSsFbBlMDRSwDSrfW7snUT6Zz7TVfQCAITXLfyW6LGBLTbml15RfIP6T6HMg0n29VT7HVoKOc8/ElWex9rTX7foywGlPcDIwiPJjbZ+N1pz9lyHLgq5+ov9rmWfwmb/OrNvXIoPVGPfnDJbbh/0zWNq2bdv1Tfl1ZnP+LdH8JenSc4flpDOfqVtX8Rl8GeuajbFfXU0QDwBsrNH8tw11ZVd8eb8ewcktdf0ajUYg8GiM92C3Mp+Ni3l2R5Ay1a3fSBEcTcQ6mrr+Qsp+rSpoHUTuSTlpPG/srF9hX5b2sXsyud7i89hc1wEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA/M/8DRdTQ7eDLYCAAAAAAElFTkSuQmCC>

[image7]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADoAAAAZCAYAAABggz2wAAADUElEQVR4Xu2XTWgTQRTHN7Si4hciNZQknTQNBBGkElEsRVDUi59QRdFebA+KiAUFi1CkIKWoiKJFUArFgwgqiIeeFFr0oNhSPIgXEVEKnvQgCIrY+HvuTJhOs/lSK8j+4c/uvPm/l/d2Z95sPC9EiBAhQvwnaGxszCilzsMb8EA8Hp/vaoogkkwm1+F3BV4j1gaxmcmGhoa92Fvq6uoWir2pqWk5tnZ8mvMRXOCwET5DtD+bzc5x56sB8drgK/lhSYb7s/BhKpVa4mpdRKPRBSQ9hP4+XMH9Sq7jXLdoSS1x72DLObxbMr48bYQd8AXslB9zNeWCp5sgxmt40NhIcinjMXjM1hZABO1lCnlsksbnhBSCvduIGA/Cl9jec70Ht2GuyUcpBXmj8mZxHIc96XR6saspBSkQfiGJrGWOYLsFR/VyKwjxEV9yOGxssVgsjq2PuZSxMb7qxK8aNQTbBJ/AC/zYMlcQBOXvK7dQj+RvYv9gJ+wCTS+a72ha9ZKvxzbP1f3JQg1MUxiBA/LDrsCFLiio0Bl2AymI+WGt6Wd8XfnLVrbBUc9qRpILmktcJ+AkfApXW+GqA0t4LoHPSFD2c9qdN9BvYbRQQaUKtXxzcBBTrdiV3yw/JxKJnUZLjCHinfb0vmTcjuYT3Xmt0VQEu0kRrKtUk5J5tI8KFVRBoVNoNhs743r4Dg6bZZzJZBZ5VvPR+1je7G1PP6CyoFt8F44TyQqPnaCCguwGQQ/JKlRYcOtYmje81ag7PwPSZRH3KP9c3eVV0rI18O1zkxXoQifl6dt2G8o/NooWSpxDXKe4HgnSGPsMaKE0mxHY4lVRoIHsJWL8sJefLDnlN5r88gM1SR/5rpr0j7Zv+LYam1XEL1/murnP2YVaSzf4+MJxO04PeOWrPKuzVQs5ioj3HPYamzQwSUQKMTbGnZKwsvaVfMop/0PA/jiY1oy4Xw8H7O1E3H3YvsI2Y5sVkNQafvQtCZ+Ce7gfI5lzdnLYd0hyovGmHx1yfk9i74fH5R6etDQRGSt/9cnDknP7o3KOoFmDNBdWyVYK3C2fhe58MZTjK3aZF13Jb1yBPGXpVMrfC0UpS8v7jf37T8FTaVb+X6lyeFEKdmOECBHir+AncVgK5YISmn4AAAAASUVORK5CYII=>

[image8]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD0AAAAZCAYAAACCXybJAAADI0lEQVR4Xu2XW4iNURTHv9MhI9fScdK57HNmDieRSydkCkNRk1sht0SZkjel5NLkkjy4TQmleXF7IUkePKipESWD8DBJ4oGMNzzNlKTjt3x7Z7c737nMmQz1/evfN2vt/7f2Wvvbe+0znhciRIgQIUKUQjSdTm/MAHdAfEqprYlEIik6zIZsNjsb/S75W2sasLenUqmZ2hcVvdZkTCzizIXX4S20i3BFzFhJIFwKnxBjc6FQGOmO14pkMjmapFYT7xxx+2A/dsHV4W+G32HRYj9caTTEmMi7PY5GeMbkyvgMdLv5MwojjK3HbjExAiGJIt4JX8G2eDw+xtVUCx2rVSYmoSMqoGjxMfYavpF50R/jOcXWxGKxsfjuwV74nneuoFvgWV8Se4sdX2LAfcauCFk9+eK89By253K58a6mFpDM/gpFn3f9NqRo8rnqLoYNjsQKNEuMzaJPJfY2W1Mtoky0DD6CpzlHk1xBNfgbReuddQKuk/PM81RjY+MEV1cLIrKdCNQNL5SbvBSqKPoOvAbfYX+U4yBFGI0u+ibjHcrf4n3Yd2HGCiWIyIdpamqa7Plnu36wzUeR0GEm/URSOXc8CJWKhj0mniRNLU/Rd5omJUWjuY9/k+efY2lUx/G9ZVsrO96QwW5wTLSn1gZXrmgpzI2H9pDyO/pC7Yrk8/lx8jQatvA8xgdYiKPGNySQZKRIgr+QxjbYq6xc0aWg9UW41x0zkFgSE3a5izYoSLcmWLvy7+21Xp3nI6ho2cr4n8FefQ5tfVGeYpPDSewf2MstjSn6gWx/468Zyr/bpFF1w2avzmINgorW831wi1b+9i6yhdeIrTv3T7tos73xXfYq/fIKAi+vko5IY5jlDTZIAHTRA5KoMzSCsUtyMxiHXDPYD9F3mStHjhY84P3JS26Tg2i+ke988+6wQ/eDGyT2Vb6axc+ww+ik+yr9OwC2wZfwsf4t/hvSS/BdhLcpdofyr7cvsNVo/jtIURS/mEXaQCHTvdLHKsKXnyYaCm/J6H9G6oKeOK78c1aWQ3r5DydYuTkU1Fklz0rxbowQIUL8E/gFgivvJmyCBgAAAAAASUVORK5CYII=>

[image9]: <data:image/png;base64,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>

[image10]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD0AAAAZCAYAAACCXybJAAACmklEQVR4Xu2WT4hNURzHz+1RNP4k3ry8v+57T68xKXpJRCSJNNEojZLZSbOZFTZTVjZEmYXF1CRpSo2tWAlvo5SFYicpFMVC2E18fs05Oo53n/vuefIW91vfzj2/f/d8zz1/rlIpUqRIkSJFO2TK5fLxDcB12BA/GHPtGgG+7ZVKZRpeh4exZewAbFvhLThfKpV2S47tT4R6vb6KglO8/DFFa67fRrFYXI7QEWKvkfMOfqPfdOOwD8EJ+AAuEH/TjQEBuefgI2LCQqGwlnYOzjSbzaUSgG+Y3DNqcSICfMfo7/2tSjfQL7kMW3Cfcma4HUQ0sYfkxQzoQifRxByl3QnfthMtefg+0O6ybFVsb4g/KH3aE3Z9fOvhWdOPDZJCOCtfVpaWSrhcGMz5KNEGepAi4g/R2C+KT2KMrdForKTfouYNukEYhgfI3WP8TPpGfCdN/6+gQIOC80ISN6mEYg18RNNfhv2uKzqbza6g/xA+pe4avbJkckZlP9Neqlarq+1a7WAOintwFoZuQFL4iLbERYm27YFsxVqtNqhibEGZ0SMkf5GZUp5f1oWPaGOPKbp7JDmw4sBHNFsth/2VK65nog3M1QSfyApQnuJ9REeJi7J7I5fLDTDQSYo+oz0lh4UbEwc+osES7HdccZbolpzkdkJPID8ADGZMi5+UyXBjOsFTtMn/BIeMLZ/Pr6P/Ak7bsf8CGVnuDOI+X73uOqOgB/2dq2Sb6zMwouGccg5SeVdl8cfl1y+qvpY+wh127H+F3hq3GdRn+MPie3jVxBGzXwTBBSvmKwKfI3azicM2Cl8TfxrfOM8v4YTq8U3Td5DbBdEjQnl2/V1D9q2+HmSZdWTsy7/fwZLZgqCZmLwi4t0aKVKk6Av8BAYn4vmYPfmoAAAAAElFTkSuQmCC>

[image11]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAD0AAAAZCAYAAACCXybJAAAC2klEQVR4Xu2WXYhNURiG92kS8pc4Ts7fnvOjk0R0QkR+kohJzUSUUpQmN6IQTVy5oVEmuTgu5kJKcePC9dApKZIb7iQaiuIKdxrP56yV5Zu9zd5njmYu9ltve3/fete317v22mttz0uQIEGCBAmC0FUsFg90A92gUS6XF6AdzufzK1VTiu7rfd8fgjfhHnJdroDcGngb3isUCpulj9s+Dgi3wacUPliv12fo9rhg0LMZfA/1rlP3A/xOXNc6BTF2PkCbIj4LH9NWyuVyi7jegQ07VtpW0Lffa01EirY+4q1OjWDIQBEfhS/hsUwmM0drosLU2i0PZkCXAoyMg3mTX7VW7sl94rrJyZXJvaPPLtP3kNuHtqXwjI0nhMyevHE6PYcD1Wp1vtbEAYM5p41oyLJGcws2tJb4shgUIzZXq9XmETfRDROmSqXSTsa8xbYz6ctoO2zjOOii8HYpDq/KstKCKIhgWpbjadintRiZRfxQm06n03OJH8FnaBealSWT0yvfM9crMpF/HhEfdhMZgTfch0eBNqLBW1pH+6CsMK11zIWZdvMpeTGVSmWJpza5tsEyn8lgLvKQUWa2qtvDoI24MMu6waR2S6y1YsgYi2K6c3A3OAZzMu4Gp404kBV0ivx+m9BaVkGG+I02999MizkxSdEXsrG1e5RpIxZ+61z9vazDtGHmwvJtQ3ZrCg34rXN7nzfJ70MbsaB2P7n3LtF9g2PwM2yyKWW53tfmHNNN2cndurEgRf3WRjUCN3qTNGsRZjoIQVqT+wKX21w2m11M/AoO2VxsUHgvM/+Ab2iVN9HvW0yYQf/gra3VbRroLmitbJrkRuUTszlzLMlq2GBzUw6zH9z1W39YYw4/wmtaj3aHMWF1P+ETc/zIZPTCt+iOY/4I96/hCa/DL2jaQc5gTPcI2/1R+guyg5rjQb7rf7Kjh/9UgiWz2m/990bhoJjXNRIkSDAt8Asus/SJckSovAAAAABJRU5ErkJggg==>

[image12]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAZCAYAAAAv3j5gAAAAzUlEQVR4XmNgGAWjYEgDeXl5Q0VFRTd0caoABQUFczk5uTKgJaeB+D+QXY6uBi8AukwcqHGWuLg4N7ocMoBa5AtU6wXEX0m2CKhJEmjIQlFRUR50OWwAaIHxqEVgQBOLgAYKgAxGxsDEoA+kV8vIyKigy2GznKBFoFQFVFANxLPQ8FIgvg/UOB+LXDK6OQQtwgVALqd60GEDg94iIK5Cl8MLiLUIqCYDqPYZEP9Hwu+A+LCysrIYunoMQKxFFAOQBcDgCAUyWdDlRsHwBAClHVEgTEjNZwAAAABJRU5ErkJggg==>

[image13]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFUAAAAZCAYAAABAb2JNAAAEOklEQVR4Xu2XXWhURxTH72KEFBW1NUbzsbNJ2gYpQiVtg6Log6ZCmyJNpVVDoVVRRLEf1jbigyB5UCmKHy9pJEgppVroi3lqoUqglQo+SYS2UioRoUWftNiHJP39c2ey05t7s5sNrqXcP/zZO2fOzJxz5syc2SBIkSJFihQpUqQIGhsb52az2b66urqlvhzZRmPMiqqqqtk0M01NTQuRdeZyueedDt85dM7Ai7AD0Yz8DOUF9j+BDZthD+yura2ti+oIDQ0NzfQftXqbNc7vR7YMfg4v1NfXr0KU8fuLQYa4dDHBAwLW4skrkJ9HPhrhBW2CFBRkdD5xRtG3QkZ6c5QN2LEI+y/DEwomv69iy2/8rvT1kHXAQSWGkoXvw/Bb5xP6z9G3MwiTIyN92mv8OQqCAa0MvGcmBlUG9MLryG/x+zV8JfAyEflaxq93bWvkAWSVTlYmVFhbb5CF1U5Iuxte8ZKgnvYvcIvTwYf5tK/C3Wpj+yY/DsgXw49cuyC0GAM+M+ExiAvqqajMh/pMeOTHQIY8hWyvr1MOYMMSeBdeslfVGLClHdmINt/qbYnxU9n4hRvLprQR2NWuk1P4DPqdnv6k0GQfKCgM+jhmsYJBDcIr4j30ttoAH8MoE1V61LBrP0gI6qj8U5vvk3F+4sM55HeQN9p7WRn+uu5Tfo+6TC8InH+JAZ+2tLTMnCSop5Ed5/caHII/wmW+joBR83TsNFe0rxwoFFTYq7YN3gQ/Y+QZnTrVjKDYwmuPfQ+T5dROCirtPlS6Ajsx7U707mlDfL0SoOL4JnPdhsN8f5VUqaurq2ex7vao3If15wocaG5unuPkJsy4UQXN3veX4vyMCeqUIYfeZ4KNTpAUVGvg+E7JcRNm7Jc0K/KaUwPrrGOOfoLxrLKB763wBlwe1dUJMLaITAYTVvU/8a1VbSWMnXMsqNocvr+L83PaQTXhG2zs2DtZUlCjMGEl/B3e9KvsFKFK3R0dz132tAmz7d0gv5EzaO/D6Zd93QRksuEz6ldr4zeM22G8OzUpeEnyosEEOxl8yycT3tfi8A84YN+f7/A9In031uSDKi725y0WNTU1C0zCE4XAPsl657HpZ3TOmvAuP1PqXW2DOpLNV39dBxOCZ4M6lHQFlYS4TLUyHZ3xoHrH/18FYSpQgApsSEbXAuu/Ye0pqlCgu4d5B7zXhzK3z3jvVKr5a7SHXZAF/KtE1i/q28mnDSY8AP9i0Rc92XJ42s+SXFhcHhrvbfpfgc22v2W32vZ1c0eBdDqq6Oj9BA85mb12hpC95WTTgnbMhEd+1HIY/mCfEnrHfgi/N2Eh0RtPD+xd6ovO9bhhs3AQboMH4RD+vR1EbEXvBRP+fd2v08D3VQJ6pNQrpiTorx2LbmDn24p+BD8mqPgRqHbZqmof7XdQn3Tkl/yL9qdIkSJFihT/K/wDtUBGR1UE5osAAAAASUVORK5CYII=>

[image14]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGIAAAAZCAYAAADKQPsMAAAEDklEQVR4Xu2Ya4iNQRjHz8kqcstlbfZy5uzZzeaD0HGJKCkiKcmtSFJSPpBVZEthfVZKkZQkRRQSuSWXD0SRIiWbSy75Qgm5tNbv7505O2eya+l1snn/9e+deeaZZ55n5pnLOalUggQJEiRIkCBBgt+AMWYY3Az3ZrPZrdXV1fWhzs+A3shMJrOgrq5uKNV0eXl535qaminYWRLqdhf4cwHXE1tNqPNXUFtbO54BL2gCKY+ifBq2yQma06G+DxZtsdUtkIV5hnx0qNsdgO9ziOGM5qG+vr6ccjP8BOeFurGCjO7N4CcYaAXVHpJVVVUNZiJvIvtAWz7oUgTreIvlDeprCaB/qNcdQMy9TJSEb+EYyYgnR/kVvF9ZWTkk7BMbTLQNn8J3ygJP3mSiDG/09UNoIeDGUN4dYRfiOGylPFUykrKa+nPYwvxUBF3iQz6f78mgO5nMc1oUJ9fkaiF+Ncm/WgjZVwA6/hScHU+Ya3dbYRfKlnQ0IYGZkkFjy5eUPZKpz2QevsF9VMtKHU8ZAx/zM6Mj2MEO8D2rrIGPKa9MWYfkIfUrdnfp6NqjXcZ3mdXfR3kt312oLobnKd9VJgZDlRzyAX+u4s8t509J46HzBDp/gHu14mG7Dy0Eepe4awaprtcW9efIN6TaL/o0skPwG7Znur6m/fhrdrr0m0z9q+w6vY6Qy+WG24lSxt6D01IdPC6wt9B4O74z6AWI7jXFAW/bI9u3+1fiKQLBDaDjRXiwoqKiT9geAkd6iZ7IOakgaj29Aya48DLR8feR19o4T5Y30SOhU8ftZF2As9EdaJ/M120C/NiNHtLI1/zJZYvNafIR/7f4SRl3PEXQQJloq+3Qayps7yqsk21wViC7rN8ZTmYdL3qZddVx7C3ys1GwL8DdcL//csPeJLgt1cFu6QwNDQ39TLQ7WrE73cnjjqcAtwgMsCllMwoDI9iWMwLVAnQ0oPMCnvIXzi2EP3jcjqPTqEszlIMe9F1D+2vGPAJPUr6FrgkVQ+g0QH+LiX6MFhbNi6fwKIk7HgcdJ+sxvk5lJ6S+yng/ZOQo267S6bhBgoVwR1ORQ3E7znhVfMpCuYN7tSiRunLECtLXhJvoOf/jPpG/8ltyzYfTjTseIY3R5Sh/NNEl+8yR+hu+k6WkwKjfhZ/hRMnsfXKI/llnTGUTvR78i94tzjVtdafbmeN+0KWCHfsN3O58t4+Pl/CB9/KJPx7T/oOu7Sd8haGc9GxmnKH+0N/m7JCxyG/Dpkx0JDzhe1iLpHb9F2WihXE239O+lO91T/YF7rBU2cmPdjWbY4ImeDW8b6IX0BJ4Bz7K2r9s/ul4ZFxHAM7Otbvjty/FfwlKIsXDBM+32R2+whIkSJAgQYIE/xu+A/Utlzn3bZcSAAAAAElFTkSuQmCC>
