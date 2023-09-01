# 배포한 패키지에서 모듈 import 할 때 발생하는 core-js 해결하기

## 문제상황

- Registry에 배포한 패키지를 서비스에 설치한 후 모듈 import 했을 때 에러가 발생했다.
- 주어진 경로에 있는 파일에서 `core-js/modules/es.object.keys.js` 모듈을 찾을 수 없다는 컴파일 에러 출력된다.
     
     ```bash
     ./node_modules/.pnpm/.../node_modules/@배포-패키지/esm/src/...js
     Module not found: Can't resolve 'core-js/modules/es.object.keys.js'
     
     https://nextjs.org/docs/messages/module-not-found
     ```

## 해결방법

- 프로젝트에서 사용하는 모듈 시스템이나 패키지 의존성 문제(버전 충돌)로 이런 문제가 발생할 수 있다.

- 패키지를 빌드할 때 종종 폴리필 등의 이유로 core-js 쪽 참조가 섞이는 경우가 있다.
  - ex. build 폴더 속 js 파일에 core-js를 import하는 코드

- 배포 패키지의 dependency로 'core-js' 패키지를 설치하면 더이상 모듈 임포트에 에러가 발생하지 않는다.

     ```bash
     # 의존성 추가한 다음 패키지를 재배포하여 확인한다
     pnpm install core-js
     ```



- 바벨 설정 넣어주는 코드에 플러그인이 많이 작성되어 있는데, 파싱하는 과정에서 폴리필이 필요한 부분은 추가된 것으로 추측되며

- 기존 서비스에선 바벨을 통해 core js도 함께 설치되었지만 현재 동작하는 서비스에서는 core-js가 설치되지 않았기 때문에 발생했다.