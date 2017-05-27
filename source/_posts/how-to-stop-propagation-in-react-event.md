---
title: React之如何阻止事件冒泡
date: 2017-05-27 18:48:19
tags:
  - javascript
  - react
---

#### React的事件机制

---

在React中，事件并不会绑定在具体的某个元素上，而是通过事件代理的方式，在根节点document上为每种类型的事件添加唯一的Listener，再通过事件的target来找到具体的触发元素。接着由下而上，所有绑定该类型事件的节点都会触发对应的handler。React也正是通过这种方式来模拟着原生事件，就如官网所说： <!-- more -->

> React defines these synthetic events according to the W3C spec, so you don't need to worry about cross-browser compatibility.

React的事件不会有跨平台的兼容性问题，完全符合w3c规范，也就是说原生事件的冒泡, 捕获，stopPropagation,preventDefault等等，在React中都是支持的。

也就是说，下面代码中，点击Child组件，控制台理应是没有输出的。
{% codeblock lang:javascript %}
<Parent onClick={ () => { console.log("parent"); } }>
    <Child onClick={ (e) => { e.stopPropagation(); } }></Child>
</Parent>
{% endcodeblock %}

#### 为什么e.stopPropagation不起作用

---


上面已经讲过react中的事件是绑定在根节点，且经过了封装和模拟的，所以当你想在react事件中使用e.stopPropagation来阻止原生事件其实是行不通的。

就如下面的例子：

{% codeblock lang:javascript %}
class Demo extends React.Component {
  render(){
    return (
      <div id="parent">
        <div id="child" onClick={(e) => {e.stopPropagation();}}>快来点我</div>
      </div>
     )
  }
}

const element = (
  <Demo></Demo>
);

ReactDOM.render(element, document.getElementById("root"));

$("#parent").on("click", function(e) {
  console.log("你挡不住我");
});
{% endcodeblock %}

使用jquery为parent绑定了click事件后。 想要在child的handler中使用e.stopProgapation来阻止click事件的冒泡，是徒劳的。无论如何点击child区域，控制台还是会输出"你挡不住我"。

#### 如何阻止事件的冒泡

---

通过在**componentDidMount**中来手动绑定事件，来阻止事件的冒泡。

{% codeblock lang:javascript %}
// 利用ReactDOM
componentDidMount() {
  ReactDOM.findDOMNode(this._child).addEventListener('click', (e) => {
    e.stopPropagation();
  }, false);
}

// 利用jquery
componentDidMount() {
  $("#child").on("click", (e) => {
      e.stopPropagation();
  });
}
{% endcodeblock %}

再来看看具体的例子，[点我看效果](https://codepen.io/TimLuo465/pen/gWyeBW?editors=0011)