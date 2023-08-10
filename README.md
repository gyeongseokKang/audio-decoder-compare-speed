# Audio Decode Compare
<img width="1637" alt="image" src="https://github.com/gyeongseokKang/audio-decoder-compare-speed/assets/61446585/a51b2992-2abb-441a-85f7-2a7450d4704d">
"Audio Decode Compare"는 다양한 오디오 디코딩 라이브러리를 비교하는 간단한 웹 애플리케이션입니다. 이 서비스는 @soundcut/decode-audio-data-fast, audio-decode, 그리고 브라우저 내장 디코더를 비교하며, 각 라이브러리가 오디오 파일을 디코딩하는 시간을 밀리초(ms) 단위로 테이블로 나타냅니다. 이 서비스는 스벨트 타입스크립트로 개발되었으며 Vercel을 통해 배포되었습니다.




## 주요 기능

- **다양한 라이브러리 비교**: @soundcut/decode-audio-data-fast, audio-decode, 브라우저 내장 디코더의 디코딩 성능을 비교할 수 있습니다.
- **디코딩 시간 표시**: 업로드한 오디오 파일을 각 라이브러리로 디코딩하는 시간을 밀리초(ms) 단위로 테이블로 나타냅니다.
- **간편한 사용**: 단순한 인터페이스를 통해 오디오 파일을 업로드하고 디코딩 시간을 확인할 수 있습니다.

## 사용 방법

1.  서비스에 접속합니다.
2.  "Choose File" 버튼을 클릭하여 디코딩할 오디오 파일을 업로드합니다.
3.  각 라이브러리별로 오디오 파일의 디코딩 시간이 테이블로 표시됩니다.

## 기술 스택

- **프론트엔드**: 스벨트 (Svelte)와 타입스크립트를 사용하여 개발되었습니다.
- **배포**: Vercel을 통해 배포되었습니다.

## 개발 및 배포

이 서비스는 스벨트와 타입스크립트를 활용하여 개발되었습니다. Vercel을 이용하여 손쉽게 배포되었습니다.

1.  먼저 프로젝트를 클론하거나 다운로드합니다.

    ```
    git clone https://github.com/gyeongseokKang/audio-decoder-compare-speed.git
    ```

2.  프로젝트 폴더로 이동한 후, 필요한 의존성을 설치합니다.

    ```
    npm install
    ```

3.  개발 서버를 실행하여 로컬에서 프로젝트를 실행합니다.

    ```
    npm run dev
    ```

## 라이선스

없.
