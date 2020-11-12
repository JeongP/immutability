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

> 데이터형에 따라 다르다.    

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

혹은 Object freeze로 객체 얼리기
```javascript
var o1 = {name:'kim', score:[1,2]}
Object.freeze(o1);
// score의 경우는 nested obj이기 때문에 따로 또 freeze 처리를 해줘야함.
Object.freeze(o1.score);
o1.name = 'lee'; // 반영안됨.
o1.city = 'seoul'; // 반영안됨.
o1.score.push(3); // 반영안됨.
console.log(o1); // 첫 줄의 o1 그대로..
```


### :star: const VS freeze
> const: 객체가 가리키는 곳을 바꾸지 못하게 하는 것
> freeze: 객체의 속성들을 못 바꾸게 하는 것

![freeze-vs-const](https://user-images.githubusercontent.com/39121704/98928018-9a1f8100-251c-11eb-9e65-0632f93e65b6.png)

#### Nested Object
> 객체 안의 객체의 경우

1. 바깥 객체의 복제를 먼저 한다.
2. 내부의 객체는 여전히 기존에 참조하던 메모리를 가리키고 있을 것.
3. 내부의 객체 또한 복제를 통해 새로운 주소를 갖게 하고, 처리해줘야한다.

```javascript
var o1 = {name:'park', score:[1,2]};
var o2 = Object.assign({}, o1);
// case 1 - score 객체가 같은 값을 가리키는 상황.
o2.score.push(3);
console.log(o1===o2, o1.score===o2.score) // 결과: false, true

// case 2 - score 객체도 복사를 해서 서로 다른 값을 가리키는 상황.
o2.score = o2.score.concat();
o2.score.push(3);
console.log(o1===o2, o1.score===o2.score) // 결과: false, false
```


## 불변의 함수 만들기
> JS의 함수는 인자의 타입(원시, 객체)에 따라 동작방법이 달라집니다.

```javascript
function fn(person) {
    person.name = 'lee';
}

var o1 = {name: 'kim'};
// 객체를 함수의 인자로 넘기면 원본이 바뀌어버리므로, 복제본을 사용해준다.
var o2 = Object.assign({},o1);
fn(o2);
console.log(o1,o2); // 결과: kim, lee
```

*출처: https://opentutorials.org/module/4075/24884*
