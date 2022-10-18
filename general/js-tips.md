## 1. Object의 key값을 동적으로 변경하는 법

```
const namespace = '-webkit';
const style = {
  [namespace + 'box-sizing'] : 'border-box',
  [namespace + 'box-shadow'] : '10px 10px 5px #888'
}
```

위와 같이 key값을 배열로 설정하면 된다.


## 2. dataset 활용

```
<ul>
  <li data-id="1" data-user-job="programmer">John</li>
</ul>

<script>
console.log(document.querySelecotr('li').dataset);
</script>

// 결과
// { id: '1', userJob: 'programmer' }
```

attribute 'data-'로 시작한 이후의 값이 JSON화 되어 위와 같은 값을 형성해준다.

반대로 dataset의 값을 넣으면 태그에 'data-'의 attribute가 생성된다. 

```
<script>
document.querySelector('li').dataset.monthSalary = 1000;
</script>

// 결과
// <li data-month-salary="1000"></li>
```

## 3. Object의 모든 key, value를 readonly로 만드는 방법

Object.freeze(object)를 이용하면 된다.
그러나 child 구조까지 모두 freeze되지는 않기 때문에 아래와 같은 deepFreeze 함수를 만들어서 사용한다.

```
function deepFreeze(o: object) {
  for (let [key, value] of Object.entries(o)) {
    if (value && typeof value === "object") deepFreeze(value)
  }

  return Object.freeze(o);
}

interface IUser {
    name: string,
    value: {
        age: number,
        text: {
            title: string,
            desc: {
                some: number
            }
        }
    }
}

const user: IUser = {
    name: "sim",
    value: {
        age: 30,
        text: {
            title: 'aaa',
            desc: {
                some: 555
            }
        }
    }
}

deepFreeze(user);

// user.value.age = 50;

// user.value.text.title = 'bbb';

user.value.text.desc.some = 666;

console.log(user);
```

