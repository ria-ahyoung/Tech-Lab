# GitHub Actions에서 `set-output` 사용시 Warning 수정하기

## 문제상황

- 깃헙 액션에서 step간 변수를 공유하기 위해 `set-output` 커맨드를 사용했다.
- 최신 GitHub Actions에서 deprecated 예정인 문법으로 액션 실행 시 워닝이 발생한다.

<br/>

```yaml
# 1. GitHub Actions의 JavaScript 컨텍스트에서 사용할 경우
- name: Format Lighthouse Score 🪄
  uses: actions/github-script@v3
  with:
    github-token: ${{secrets.GITHUB_TOKEN}}
    script: |
      ... 
      core.setOutput('LHCI_REPORT', result)

# 2. Bash 셸 스크립트에서 사용하는 경우
- name: Format Lighthouse Score 🪄
  run: |
    ...
    echo "::set-output name=LHCI_REPORT::$result"

# The `set-output` command is deprecated and will be disabled soon.
# Please upgrade to using Environment Files.
# For more information see: https://github.blog/changelog/2022-10-11-github-actions-deprecating-save-state-and-set-output-commands/

```

<br/>

## 해결방법

스텝간 공유가 필요한 변수는 `set-output` 명령어 대신 GitHub Actions에서 제공하는 환경 변수를 활용해 해결한다.

1. 실행 스크립트에서 결과 값을 생성하고 Environment Files(`GITHUB_ENV`)에 저장한다.
2. 변수를 사용하는 곳에서는 `${{ env.COMMENTS }}` 형태로 저장된 값을 가져온다.

```yaml
- name: Format Lighthouse Score 🪄
  run: |
    comments= ...
    # GITHUB_ENV에 값에 할당한다  
    echo "COMMENTS=$comments" >> $GITHUB_ENV

- name: Comment PR 🔖
  uses: unsplash/comment-on-pr@v1.3.0
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  with:
    # GITHUB_ENV 값을 가져온다
    msg: ${{ env.COMMENTS }}
```

<br/>

❗️이때 결과가 단일 텍스트가 아니기 때문에 GITHUB_ENV에 저장할 수 없다.

👉 결과가 여러 줄이거나 특수 문자를 포함하는 경우, GITHUB_ENV에 직접 저장하는 것은 불가능하기 때문에 결과 값을 코멘트 메시지로 출력하는 방법을 활용할 수 있다.
