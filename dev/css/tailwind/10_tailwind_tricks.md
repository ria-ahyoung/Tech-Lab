# 10 Tailwind Tricks Need to Know



## 1. Group and Peer


부모 혹은 형제 요소의 이벤트에 따라 특정 element 스타일을 변경하기 위해 `peer` & `group` 유틸리티 클래스를 활용할 수 있다.

**✓ parent 요소 hover 시 children 스타일을 변경하고 싶을 경우**

  - 부모와 자식 요소에 각각 `group` & `group-hover` 유틸리티를 사용한다.

    
    ```html
    <section class='group grid place-items-center h-20 w-20 bg-blue-400'>
      // children elements 
      <div class='group-hover:bg-red-600'></div>
      <div class='group-hover:bg-blue-600'></div>
    </section>
    ```

**✓ 형제 요소를 변경하고 싶을 경우**

  - 형제 요소에 `peer` & `peer-hover` 유틸리티 클래스를 적용한다.

    ```html
    <section class='peer group grid place-items-center h-20 w-20 bg-blue-400'></section>
    
    // sibling element
    <section class='peer-hover:bg-orange-400'></section> 
    ```

**✓ 여러 `group`과 `peer`를 사용해 별도로 처리하고 싶을 경우**

  - 개별 이벤트에 이름 지정이 가능하다.

  - `group`과 `peer`는 각각  `group/name`과 `peer/name2` 형태로 수정한다.
  - `group-hover/name`와 `peer-hover/name2` 뒤에도 지정한 이름을 추가해준다.

    ```html
    <section class='peer/sibling-name group/children-name grid place-items-center h-20 w-20 bg-blue-400'>
      <div class='group-hover/children-name:bg-red-600'></div>
      <div class='group-hover/children-name:bg-blue-600'></div>
    </section>
    
    <section class='peer-hover/sibling-name:bg-orange-400'></section>
    ```

## 2. Animations
- Tailwind에는 쉽게 사용할 수 있는 애니메이션 유틸리티 클래스가 있다. (transition, duration, curve, delay 등)
- 실제로 CSS에서 애니메이션을 위해 사용 가능한 모든 속성 동일하게 적용 가능하다.
  ```html
  <section class='[...] transition-colors duration-300 ease-in-out delay-300'>
    <div class='[...]'></div>
    <div class='[...]'></div>
  </section>
  
  <section class='[...]'></section>
  ```

- 사전 정의 애니메이션 활용 가능

  <img width="720" alt="image" src="https://github.com/ria-ahyoung/Tech-Lab/assets/136766625/14ae1cf3-2c37-446d-a79b-e3de9cdd965d">



## 3. Responsive Designs


- Tailwind는 mobile first framework이므로, 가장 작은 화면 크기를 기준으로 기본 스타일을 작성해야한다.

  <img width="700" alt="image" src="https://github.com/ria-ahyoung/next13-beta-project/assets/136766625/2f92ed35-102e-4500-914a-f80c8d4358ff">

  ```html
  <div class='grid grid-cols-2 gap-10 p-5 sm:grid-cols-3'>
    /* child elements */
  </div>
  
  ```


- 화면 너비에 맞는 레이아웃 변경 (ex. 특정 구간에서만 레이아웃 반영)
  ```html
  /* sm ~ md 구간에서 grid 레이아웃 수정 가능 */
  <div class='grid grid-cols-2 gap-10 p-5 sm:max-md:grid-cols-3'></div>
  ```

- custom breakpoint 지정
  ```html
  /* max & min 구간 지정 가능 = 400px 이상 구간 부터는 column 개수가 3개로 변경됨 */
  <div class='grid grid-cols-2 gap-10 p-5 min-[400px]:grid-cols-3'></div>
  ```

## 4. Intellisense in Variables

- html 외부에 클래스를 변수로 작성할 수 있다.
  ```jsx
  const className = 'h-10 bg-purple-700 text-white';
  ```

- 객체 형태로 작성할 수 있고, Class Attributes를 변경 가능하다.

  ```jsx
  const variants = {
    base: 'h-10 bg-purple-700 text-white',
    error: 'h-10 bg-red-700 text-white',
  };
  ```


## 5. Dynamic Utilities
- dynamic class name의 경우, 정상적으로 동작하지 않을 수 있기 때문에 CSS 속성으로 클래스를 동적으로 작성할 때 주의가 필요하다.

  
  ```jsx
  <select className={`bg-${color}-400`}/>
  ```
  
- Tailwind는 사용하지 않는 클래스들을 제거(purge)하여 번들 크기를 줄이기 때문에코드 상에서 어떤 위치에도 명시적으로 사용되지 않는 클래스들은 번들에서 제거되며
- 작동을 원하는 모든 클래스 이름을 코드 어딘가에 작성해 주어야한다.

  ```js
  const dynamicName = ['bg-red-400', 'bg-green-400', 'bg-blue-400']
  ```

  <img width="680" alt="image" src="https://github.com/ria-ahyoung/next13-beta-project/assets/136766625/b39fa589-4e45-40ae-aeff-1ae85f7938fc">


- 미사용 변수를 추가해두는게 알맞는 방식일까?
- 아주 많은 조합을 가진 스타일링이 필요할 경우, 아주 비효율적인 코드가 될 수 있다.
- Tailwind의 동작 방식을 이해하고, 프로젝트에 맞는 방식을 채택해야 한다.

## 6. Apply and theme

외부 라이브러리에서 가져온 컴포넌트 스타일을 변경하고자 할 때, 특정 클래스를 오버라이드를 위해 `@apply` 또는 `theme` 객체를 사용할 수 있다.

```css
._external-class{
  @apply text-purple-200 bg-purple-900 px-6 py-2 rounded-lg uppercase;
}
/* theme을 통해 Tailwind 테마에 설정된 모든 항목을 가져올 수 있다. */
._external-class{
  padding: theme('padding.2') theme('padding.6');
  border-radius: theme('border');
  text-transform: uppercase;
  color: theme('colors.purple.200');
  background-color: theme('colors.purple.900');
}
```

- theme 객체는 임의의 Tailwind 유틸리티에서도 사용할 수 있다.
```jsx
<div className='w-20 h-10 rounded-xl shadow-[0_0_10px_theme('colors.purple.200')]'></div>
```

## 7. Extend Tailwind
- 작성한 스타일링을 프로젝트에서 재사용하고 싶을 때, Tailwind config 파일을 사용해 자체 커스텀 유틸리티 클래스를 생성할 수 있다.
- 작성 문법은 기본적으로 CSS이므로 앞에서 언급한 theme 객체 사용이 가능하다.
```js
theme: {
  extend: {
    boxShadow: {
      neon: '0 0 5px theme('colors.purple.200'), 0 0 20px theme('colors.purple.700')'
    }
  }
}
```

```jsx
<div className='w-20 h-10 rounded-xl shadow-neon'></div>
```
- 스타일을 유틸리티화하여 재사용할 수 있고, 사용자 정의 컬러, 사용자 정의 브레이크포인트 외에도 다양한 기능을 확장 가능하다.

## 8. Plugins
- 정의한 유틸리티를 커스텀하게 활용하고 싶을 때 Tailwind plugin을 사용할 수 있다.
- 다양한 속성과 기능을 추가가 가능하며, 익스텐션이 설치되어 있다면 커스텀 스타일의 자동 완성도 정상적으로 지원된다.

```js
const plugin = require('tailwindcss/plugin');
// theme를 가져오고, 고유헌 Tailwind 쿨래스 생성을 위해 유틸리티 기능을 전달합니다.
plugins: [
  plugin(({ theme, addUtilities }) => {
    const neonUtilities = {};
    const colors = theme('colors');

    for(const color in colors) {
      if(typeof colors[color] === 'object'){
        const color1 = colors[color]['500'];
        const color2 = colors[color]['700'];
        neonUtilities[`.neon-${color}`] = {
          boxShadow: `0 0 5px ${color1}, 0 0 20px ${color2}`
        }
      }
    }
    addUtilities(neonUtilities)
  })
]
```

```jsx
<div className='w-20 h-10 rounded-xl neon-green]'></div>
```

## 9. Colors object
- Tailwind의 색상을 자바스크립트 객체로 가져와서 사용할 수 있다.
- Tailwind 색상으로 커스텀 테마를 만들고자할 때 유용하게 활용된다.
```js
const colors = require('tailwindcss/colors');

theme: {
  extend: {
    colors: {
      primary: colors.violent,
    }
  }
}
```

```jsx
<div className='text-primary-400' />
```


```js
theme: {
  extend: {
    colors: {
      primary: { colors.sky, DEFAULT: colors.sky[600] },
    }
  }
}
```


```jsx
<div className='text-primary' />
```

## 10. tailwind merge
```node
yarn add tailwind-merge
```

- 클래스를 전달할 때, 중복되는 유틸리티 클래스가 전달될 경우 충돌이 발생할 수 있다.
- `Tailwind-merge` 패키지를 사용해 기본 클래스를 재정의함으로써 오버라이드로 발생 가능한 충돌을 방지할 수 있다.


```jsx
// 전달된 매개변수를 default 매개변수와 함께 매핑하여 twMerge 함수로 전달해주면 된다.
const CustomButton: React.FC<{ className?: string }> = ({ className }) => {
  return(
    <div
      className={
        twMerge(
          'px-6 py-1 rounded-md text-black',
          className
        )
      }
    ></div>
  )
}

<CustomButton className='text-white' />
```

- multiple variants for a component and change 의 경우에도 활용할 수 있다.
```jsx
<DropZone 
  className={
    twMerge(
      variants.base,
      isActive && variants.active,
      error && variants.error,
    )
  }
/>
```


# 참고 자료

- [10 Tailwind Tricks You NEED To Know!](https://www.youtube.com/watch?v=aSlK3GhRuXA)
- [Tailwind CSS Docs](https://tailwindcss.com/)
