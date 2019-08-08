---
title: 자바스크립트 언락 도서 요약
toc: true
date: 2019-07-22 14:28:21
tags: 
    - Book
categories:
    - Book
---

# 자바스크립트 언락 (2017년 발행)

## 새롭게 알게된 사실들

### **조건부 호출**

조건문 간략하게 작성
```js
const age = 20
age >= 18 && console.log(`나이가 18세 이상이구나!`)
age >= 18 || console.log(`나이가 18세 미만이구나...`)
```

함수 호출 단축 조건

```js
function fn(cb) {
    cb && cb()
}

// 또는

class AbstractFoo {
    constructor() {
       // init 메서드가 있을경우에만 호출 
        this.init && this.init()
    }
}
```

### **나머지 연산자**

ES5는 작성하지 않음(`[].slice.call(arguments)` 찾아보아요)

ES6

```js
let cb = (foo, bar, ...args) ={
    console.log(foo, bar, args)
}
cb(`foo`, `bar`, 1, 2, 3) // foo bar [1,2,3]
```

변수 나머지 연산(선언이 특이하네)

```js
let [bar, ...others] = [`bar`, `foo`, `baz`, `qux`]
console.log([bar, others]) // [`bar`, [`foo`, `baz`, `qux`]]
```

### **펼침 연산자**

배열 요소를 인수로 확산

```js
let args = [2015, 6, 17]
let relDate = new Date(...args)
console.log(relDate.toString()) // Fri Jul 17 2015 00:00:00 GMT+0900 (한국 표준시)
```

### **ES6 컬렉션**

객체 중복없이 넣기

```js
let foo = new Set()
foo.add(1)
foo.add(1)
foo.add(2)
console.log(Array.from(foo)) // [1, 2]
```

### **Map vs Set**

비교
`Objects`의 키는 `Strings`이며, `Map`은 모든 값을 가질 수 있음
`Objects`의 사이즈는 수동으로 체크해야하지만, `Map`은 쉽게 얻을 수 있음
`Map`은 삽입된 순서대로 반복됨

사용팁
실행시까지, 키를 알 수 없고 모든 키·값이 동일한 타입이면 `Map`을 사용
각 개별 요소에 대한 적용이 있을 경우는 `Object`를 사용

참고자료
[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Keyed_collections)
[Javascript Info](http://javascript.info/map-set)

### **getter/setter (알려진 속성에 대한 접근제어)**

SyntaxError 객체가 흥미롭네

```js
class Bar {
    constructor() {
        this.arr = [1, 2]
    }
    
    get length() {
        return this.arr.length
    }
    
    set length(val) {
        throw new SyntaxError(`Cannot assign to read only property 'length'`)
    }
}
```

### **proxy (임의 속성에 대한 접근 제어)**

오오... proxy, IE11 적용법 찾긴 해야되는데...
아직 이해도가 낮은 것 같다.(★★★)

```js
const myStorage = {
    data: {},
    
    getItem(key) {
        return this.data[key]
    },
    
    setItem(key, val) {
        this.data[key] = val
    }
}

let storage = new Proxy(myStorage, {
    get(storage, key) {
        return storage.getItem(key)
    },
    
    set(storage, key, val) {
    	return storage.setItem(key, val)
	}
})

storage.bar = `bar`
console.log(myStorage.getItem(`bar`))
myStorage.setItem(`bar`, `baz`)
console.log(storage.bar)
```

### **NodeList에 관하여**

라이브 콜렉션으로 상당한 비싼 자원(DOM이 `reflow`될때마다 갱신)
그러므로, 라이브일 필요가 없을 경우는 배열로 변환

```js
const li = document.querySelectorAll(`li`)
console.log(li.slice.call(li)) // ES5 방식
console.log([...li]) // ES6 방식
```

### **DOM 이벤트 핸들링**

Event 객체 상세사항

`event.type`: 이벤트 이름

`event.target`: 발생한 이벤트 타겟

`event.currentTarget`: 리스너가 부착된 타겟

`event.eventPhase` : 이벤트 단계 (`none`, `capturing`, `at target`, `bublling`)

`event.bublles`: 이벤트 버블 여부

`event.cancelable`: 이벤트 디폴트 동작 방지 여부

`event.timeStamp`: 이벤트 시간 지정

**메서드**

`event.stopPropagation()`: 추가전파 중지

`event.stopImmediatePropagation()`: 다른 이벤트 호출X

`event.preventDefault()`: 디폴트 동작 방지 (ex. submit)

### **XHR vs Axios**

fetch는 취소 불가, 다른 브라우저 호환성, 인코딩 문제(★)로 별로 쓰고싶지 않다.

[Fetch API](https://flaviocopes.com/fetch-api/)보다 Axios가 더 좋은 장점은 아래와 같습니다.([출저](https://tuhbm.github.io/2019/03/21/axios/))

1. 구형브라우저를 지원합니다.(Fetch API의 경우는 [폴리필](http://hacks.mozilla.or.kr/2014/12/an-easier-way-of-using-polyfills/)이 필요합니다.)
2. 요청을 중단시킬 수 있습니다.
3. 응답 시간 초과를 설정하는 방법이 있습니다.
4. [CSRF](https://laravel.kr/docs/5.5/csrf) 보호 기능이 내장되어있다.
5. JSON 데이터 자동변환
6. Node.js에서의 사용

### **웹 스토리지 API**

쿠키의 단점으로 생겨남.

​	단점1. 최대크기 4KB

​	단점2. `Cross-site-scripting-acttack`에 취약함

웹 스토리지 API는 5~25MB를 저장, HTTP 요청헤더에 어떠한 데이터도 추가하지 않음

​	종류1. `localStorage` : 다른 탭과 공유, 영구히 유지

​	종류2. `sessionStorage` : 하나의 탭, 세션 동안에만 유지

사용 예제

```js
function useStorage() {
    if (!localStorage) {
        return
    }
    let storage = localStorage
    storage.setItem(`foo`, `Foo`)
    console.log(storage.getItem(`foo`))
    storage.removeItem(`foo`)
}
useStorage()
```

또는 (getter/setter 사용)

```js
function useStorage() {
    if (!localStorage) {
        return
    }
    let storage = localStorage
    storage.foo = `food`
    console.log(storage.foo)
    delete storage.foo
}
useStorage()
```

반복문 사용시

```js
let len = storage.length
let key
for(let i = 0 ;i < len; i++) {
    key = storage.key(i)
    console.log(storage.key)
}
```



------------------

*여기서부터 정리안됨, 이후 다시 책봐서 정리...*

## Web Storage 예제 (쇼핑카트)

<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

```html
<html>
  <head>
    <title>Web Storage</title>
  </head>
  <body>
    <div>
      <button data-bind="btn">Add to cart</button>
      <button data-bind="reset">Reset</button>
    </div>
    <output data-bind="output">

    </output>
    <script>

    var output = document.querySelector( "[data-bind=\"output\"]" ),
        btn = document.querySelector( "[data-bind=\"btn\"]" ),
        reset = document.querySelector( "[data-bind=\"reset\"]" ),
        storage = localStorage,
       /**
        * Read from the storage
        * @return {Arrays}
        */
        get = function(){
           return JSON.parse( storage.getItem( "cart" ) ) || [];
        },
        /**
         * Append an item to the cart
         * @param {Object} product
         */
        append = function( product ) {
          var data = get();
          data.push( product );
          storage.setItem( "cart", JSON.stringify( data ) );
        },
        /** Re-render list of items */
        updateView = function(){
          var data = get();
          output.innerHTML = "";
          data && data.forEach(function( item ){
            output.innerHTML += [ "id: ", item.id, "<br />" ].join( "" );
          });
        };

    this.btn.addEventListener( "click", function(){
      append({ id: Math.floor(( Math.random() * 100 ) + 1 ) });
      updateView();
    }, false );

    this.reset.addEventListener( "click", function(){
      storage.clear();
      updateView();
    }, false );

    // Update item list when a new item is added in another window/tab
    window.addEventListener( "storage", updateView, false );

    updateView();

    </script>
  </body>
</html>
```

</div>
</details>

## 인덱스 DB 예제

<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

```js

/**
 * IndexDB usage example
 */

/**
 * @type {IDBOpenDBRequest}
 */
var request = indexedDB.open( "Cem", 2 );

/** Report error */
request.onerror = function() {
  alert( "Opps, something went wrong" );
};
/**
 * Create DB
 * @param {Event} e
 */
request.onupgradeneeded = function ( e ) {
  var objectStore;
  if ( e.oldVersion ) {
    return;
  }
  // define schema
  objectStore = e.currentTarget.result.createObjectStore( "employees", { keyPath: "email" });
  objectStore.createIndex( "name", "name", { unique: false } );
   // Populate objectStore with test data
  objectStore.add({ name: "John Dow", email: "john@company.com" });
  objectStore.add({ name: "Don Dow", email: "don@company.com" });
};
/**
 * Find a row from the DB
 * @param {Event} e
 */
request.onsuccess = function( e ) {
  var db = e.target.result,
      req = db.transaction([ "employees" ]).objectStore( "employees" ).get( "don@company.com" );

  req.onsuccess = function() {
    console.log( "Employee matching `don@company.com` is `" + req.result.name + "`" );
  };
};
```

</div>
</details>

-------------




<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

```js

/**
 * IndexDB with Dexie 
 */
var db = new Dexie( "Cem" );
// Define DB
db.version( 3 )
  .stores({ employees: "name, email" });

// Open the database
db.open().catch(function( err ){
  alert( "Opps, something went wrong: " + err );
});

// Populate objectStore with test data
db.employees.add({ name: "John Dow", email: "john@company.com" });
db.employees.add({ name: "Don Dow", email: "don@company.com" });

// Find an employee by email
db.employees
  .where( "email" )
  .equals( "don@company.com" )
  .each(function( employee ){
    console.log( "Employee matching `don@company.com` is `" + employee.name + "`" );
  });
```

</div>
</details>


## 파일 시스템 API 예제

<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

```js
/**
 * FileSystem API usage example
 */
window.requestFileSystem  = window.requestFileSystem || window.webkitRequestFileSystem;
    /**
     * Read file from a given FileSystem
     * @param {DOMFileSystem} fs
     * @param {String} file
     */
var readFile = function( fs, file ) {
      console.log( "Reading file " + file );
      // Obtain FileEntry object
      fs.root.getFile( file, {}, function( fileEntry ) {
        fileEntry.file(function( file ){
           // Create FileReader
           var reader = new FileReader();
           reader.onloadend = function() {
             console.log( "Fetched content: ", this.result );
           };
           // Read file
           reader.readAsText( file );
        }, console.error );
      }, console.error );
    },
    /**
     * Save file into a given FileSystem and run onDone when ready
     * @param {DOMFileSystem} fs
     * @param {String} file
     * @param {Function} onDone
     */
    saveFile = function( fs, file, onDone ) {
      console.log( "Writing file " + file );
      // Obtain FileEntry object
      fs.root.getFile( file, { create: true }, function( fileEntry ) {
        // Create a FileWriter object for the FileEntry
        fileEntry.createWriter(function( fileWriter ) {
          var blob;
          fileWriter.onwriteend = onDone;
          fileWriter.onerror = function(e) {
            console.error( "Writing error: " + e.toString() );
          };
          // Create a new Blob out of the text we want into the file.
          blob = new Blob([ "Lorem Ipsum" ], { type: "text/plain" });
          // Write into the file
          fileWriter.write( blob );
        }, console.error );
      }, console.error );
    },
    /**
     * Run when FileSystem initialized
     * @param {DOMFileSystem} fs
     */
    onInitFs = function ( fs ) {
      const FILENAME = "log.txt";
      console.log( "Opening file system: " + fs.name );
      saveFile( fs, FILENAME, function(){
        readFile( fs, FILENAME );
      });
    };

window.requestFileSystem( window.TEMPORARY, 5*1024*1024 /*5MB*/, onInitFs, console.error );
```



</div>
</details>



-------------

*참고파일 참조*

## 웹 워커 멀티스레드 예제

## 서버 - 브라우저 간 통신 채널 예제

## 웹소켓 예제

**비동기 자바스크립트(나중에)**

## 자바스크립트 디자인 패턴

**NW.js, 폰갭 생략**

## 디버깅

## 참고파일
{% raw %}
<a href ='/assets/JavaScript Unlocked_code.zip' download>예제 코드 Download</a>
{% endraw %}

