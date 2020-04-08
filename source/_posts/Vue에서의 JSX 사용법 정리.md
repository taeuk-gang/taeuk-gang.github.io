---
title: Vue JSX 사용법
toc: true
date: 2020-04-08 20:39:05
tags: 
    - Vue.js
categories: 
    - Vue.js
---

# Vue에서의 JSX 사용법 정리

> Vue 공식 홈페이지에서 JSX에 대한 사용법이 자세히 명시되어있지 않아 정리함
>
> [이 사이트](https://segmentfault.com/a/1190000019659205)를 중심으로 재정리함 (중국어를 하나도 몰라서, 번역은 아님)



## 개인적 코드 스타일

```vue
<template>
  <div>
    <ChildElement />
  </div>
</template>

<script lang="tsx">
import { Vue, Component } from 'vue-property-decorator';
import { VNode } from 'vue';

type ChildComponent = any;

// # 메뉴바
const ChildElement: ChildComponent = {
	render(): VNode {
		return (
			<div>Test</div>
		);
	},
};

@Component({
	components: {
		ChildComponent,
	},
})
export default class Test extends Vue {

}


<style scoped></style>
```



## JSX에서의 데이터 교류

```vue
<span>Message: {this.messsage}</span>
<!-- v-html -->
<div domPropsInnerHTML={this.dangerHtml}/>
<!-- v-model -->
<custom-input v-model={this.vm.name} />
```

![image-20200408202725336](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200408202725336.png)



## 동적 지정이 가능

```vue
<!-- 일반적인 형태 -->
<div class="btn btn-default" style="font-size: 12px;">Button</div>

<!-- JSX에서의 형태(동적 지정 가능) -->
<div class={`btn btn-${this.isDefault ? 'default' : ''}`}></div>
<div class={{'btn-default': this.isDefault, 'btn-primary': this.isPrimary}}></div>
<div style={{color: 'red', fontSize: '14px'}}></div>
```



## 일반적인 Template 방식과 다름

> 사실 거의 javascript 위에서 HTML을 작성한다고 보면 됨

```vue
{/* v-if */}
{this.withTitle && <Title />}

{/* v-if 加 v-else */}
{this.isSubTitle ? <SubTitle /> : <Title />}

{/* v-for */}
{this.options.map(option => {
  <div>{option.title}</div>
})}
```



## 이벤트

```vue
<!-- 일반 이벤트(@event와 동일) -->
<custom-buton onClick={this.handleClick}>Click me</el-buton>
<!-- 네이티브 이벤트 -->
<custom-button nativeOnClick={this.handleClick}>Native click</el-button>
<!-- 파라미터 이용시 -->
<custom-button onClick={e => this.handleClick(this.id)}>Click and pass data</el-button>
```



## 작성법

하나의 엘리먼트로 래핑하여 작성

```vue
const Demo = () => (
	<div>
    	<li>One</li>
	    <li>Two</li>
	</div>
)
```

여러개 작성해야할 필요가 있을 경우

```vue
const Demo = () => [
  <li>One</li>
  <li>Two</li>
]
```

Map을 이용한 반복 작성

```vue
{
  data() {
    return {
      options: ['one', 'two']
    }
  },
    
    render() {
    const LiItem = () => this.options.map(option => <li>{option}</li>)
                                          
    return (
      <div>
            <ul>
              <LiItem />
          </ul>
         </div>
    )
  }
}
```



## 이벤트 capture, passive, once, ...

| 이벤트 키워드 | 기호 |
| ------------- | ---- |
| .passive      | &    |
| .capture      | !    |
| .once         | ~    |
| .capture.once | ~!   |

```vue
<el-button {...{
    '!click': this.doThisInCapturingMode,
  	'!keyup': this.doThisOnce,
  	'~!mouseover': this.doThisOnceInCapturingMode
}}>Click Me!</el-button>
```



## 이벤트 전파

| 이벤트 키워드                | 설명                                                      |
| ---------------------------- | --------------------------------------------------------- |
| .stop                        | event.stopPropagation()                                   |
| .prevent                     | event.preventDeafult()                                    |
| .self                        | if (event.target !== event.currentTarget) return          |
| .enter (.13)                 | if (event.keyCode !== 13) return                          |
| .ctrl  .alt   .shift   .meta | if (!event.ctrlKey) return<br />altKey, shiftKey, metaKey |

```vue
methods: {
  keyup(e) {
    // .self
    if (e.target !== e.currentTarget) return
    
    // .enter` .13
    if (!e.shiftKey || e.keyCode !== 13) return
    
    // .stop
    e.stopPropagation()
    
    // .prevent
    e.preventDefault()
    
    // ...
  }
}
```



## v-for

```vue
const LiArray = () => this.options.map(option => (
  <li ref="li" key={option}>{option}</li>
))
```



```vue
const LiArray = () => this.options.map(option => (
  <li ref="li" refInFor={true} key={option}>{option}</li>
))
```



## v-slot

```vue
<div class="page-header__title">
    {this.$slots.title ? this.$slots.title : this.title}
</div>
```

(동일 기능) Template에서는 아래와 같다

```vue
<div class="page-header__title">
  <slot name="title">{{ title }}</slot>
</div>
```

JSX에서는 default상태의 슬롯은 허용되지 않는다.

```vue
<!-- 에러! -->
<current-user>
    {{ user.firstName }}
</current-user>
```



### JSX에서는 스코프 슬롯을 사용해야함

#### 일반적인 Vue slot 형태

부모

```vue
<current-user>
    <template v-slot:default="{ injectedProps }">
      <div>{{ injectedProps.user.firstName }}</div>
        <el-button @click="injectedProps.logFullName">Log Full Name</el-button>
  </template>
</current-user>
```

자식

```vue
<template>
    <div>
    <slot v-bind:injectedProps="slotProps">
      {{ user.lastName }}
      </slot>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        user: {
          firstName: 'snow',
          lastName: 'wolf'
        }
      }
    },
    
    computed: {
      slotProps() {
        return {
          user: this.user,
          logFullName: this.logFullName
        }
      }
    },
    
    methods: {
      logFullName() {
        console.log(`${this.firstName} ${this.lastName}`)
      }
    }
  }
</script>
```

#### JSX에서 slot 형태

부모

```vue
<current-user {...{
  scopedSlots: {
    subTitle: ({ injectedProps }) => (
        <div>
          <h3>injectedProps.user</h3>
        <el-button onClick={injectedProps.logFullName}>Log Full Name</el-button>
      </div>
    )
  }
}}></current-user>
```

자식

```vue
{
  data() {
    return {
      user: {
        firstName: 'snow',
        lastName: 'wolf'
      }
    }
  },
    
  computed: {
    slotProps() {
      return {
        user: this.user,
        logFullName: this.logFullName
      }
    }
  },
    
  methods: {
    logFullName() {
      console.log(`${this.firstName} ${this.lastName}`)
    }
  },
    
  render() {
    return (
        <div>
        {this.$scopedSlots.subTitle({
          injectedProps: this.slotProps
        })}
      </div>
    )
  }
}
```

----------

중간 나중에 정리 (현재 불필요 내용)

--------

## 이상적인 나누는 구조

```vue
render() {
  const TabHeader = (
      <div class="page-header page-header--tab"></div>
  )
  
  const Header = () => (
      <div class="page-header"></div>
  )
  
  <div class="page-header">
      {this.withTab ? TabHeader : <Header/>}
  </div>
}
```



## VueComponent 구조

```plain
children        
data         
    attrs      
    domProps    
    on                
injections  
listeners:  
    click        
    ...
parent            
props                
scopedSlots 
slots                
```



## Props 이용법

```vue
render() {
  const Demo = props => {
    return (
        <div>
          <h3>Jsx中的内部组件 { props.data.title }</h3>
        { props.children }
        <br />
        { props.scopedSlots.bar() }
      </div>
    )
  }
  
  return (
      <div>
        <Demo title="test" attrsA="a" domPropsB="b" onClick={this.demo}>
          <h3>Children</h3>
        <template slot="bar">
            <p>Slot</p>
        </template>
      </Demo>
    </div>
  )
}
```