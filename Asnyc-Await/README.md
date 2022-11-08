자바스크립트에서 async 함수는 Promise 객체를 반환한다.

```js
async function resolveOrReject() {
  // 1초를 그냥 기다린다.
  await new Promise((_) => setTimeout(_, 1000))

  // 50% 확률로 true false
  const randomResult = Boolean(Math.round(Math.random()))

  if (randomResult) {
    return 'good'
  } else {
    throw Error('bad')
  }
}
```

## Just Calling
```js
async function foo() {
  try {
    resolveOrReject()
  } catch (e) {
    return 'caught!'
  }
}
```

이 경우 function foo() 는 비동기 함수이므로 resolveOrReject() 또한 비동기적으로 호출된다. 하지만 await 하지 않으니 resolve나 reject 되기 전에(결과가 나오기 전에) Promise 껍데기만이 반환된다.

## Await
```js
async function foo() {
  try {
    await resolveOrReject()
  } catch (e) {
    return 'caught!'
  }
}
```

await 를 작성한 경우, resolveOrReject() 를 실행할 때 항상 1초를 기다리며 b() 의 결과로 undeifined 혹은 에러가 발생될 것이다.

## Return
```js
async function foo() {
  try {
    return resolveOrReject()
  } catch (e) {
    return 'caught!'
  }
}
```

resolveOrRejct() 함수를 리턴할 경우 성공반환값인 'good', 실패반환값인 'bad' 가 50/50으로 리턴된다.

## Return Await
```js
async function foo() {
  try {
    return await resolveOrReject()
  } catch (e) {
    return 'caught!'
  }
}
```

resolveOrReject() 가 실행될 때 1초를 기다리며, 성공 시 'good', 에러가 발생하면 catch 문으로 넘어가 caught 가 반환된다.