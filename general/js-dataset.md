## dataset 활용

```
<ul>
  <li data-id="1" data-user-job="programmer">John</li>
</ul>

<script>
console.log(document.querySelecotr('li').dataset);
// 결과
// { id: '1', userJob: 'programmer' }
</script>
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

