VillageON: GitHub 저장소로 Streamlit 앱 배포하기
================================================

1. GitHub Pages 404가 나오는 이유

현재 앱은 Python으로 실행하는 Streamlit 앱입니다.
GitHub Pages는 HTML/CSS/JavaScript 정적 사이트 호스팅이므로 이 앱의 Python 서버를 실행하지 않습니다.
index.html을 추가하거나 app.py의 확장자를 바꾸는 것만으로 Streamlit 앱이 실행되지는 않습니다.
코드는 GitHub 저장소에 올리고, 앱 실행은 Streamlit Community Cloud에 연결하세요.

2. 저장소에 올릴 파일

이 ZIP을 먼저 압축 풀고, 안의 파일을 GitHub 저장소의 맨 바깥에 올립니다.
ZIP 파일 자체를 저장소에 올려 두는 방식으로 배포하지 않습니다.

- app.py: 모든 기능이 포함된 전체 코드
- requirements.txt: 서버가 설치할 Python 패키지 목록
- .streamlit/config.toml: 밝은 테마와 입력 색상 설정 (폴더 경로 유지)
- .gitignore: 개인 키, 로컬 가상환경과 임시 파일의 Git 업로드 제외
- README_DEPLOY_KO.txt: 이 안내서

이 패키지에는 실제 API 키나 개인 가상환경이 들어 있지 않습니다.
기존 저장소를 사용한다면 현재 파일을 확인한 뒤 같은 위치의 앱 코드를 교체하세요.
다른 프로젝트의 app.py를 이 앱으로 덮어쓰지 않도록 저장소 이름을 확인합니다.

3. Streamlit Community Cloud 연결

① https://share.streamlit.io/ 에 접속하고 본인의 GitHub 계정으로 로그인합니다.
② Create app을 누릅니다. 앱 보유 여부를 물으면 Yup, I have an app을 선택합니다.
③ Repository에는 이 앱을 올린 저장소를 선택합니다.
④ Branch에는 코드를 올린 실제 브랜치를 선택합니다. 보통 main입니다.
⑤ Main file path에는 app.py를 입력합니다.
   앱이 하위 폴더에 있다면 그 경로를 포함합니다. 예: villageon/app.py
   경로는 대소문자를 맞추고, 윈도우 역슬래시 대신 / 를 사용합니다.
⑥ Advanced settings에서 Python version을 3.12로 선택합니다.
⑦ Secrets에 아래 형식으로 본인의 실제 키를 입력하고 Save를 누릅니다.

ADMIN_API_KEY = "행정안전부 일반 인증키 Decoding 값을 여기에 입력"
KAKAO_REST_API_KEY = "카카오 REST API 키를 여기에 입력"

위 한글 설명을 실제 키로 바꾸고 따옴표는 유지합니다.
키는 Streamlit 앱 설정의 Secrets에 입력합니다. 실제 키가 들어 있는 secrets.toml을 GitHub에 올리지 않습니다.

⑧ Deploy를 누르고 패키지 설치와 앱 시작을 기다립니다.
⑨ 배포가 완료되면 Streamlit이 제공한 .streamlit.app 주소를 엽니다.
   이 주소가 다른 사람에게 공유할 앱 주소입니다.
   이전 GitHub Pages의 .github.io 주소는 별도 서비스의 주소입니다.

4. API 키 동작

이번 배포본은 서버의 Secrets 또는 환경변수에 설정된 키를 서버에서만 사용합니다.
방문자 화면에는 '서버 설정 사용 중' 상태만 보이며 운영자의 키를 입력칸에 채우지 않습니다.
서버에 설정되지 않은 API에 대해서만 방문자가 본인의 키를 입력할 수 있습니다.
이 직접 입력칸은 비밀번호 형식이며, 방문자가 본인의 입력만 눈 아이콘으로 확인할 수 있습니다.
운영자 키를 Secrets에 설정하면 접속자가 같은 키를 매번 입력할 필요가 없습니다.

5. 앱이 열리지 않을 때

- GitHub Pages File not found: 접속한 주소가 GitHub Pages인지 확인하고 Streamlit 앱 주소를 사용합니다.
- Main file path 오류: 저장소에서 보이는 파일 경로와 대소문자를 정확히 맞춥니다.
- ModuleNotFoundError: requirements.txt가 저장소 루트 또는 app.py 옆에 있는지 확인합니다.
- 지역 목록을 못 불러옴: 행정안전부 API 활용 승인과 Decoding 키를 확인합니다.
- 장소 검색 실패: 카카오 REST API 키와 Local API 사용 설정/권한을 확인합니다.
- 설치 또는 실행 실패: Streamlit 배포 로그의 오류를 확인합니다.

6. 검증 범위

Python 3.12에서 문법 검사와 Streamlit 기능 자동 검사를 수행했습니다.
지역 선택/랜덤 추천, 예산 코스, API 키 누락/오류 및 빈 데이터 처리를 모의 응답으로 검증했습니다.
서버 키가 화면의 입력값이나 위젯 데이터에 전달되지 않는지도 가짜 키로 검사했습니다.
실제 외부 API 인증과 실제 클라우드 배포 성공은 사용자의 저장소 및 앱 설정에서 확인해야 합니다.

공식 안내

GitHub Pages 설명:
https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages

Streamlit 배포:
https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy

파일 배치:
https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/file-organization

Secrets 설정:
https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management
