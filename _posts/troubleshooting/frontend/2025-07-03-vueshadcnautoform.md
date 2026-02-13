---
title: Troubleshooting - ShadCN Vue AutoForm에서 Zod Validation 미적용 문제 해결
date: 2025-07-03 06:00:00 +0900
categories: [Troubleshooting]
tags: [shadcn, vue.js]
---

## **에러 상황**

ShadCN 에서 제공하는 컴포넌트 중 하나인 `AutoForm`을 커스터마이징 중 `Validation`이 제대로 동작하지 않는 문제가 발생했다.<br>
`AutoForm` 요소에 데이터를 입력받는 것뿐만 아니라, 데이터를 넣기도 해야하기 때문에 `useForm`을 사용하여 `Form`을 만든 후 `AutoForm` 데이터 바인딩을 하도록 설정하였다.

아래와 같이 연결해두었다.
```html
<AutoForm :schema="validate" @submit="update" :form="form">
```

`schema` 값으로 넘긴 `validate`는 Zod를 사용하여 유효성 검증 스키마를 만들어 연동하였는데, 이때 넘긴 `validate`가 `AutoForm`에서 제대로 동작하지 않는 문제가 발생하였다.

## **원인**
ShadCN에서 제공하는 `AutoForm` 코드 일부
```js
const formComponent = computed(() => props.form ? 'form' : Form)
const formComponentProps = computed(() => {
  if (props.form) {
    return {
      onSubmit: props.form.handleSubmit(val => emits('submit', val)),
    };
  }
  else {
    const formSchema = toTypedSchema(props.schema)
    return {
      keepValues: true,
      validationSchema: formSchema,
      onSubmit: (val: GenericObject) => emits('submit', val),
    };
  }
})
```

위 코드는 ShadCN에서 제공하는 `AutoForm` 코드의 일부이다.

보는 것과 같이 `props`로 `form`을 넘길 경우 분기 조건에 걸려 `validationSchema`를 설정하지 않고 넘어온 `form`을 그대로 사용하기 때문에 적용되지 않은 것이였다.

## **해결**
`useForm` 사용 시 `validationSchema`를 설정하여 직접 Zod 스키마를 적용시키는 것으로 해결하였다.

```js
import { toTypedSchema } from '@vee-validate/zod'

const form = useForm({
  initialValues: {
    example1: '',
    example2: '-',
    example3: '-',
    example4: '-'
  },
  validationSchema: toTypedSchema(formSchema.value)
})
```

- `toTypeSchema`: 타입 기반 `Form` 라이브러리에서 쓰기 위해 Zod 스키마를 타입 안전하게 변환해주는 함수
