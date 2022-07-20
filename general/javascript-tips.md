# Javascript Tips

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
