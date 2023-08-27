# MUI가 제공하는 styled API를 import하는 방법

이전 MUI 버전에서는 컴포넌트 스타일을 적용하기 위해 `withStyles`와 `makeStyles` 두가지 스타일링 방식을 제공해왔다.

MUI v5부터는 새로운 스타일링 솔루션을 (sx, styled) 권장하고 있으며, 컴포넌트 스타일링 방식이 통일되어 훅 기반의 styled 함수를 제공한다.

이는 emotion 위에서 동작하며 `Styled Engine`이라는 스타일링 시스템을 기반으로 하고 있다. 


### 코드 예시

 MUI 패키지로부터 styled를 import 하는 방법 : 

```jsx
import { styled } from '@mui/material/styles';

import { styled } from '@mui/system';
```

실제 컴포넌트 스타일링 코드 :

```jsx
import { styled } from '@mui/system';

const CustomButton = styled('button')(({ theme, variant }) => ({
  backgroundColor: variant === 'primary' ? theme.palette.primary.main : theme.palette.secondary.main,
  color: 'white',
  padding: '10px 20px',
}));

function Component() {
  return (
    <div>
      <CustomButton variant="primary">Custom 버튼</CustomButton>
    </div>
  );
}
```

### Conclusion

- @mui/styles와 @mui/material/styles는 사용하는 기본 테마에 차이가 있다.
- @mui/styles와 @mui/material/styles를 혼용할 경우, 클래스 네임 중첩과 같은 예상치 못한 버그를 발견할 수 있으므로, 한 가지의 스타일링 방식을 선택하는 것이 좋다.
- 새로운 프로젝트에서는 JSS 방식을 완전히 걷어내고, emotion 방식으로 마이그레이션하는 것이 번들 사이즈를 줄일 수 있다.

### Reference

- https://mui.com/material-ui/migration/v5-style-changes/#styled
