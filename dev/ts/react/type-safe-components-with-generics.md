# Generics을 사용해 `type-safe` React 컴포넌트를 작성하는 법


- `Generics`과 React 컴포넌트를 함께 사용하는 것은 완전한 type safe를 보장할 수 있는 방법이다.

- 기능적으로 동일하게 동작하지만 `Generics`을 사용했을 때 더 간편한 리팩토링과 DX를 향상시킬 수 있다.


## code side에서 어떤 차이점이 있는지?

- 컴파일러에서 빌드 단계에서 오류 발견하여 코드 실행 전 에러 확인이 가능하다.

- 불필요한 버그를 방지하고, 컴포넌트를 완전히 **type-safe** 하도록 만들 수 있다. 


- 예시 Code Based

     - `Dummy Data` : list based on an employee interface
     
     - column definitions 전달 시 별도 조합이 필요한 칼럼은 `render` function을 전달해준다.



### Generics를 사용하지 않은 코드
- 실수를 하더라도 컴파일러가 에러를 감지하지 못하고, 런타임에서 에러가 발생하게 된다.


1. render 함수로 전달되는 데이터를 'any' 타입으로 인지하며, 잘못된 변수명으로 작성해도  compiler에서 에러를 인식하지 못한다.

      <img width="490" alt="image" src="https://github.com/ria-ahyoung/medium-trends-daily/assets/136766625/28f08281-33a0-47f0-aedb-a209593ea2d6">


     ```ts
     export interface Employee {
       id: number;
       firstName: string;
       lastName: string;
       age: number;
       email: string;
       phone: string;
     }
     ```


   


3. field를 잘못 작성해도 에러를 인식하지 못한다.

   - 코드 실행하기 전에는 알 수 없기 때문에, runtime에서 에러를 알게됨



### Generics을 사용한 코드


- 제네릭 테이블에서는 더미 데이터의 타입을 명확히 전달하고 있기 때문에 컴파일러가 에러를 감지할 수 있고 이로 인해 코드 빌드가 실패하게 된다.

  <img width="416" alt="image" src="https://github.com/ria-ahyoung/medium-trends-daily/assets/136766625/33282e29-2fa4-43b4-a6a1-f6af49c4da9e">



- 실수를 미리 방지하며, 불필요한 버그를 방지할 수 있다.

- 컴포넌트 개발 시 보장된 타입 안정성을 가져갈 수 있다.



<br/>

## 일반 테이블을 제네릭 테이블로 변환하는 방법

- **as-is :**

```tsx
export type ColumnDef = {
  title: string;
} & (
  | { field: string; }
  | { render: (rowData: Record<string, any>) => React.ReactNode; }
);
const Table = ({ data, columnDefs, onClick }: {
  data: Record<string, any>[];
  columnDefs: ColumnDef[];
  onClick?: (rowData: Record<string, any>) => void;
}) 
```

- **to-be :**

```tsx
//  1. column definitions을 제네릭으로 변환 
export type ColumnDef<T> = { // 제네릭 타입 추가
  title: string;
} & (
  | { field: keyof T } // 'employees'에서 정의된 키들과 일치시킴
  | { render: (rowData: T) => React.ReactNode; } //  'any' 객체 대신 전달한 타입으로 정의
);


  // 2. 컴포넌트 타입 제네릭으로 변환
  const Table = ({ data, columnDefs, onClick }: {
    data: Record<string, any>[];
    columnDefs: ColumnDef[];
    onClick?: (rowData: Record<string, any>) => void;
  }) 
  ```

- 일반 JS 파일에선 단순히 `<T>`를 추가하면 되지만 TSX 파일을 사용하는 경우, 컴파일러가 이러한 문법을 허용하지 않음
- 해결하는 방법 중 하나는 쉼표를 추가하거나, `unknown`을 확장하는 방식을 사용할 수 있다.

  ```tsx
  const Table = <T, >

  const Table = <T extends unknown>
  ```

- ❗️ 하지만 실제로 'unknown'을 확장하는 것이 아니라, 모든 키가 문자열(string)인 객체를 의미하도록 만들고 싶기 때문에, 'object'로 확장해준다.

  ```tsx
  const Table = <T extends Record<string, any>>({ 
    data, 
    columnDefs, 
    onClick 
  }: {
    data: T[];
    columnDefs: ColumnDef<T>[];
    onClick?: (rowData: T) => void;
  }) 
  ```

- 컴파일러가 'object'로 확장되도록 정의했지만, 이 문제는 arrow functions일 때만 발생하기 때문에 일반 함수일 경우 아래처럼 활용할 수 있다.


  ```tsx
  function Table<T>({})
  ```

  - WHY❓ : [Arrow Functions 에서 올바른 제네릭(`<T>`) 작성 방법](https://github.com/ria-ahyoung/Tech-Lab/blob/main/ts/using-generics-in-arrow-functions.md)

- arrow functions을 사용하기 위해서는 객체를 확장함으로써 완전한 type safe 컴포넌트를 작성할 수 있다. 


- Component 단에서는 제너릭 annotation을 통해 타입을 제외한 값을 허용하지 않도록 설정 가능하지만, 대부분 data로 전달된 값의 interface로 타입지정이 가능하다.

  ```tsx
  <Table<Employee>
    data={employee}
    columnDefs={[{...}]}
    onClick={(rowData) => console.log(rowData.firstName)}
  />
  ```

## Generics Component의 장점

1. 정상적 오류 표시 (human mistake에 대해 정상 complaining)
2. 컴포넌트에 다른 타입이 들어올 수 없도록 강제
3. 데이터에 따라 객체 타입을 유추 및 자동완성
4. 완전히 타입 안전한 컴포넌트 작성 가능


- 테이블 컴포넌트는 예측 불가능한 다양한 타입과 함께 작동할 수 있기 때문에 제네릭 컴포넌트의 가장 잘 보여주는 예시가 될 수 있다.
- 완벽한 type-safe를 보장하면서 커스터마이징 가능한 테이블 컴포넌트를 원한다면 Tanner Lindsley의 TanStack Table 라이브러리를 추천!


# 참고 자료
- [Build Better Type-Safe React Components With Generics](https://www.youtube.com/watch?v=L1S_N-gRhOw)
- [TanStack Table](https://tanstack.com/table/v8 )
