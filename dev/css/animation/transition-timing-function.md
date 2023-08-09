# Transition Timing Function

### Ease 속성

애니메이션 세계에서 용이함(Ease)은 더 부드럽게 하는 과정을 쉽게 해주는 의미를 가진다.

ease 속성은 애니메이션이 보다 자연스럽고 사실적으로 느껴지도록 돕는다.


<img width="972" alt="image" src="https://github.com/ria-ahyoung/Tech-Lab/assets/136766625/7b9773e7-4cd4-4e04-90b9-4ca1b3be8b37">


### Timing Function 정의

- 애니메이션의 타이밍 함수
- 타이밍 함수는 애니메이션의 시작과 끝을 부드럽게 전환하여 자연스러운 움직임을 만들어준다.
- 함수를 사용하면 애니메이션이 처음에는 느리게 시작해서 후반에는 빠르게 진행되는 부드러운 변화 효과를 만들 수 있다.


  |               | npm                                                                        | 움직임               |
  |---------------|----------------------------------------------------------------------------|--------------------|
  | ease (기본값)   | 애니메이션 시작과 끝에 가속 및 감속 효과 제공                                        | 보통-빠름-보통        |
  | ease-in       | 애니메이션이 시작할 때 천천히 가속되어 부드러운 변화                                    | 보통-빠르게        |
  | ease-in-out   | 시작과 끝에 가속 및 감속 효과를 가진 부드러운 애니메이션 효과를 제공                        | 천천-보통-천천     |
  | ease-out      | 애니메이션이 끝에 가까워질수록 감속되어 부드러운 종료 효과                                | 보통-느리게        |
  | linear        | 일정한 속도로 애니메이션이 진행되며, 부드러운 변화 없이 일정한 속도를 유지                   | 등속               |
  | cubic-bezier  | 사용자 정의 베지어 곡선 값을 사용하여 애니메이션 효과 정의 가능                           | 커스텀             |
  | steps         | 단계적으로 애니메이션 효과 적용 가능                                                | 뚝뚝 끊어 보여준다 |
  | steps(4, jump-start)| 4단계로 애니메이션을 적용하며, 끝 지점에서 단계적으로 애니메이션 효과가 적용            | 4단계 애니메이션 |
  
- **`ease, in, in-out`** 도 미리 정의된 베지어 곡선값이다.

  |               | npm                                |
  |---------------|------------------------------------|
  | ease (기본값) | cubic-bezier(0.25, 0.1, 0.25, 1.0) |
  | ease-in       | cubic-bezier(0.42, 0, 1.0, 1.0)    |
  | ease-in-out   | cubic-bezier(0.42, 0, 0.58, 1.0)   |
  | ease-out      | cubic-bezier(0, 0, 0.58, 1.0)      |
  | linear        | cubic-bezier(0.0, 0.0, 1.0, 1.0)   |


- 위 키워드 외에도 **Function** 혹은 **Global** 값 지정이 가능하다.

  - `Function` values : steps(4, jump-end), cubic-bezier(0.1, 0.7, 1, 0.1)
  - `Global` values : inherit, initial, revert, revert-layer, unset
  - `Multiple easing functions` : ease, step-start, cubic-bezier(0.1, 0.7, 1, 0.1)


<br/>

### Reference

- https://darvideo.tv/dictionary/ease
- https://chinsun9.github.io/2021/06/18/transition-timing-function
- https://www.youtube.com/shorts/el0Zn9AOfpc
