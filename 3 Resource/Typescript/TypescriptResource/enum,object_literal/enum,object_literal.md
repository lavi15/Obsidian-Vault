
### enum
- 상수의 그룹화를 위해 사용
- enum 타입은 코드의 가독성을 높이고 명확한 상수 값을 정의 가능
- 컴파일 시에 자동으로 숫자 값으로 매핑되므로 따로 값을 할당할 필요가 없음
```typescript
enum UserRole {
  ADMIN = "ADMIN",
  EDITOR = "EDITOR",
  USER = "USER",
}

enum UserLevel {
  NOT_OPERATOR, // 0
  OPERATOR // 1
}

function checkPermission(userRole: UserRole, userLevel: UserLevel): void {
  if (userLevel === UserLevel.NOT_OPERATOR) {
    console.log('당신은 일반 사용자 레벨이에요');
  } else {
    console.log('당신은 운영자 레벨이군요');
  } 

  if (userRole === UserRole.ADMIN) {
    console.log("당신은 어드민이군요");
  } else if (userRole === UserRole.EDITOR) {
    console.log("당신은 에디터에요");
  } else {
    console.log("당신은 사용자군요");
  }
}

const userRole: UserRole = UserRole.EDITOR;
const userLevel: UserLevel = UserLevel.NOT_OPERATOR;
checkPermission(userRole, userLevel);
```

### object literal
- 키 + 값의 쌍(pair)으로 구성된 객체를 정의
- enum은 number, string 타입의 값만 대입되지만 object literal은 타입의 제한이 없음
- 코드 내에서 사용하기 전에 값이 할당되어야 하므로, 런타임 에러를 방지 가능
```typescript
const obj = {
  a: [1,2,3],
  b: 'b',
  c: 4
}
```

### enum, object literal 사용
- **enum**은 간단한 상수 값을 그룹화해서 관리를 할 때 적합
- **enum**은 상수 값이기 때문에 각 멤버의 값이 변하면 안된다는 조건이 존재
- **object literal**은 멤버의 값이나 데이터 타입을 맘대로 변경 가능
- **object literal**은 복잡한 구조와 다양한 데이터 타입을 사용해야 할 때 사용
