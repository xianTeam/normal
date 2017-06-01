##react编码规范
2016年8月2日 curry


##1、Basic Rules 基本规范

* 每个文件只写一个模块.
    *  但是多个无状态模块可以放在单个文件中
* 推荐使用JSX语法.
* 不要使用 React.createElement，除非从一个非JSX的文件中初始化你的app.

##2、创建模块

* Class vs React.createClass vs stateless
    * 如果你的模块有内部状态或者是refs, 推荐使用 class extends React.Component 而不是 React.createClass ,除非你有充足的理由来使用这些方法.

```javascript
  // bad
  const Listing = React.createClass({
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  });

  // good
  class Listing extends React.Component {
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  }
```
* 如果你的模块没有状态或是没有引用refs， 推荐使用普通函数（非箭头函数）而不是类:

```javascript
  // bad
  class Listing extends React.Component {
    render() {
      return <div>{this.props.hello}</div>;
    }
  }

  // bad (relying on function name inference is discouraged)
  const Listing = ({ hello }) => (
    <div>{hello}</div>
  );

  // good
  function Listing({ hello }) {
    return <div>{hello}</div>;
  }
```

##Naming命名
* 扩展名: React模块使用 .jsx 扩展名.
* 文件名: 文件名使用驼峰式. 如, ReservationCard.jsx.
* 引用命名: React模块名使用驼峰式命名，实例使用骆驼式命名

```javascript
// bad
import reservationCard from './ReservationCard';

// good
import ReservationCard from './ReservationCard';

// bad
const ReservationItem = <ReservationCard />;

// good
const reservationItem = <ReservationCard />;
```
* 模块命名: 模块使用当前文件名一样的名称. 比如 ReservationCard.jsx 应该包含名为 ReservationCard的模块. 但是，如果整个文件夹是一个模块，使用 index.js作为入口文件，然后直接使用 index.js 或者文件夹名作为模块的名称:

```javascript
// bad
import Footer from './Footer/Footer';

// bad
import Footer from './Footer/index';

// good
import Footer from './Footer';
```

##Declaration

* 不要使用 displayName 来命名React模块，而是使用引用来命名模块， 如 class 名称.

```javascript
// bad
export default React.createClass({
  displayName: 'ReservationCard',
  // stuff goes here
});

// good
export default class ReservationCard extends React.Component {
}
```
##Alignment代码对其
* 遵循以下的JSX语法缩进/格式.

```javascript
// bad
<Foo superLongParam="bar"
     anotherSuperLongParam="baz" />

// good, 有多行属性的话, 新建一行关闭标签
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
/>

// 若能在一行中显示, 直接写成一行
<Foo bar="bar" />

// 子元素按照常规方式缩进
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
>
  <Quux />
</Foo>
```
##Quotes单引号还是双引号
* 对于JSX属性值总是使用双引号("), 其他均使用单引号. 

```javascript
  // bad
  <Foo bar='bar' />

  // good
  <Foo bar="bar" />

  // bad
  <Foo style={{ left: "20px" }} />

  // good
  <Foo style={{ left: '20px' }} />
```

##Spacing空格
* 总是在自动关闭的标签前加一个空格，正常情况下也不需要换行.

```javascript
// bad
<Foo/>

// very bad
<Foo                 />

// bad
<Foo
 />

// good
<Foo />
```
* 不要在JSX {} 引用括号里两边加空格.

```javascript
// bad
<Foo bar={ baz } />

// good
<Foo bar={baz} />
```
##Props 属性
* JSX属性名使用骆驼式风格camelCase.

```javascript
// bad
<Foo
  UserName="hello"
  phone_number={12345678}
/>

// good
<Foo
  userName="hello"
  phoneNumber={12345678}
/>
```
* 如果属性值为 true, 可以直接省略. 

```javascript
// bad
<Foo
  hidden={true}
/>

// good
<Foo
  hidden
/>
```

* <img> 标签总是添加 alt 属性. 如果图片以presentation(感觉是以类似PPT方式显示?)方式显示，alt 可为空, 或者<img> 要包含role="presentation". 

```javascript
// bad
<img src="hello.jpg" />

// good
<img src="hello.jpg" alt="Me waving hello" />

// good
<img src="hello.jpg" alt="" />

// good
<img src="hello.jpg" role="presentation" />
```
* 避免使用数组的index来作为属性key的值，推荐使用唯一ID. 

```javascript
// bad
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}

// good
{todos.map(todo => (
  <Todo
    {...todo}
    key={todo.id}
  />
))}
```

##Parentheses 括号

* 将多行的JSX标签写在 ()里.

```javascript
// bad
render() {
  return <MyComponent className="long body" foo="bar">
           <MyChild />
         </MyComponent>;
}

// good
render() {
  return (
    <MyComponent className="long body" foo="bar">
      <MyChild />
    </MyComponent>
  );
}

// good, 单行可以不需要
render() {
  const body = <div>hello</div>;
  return <MyComponent>{body}</MyComponent>;
}
```

##Tags 标签
* 对于没有子元素的标签来说总是自己关闭标签.

```javascript
// bad
<Foo className="stuff"></Foo>

// good
<Foo className="stuff" />
```
* 如果模块有多行的属性， 关闭标签时新建一行.

```javascript
// bad
<Foo
  bar="bar"
  baz="baz" />

// good
<Foo
  bar="bar"
  baz="baz"
/>
```
##Methods 函数
* 使用箭头函数来获取本地变量.

```javascript
function ItemList(props) {
  return (
    <ul>
      {props.items.map((item, index) => (
        <Item
          key={item.key}
          onClick={() => doSomethingWith(item.name, index)}
        />
      ))}
    </ul>
  );
}
```
* 当在 render() 里使用事件处理方法时，提前在构造函数里把 this 绑定上去.

```javascript
 // bad
  class extends React.Component {
    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv.bind(this)} />
    }
  }

  // good
  class extends React.Component {
    constructor(props) {
      super(props);

      this.onClickDiv = this.onClickDiv.bind(this);
    }

    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv} />
    }
  }
```

##Ordering React 模块生命周期
* class extends React.Component 的生命周期函数:
    * 可选的 static 方法
    * constructor 构造函数
    * getChildContext 获取子元素内容
    * componentWillMount 模块渲染前
    * componentDidMount 模块渲染后
    * componentWillReceiveProps 模块将接受新的数据
    * shouldComponentUpdate 判断模块需不需要重新渲染
    * componentWillUpdate 上面的方法返回 true， 模块将重新渲染
    * componentDidUpdate 模块渲染结束
    * componentWillUnmount 模块将从DOM中清除, 做一些清理任务
    * 点击回调或者事件处理器 如 onClickSubmit() 或 onChangeDescription()
    * render 里的 getter 方法 如 getSelectReason() 或 getFooterContent()
    * 可选的 render 方法 如 renderNavigation() 或 renderProfilePicture()
    * render render() 方法
* 如何定义 propTypes, defaultProps, contextTypes, 等等其他属性...

```javascript
import React, { PropTypes } from 'react';

const propTypes = {
  id: PropTypes.number.isRequired,
  url: PropTypes.string.isRequired,
  text: PropTypes.string,
};

const defaultProps = {
  text: 'Hello World',
};

class Link extends React.Component {
  static methodsAreOk() {
    return true;
  }

  render() {
    return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
  }
}

Link.propTypes = propTypes;
Link.defaultProps = defaultProps;

export default Link;
```
