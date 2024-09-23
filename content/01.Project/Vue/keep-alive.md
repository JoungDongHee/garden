keep-alive 를 사용하면 컴포넌트가 체인지 혹은 제거가 되어도 해당 컴포넌트에서 작업한 입력 값이 그대로 유지된다.

``` html
 <keep-alive>  
  <component :is="selectComponent"></component>  
</keep-alive>
```
