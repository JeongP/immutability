# immutability
JavaScript에서 데이터를 불변하게 다루는 방법을 다룹니다.   

### Why Learn?
훗날 수 많은 사람들과 수 많은 줄의 프로그램을 함께 작업하게 될 때,    
데이터가 함부로 바뀌어 곤혹을 치루지 않기 위해 배웁니다.


### 불변함의 대상.
변수 이름과 데이터.

### 이름의 경우에는

``` javascript
// 변하는 예
var v = 1;
// ~
v = 2;
console.log('v :', v);

// 불변의 예
const c = 1;
// ~
c = 2;
```

### 내용(데이터)의 경우
데이터형에 따라 다르다.
primitive : Number,String,Boolean,Null,Undefined,Symbol    
- 같은 값이면 같은 곳을 가리킴. (원시데이터 값은 바뀔수 없다. 불변성을 가짐)
```javascript
var p1 = 1;
var p2 = 1;
console.log(p1,p2,p1===p2); // 참
```

Object : Object, Array, Function
- 같은 객체를 생성하더라도 따로 저장함.(객체는 값 자체를 prop을 통해서 바꿀 수 있어서 가변적을 성격을 띄기 때문에)

```javascript
var o1 = {name:'kim'}
var o2 = {name:'kim'}
console.log(o1,o2,o1===o2); // 거짓
```
    
     
         
또한, 객체형의 경우엔 다음과 같은 결과가 나올 수 있다.
```javascript
var o1 = {name:'kim'}
var o2 = o1;
o2.name = 'park';
console.log(o1.name, o2.name); // 결과: park, park
```

    
이 문제를 해결하기 위해, 객체를 복사해서 복사한 객체를 변경해주는 방식을 사용.
```javascript
var o1 = {name:'kim'}
var o2 = Object.assign({}, o1);
o2.name = 'park';
console.log(o1.name, o2.name); // 결과: kim, park
```
