# NPM & Yarn & PNPM 의 차이점

## NPM (Node Package Manager) 

- 최초의 패키지 매니저, since `2009` 👋
- 단일 저장소를 통한 패키지 관리로, 버전 및 의존성 충돌 문제 해결했다. (빌드 기능 제공)


![image](https://github.com/ria-ahyoung/Tech-Lab/assets/136766625/7250ecce-60cd-42ca-b512-f85e62ad0db6)


**기능**

- `node_modules` 폴더에 프로젝트 dependencies 설치 및 관리
- `package.json` 파일에 프로젝트의 패키지 목록 명시 & `package.lock.json` 잠금 파일 제공
- `.npmrc` 파일을 생성해 npm에 필요한 설정을 추가 가능

     ```
     .
     ├── node_modules/
     ├── .npmrc 
     ├── package-lock.json
     └── package.json
     ```


**장점**
- Node.js의 기본 패키지 매니저로, 내장되어 별도 설치가 필요하지 않다.
- `nvm`이나 `volta`와 함께 사용시 버전 관리에 유용하다.

**단점**
- 파일 시스템을 활용한 의존성 관리 = 비효율적인 패키지 탐색 초래 (수 많은 디스크 I/O 호출)
- 복잡하고 깊은 의존성 트리 = 큰 공간 차지
- 순차적(병렬) 설치로 인한 많은 작업 시간 소요
- 패키지 호이스팅으로 인한 유령 의존성 현상


## Yarn (Yet Another Resource Negotiator) 
<img width="700" alt="image" src="https://github.com/ria-ahyoung/Tech-Lab/assets/136766625/42b34e80-4b9c-49c3-8d04-5a6f454219ce">



```Shell
npm install -g yarn
yarn -v
```

- 초기 NPM이 가진 문제점 (설치 속도와 보안, 안정성) 을 보완하기 위한 패키지 매니저 🧯 
- yarn `classic`(2016)과 `berry`(2020) 두 가지 버전이 존재한다.


**장점**
- 중복 모듈 최소화
- 설치 프로세스의 속도를 높이기 위해 작업 병렬화
- 캐싱 기능 : 오프라인 캐싱 & 로컬 캐시를 사용해 패키지 저장

  - 다운로드할 때 먼저 로컬 캐시를 확인하기 때문에 반복적인 패키지 설치에 효율적이다. <br/><br/>


  ```Shell
  .
  ├── .yarn/
  │   ├── cache/
  │   └── releases/     # 현재 yarn classic의 버전 저장
  │       └── yarn-1.22.17.cjs
  ├── node_modules/
  ├── .yarnrc
  ├── package.json
  └── yarn.lock

  ```

- 보안 향상 : `yarn.lock` 파일 도입
- 다양한 추가 기능 : `mono-repo` 지원 등


**단점**
- 패키지 호이스팅으로 인한 유령 의존성 현상
- npm과 마찬가지로 node_modules 폴더 내부에 flat하게 패키지를 관리하는 방식

⭐️ Yarn berry 부터 더 나은 퍼포먼스와 사용자 경험이 제공되었으며, classic 버전이 가진 많은 문제점을 해결했다.
- `Plug'n'Play` 라는 새로운 설치 방식을 도입하여 의존성 설치를 효율적으로 처리함
- 패키지 의존성 정보는 `.yarn/cache` 폴더 내부에 zip 파일 형태로 저장 (node_modules 생성 x)


## PNPM (Fast, disk space efficient package manager) 

<img width="949" alt="image" src="https://github.com/ria-ahyoung/Tech-Lab/assets/136766625/cb2d90ef-2438-4fb1-8fb9-d4e90febe278">

- 성능과 효율성에 중점을 둔 패키지 매니저, since `2017`
- 중복 설치를 피하고 패키지 공유를 통해 디스크 공간을 절약하도록 설계

  - home 폴더의 글로벌 저장소 (`~/.pnpm-store`)에 패키지 저장을 위한 node_modules 폴더가 생성된다.
  - 모든 버전의 dependencies은 해당 폴더에 물리적으로 한번만 저장되므로, 상당한 디스크 공간을 절약

- 프로젝트 간의 중복된 dependencies 저장을 획기적으로 없앰
- 호이스트 대신 `content-addressable storage` 방식 채택

**장점**
- 추가 설치 없이 npm이 있다면 사용 가능
- 공유 패키지들을 중앙 저장소(단일 파일)에 캐싱하여 디스크 공간을 절약
  - 불필요한 I/O 과정을 없애 빠른 속도 제공
  - 사용 용량이 적어 디스크 공간 절약 & 효율적으로 디스크를 관리한다
- 여러 프로젝트 간에 패키지를 공유함으로써 동일한 의존성을 여러 번 설치할 필요가 없어짐

**단점**
- 다른 패키지 매니저(npm, Yarn 레지스트리 등) 간에 호환되지 않을 수 있다.
  - 일부 패키지를 설치하거나 업데이트하는 데 어려움이 있을 수 있다.
  - 동일한 패키지를 여러 프로젝트에서 공유하는 pnpm의 하드링크 방식과 캐싱 매커니즘을 지원하지 않을 수 있기 때문

## Command 비교

|         | npm                     | yarn                   | pnpm                     |
|---------|-------------------------|------------------------|--------------------------|
| install | npm install <package>   | yarn add <package>     | pnpm install <package>   |
| upgrade | npm update <package>    | yarn upgrade <package> | pnpm update <package>    |
| remove  | npm uninstall <package> | yarn remove <package>  | pnpm uninstall <package> |
| run     | npm run <script>        | yarn run <script>      | pnpm run <script>        |

