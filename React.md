1. React自身特点及选型时考虑 

   ```
   React的特点：
   （1）声明式设计
   （2）高效：通过对DOM的模拟，最大限度的减少与DOM的交互。（虚拟DOM）
   （3）灵活：可以与已知的框架或库很好的配合。
   （4）JSX：是js语法的扩展，不一定使用，但建议用。
   （5）组件：构建组件，使代码更容易得到复用，能够很好地应用在大项目的开发中。
   （6）单向响应的数据流：React实现了单向响应的数据流，从而减少了重复代码，这也是解释了它为什么比传统数据绑定更简单。
   
   选型：
   react很大的一个性能优点就是使用了virtual DOM，并不直接操作真实dom，避免了页面多次重绘回流的问题；
   react通过jsx，在js中插入html代码
   ```

   [详情](https://www.cnblogs.com/cxying93/p/6114950.html)

2. React与VUE的异同 

   ```
   相似之处：
   1.都使用了虚拟DOM，Virtual DOM是一个映射真实DOM的JavaScript对象，如果需要改变任何元素的状态，那么是先在Virtual DOM上进行改变，而不是直接改变真实的DOM。当有变化产生时，一个新的Virtual DOM对象会被创建并计算新旧Virtual DOM之间的差别。之后这些差别会应用在真实的DOM上。
   2.React与Vue都鼓励组件化应用,将你的应用分拆成一个个功能明确的模块,每个组件可以通过合适的方式相互联系
   3.React和Vue都有自己的构建工具，你可以使用它快速搭建开发环境。React可以使用Create React App (CRA)，而Vue对应的则是vue-cli
   4.React和Vue都有很好的Chrome扩展工具去帮助你找出bug。
   
   差异：
   1.Vue宣称可以更快地计算出Virtual DOM的差异，这是由于它在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。 而对于React而言，每当应用的状态被改变时，全部子组件都会重新渲染。当然，这可以通过`shouldComponentUpdate`这个生命周期方法来进行控制，但Vue将此视为默认的优化。 
   2.React与Vue最大的不同是模板的编写。vue推荐用template写近似html的模板，但是react使用JSX书写，使用JavaScript而不是模板来开发，赋予了开发者许多编程能力
   3.在react中我们使用state（状态）来获取和改变属性，在React中你需要使用setState()方法去更新状态。但在vue中，我们使用data对象属性进行管理，state在vue中不是必须的。
   ```

   [详情](http://caibaojian.com/vue-vs-react.html)

3. Virtual DOM 

   ```js
   //虚拟dom的定义
   在 React 中，render 执行的结果得到的并不是真正的 DOM 节点，结果仅仅是轻量级的 JavaScript 对象，我们称之为 virtual DOM。（用JavaScript对象模拟dom树）
   
   //虚拟dom的亮点：具有 batching(批处理) 和高效的 Diff 算法
   由虚拟dom来确保只对界面上真正变化的部分进行实际的dom操作
   
   
   //比较 innerHTML 和 Virtual DOM 的重绘过程如下：
   innerHTML: render html string + 重新创建所有 DOM 元素
   Virtual DOM: render Virtual DOM + diff + 必要的 DOM 更新
   //和 DOM 操作比起来，js 计算是非常便宜的
   
   React.js 相对于直接操作原生 DOM 有很大的性能优势， 很大程度上都要归功于 virtual DOM 的 batching 和 diff（对于大型的应用开发而言，如果只是一个简单dom操作，肯定是直接操作原生dom元素效率更高）
   ```

   [详情](http://www.alloyteam.com/2015/10/react-virtual-analysis-of-the-dom/)

4. React生命周期 

   ```
   https://www.jianshu.com/p/b331d0e4b398
   https://zhuanlan.zhihu.com/p/60168527
   ```

5.  Diff算法 

   ```js
   //作用：
   计算出Virtual DOM中真正变化的部分，并只针对该部分进行原生DOM操作，而非重新渲染整个页面。
   
   //三种diff策略
   1.tree diff
   （1）React通过updateDepth对Virtual DOM树进行层级控制。
   （2）对树分层比较，两棵树 只对同一层次节点 进行比较。如果该节点不存在时，则该节点及其子节点会被完全删除，不会再进一步比较。
   （3）只需遍历一次，就能完成整棵DOM树的比较。
   //如果是跨层级的话，只有创建节点和删除节点的操作。
   
   2.component diff
   如果是同一类型的组件，按照原策略继续比较 virtual DOM tree。
   如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点
   对于同一类型的组件，有可能其 Virtual DOM 没有任何变化，如果能够确切的知道这点那可以节省大量的 diff 运算时间，因此 React 允许用户通过 shouldComponentUpdate() 来判断该组件是否需要进行 diff。
   
   3.element diff
   当节点处于同一层级时，React diff 提供了三种节点操作，分别为：INSERT_MARKUP（插入）、MOVE_EXISTING（移动）和REMOVE_NODE（删除）。
   
   //总结：
   -React 通过制定大胆的 diff 策略，将 O(n3) 复杂度的问题转换成 O(n) 复杂度的问题；
   -React 通过分层求异的策略，对 tree diff 进行算法优化；
   -React 通过相同类生成相似树形结构，不同类生成不同树形结构的策略，对 component diff 进行算法优化；
   -React 通过设置唯一 key的策略，对 element diff 进行算法优化；
   -建议，在开发组件时，保持稳定的 DOM 结构会有助于性能的提升；
   -建议，在开发过程中，尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。
   ```

   [详细](https://zhuanlan.zhihu.com/p/103187276)

6.  受控组件与非受控组件 （[写的非常好--1](https://www.jianshu.com/p/f83e961b4d73)   [写的非常好-2](https://blog.csdn.net/qq_41846861/article/details/86598797)）

   ```js
   //受控组件
   1、每当表单的状态发生变化时，都会被写入到组件的state中
   2、在受控组件中，组件渲染出的状态与它的value或checked prop相对应
   3.使用受控组件需要为每一个组件绑定一个change事件,并且定义一个事件处理器来同步表单值和组件的状态,某些情况可以使用一个事件处理器来处理多个表单域（bind方法）
   4.在受控组件上指定 value 的 prop 可以防止用户更改输入。(如果指定了 value，但输入仍可编辑，则可能是意外地将value 设置为 undefined 或 null)
   
   //非受控组件
   1、如果一个表单组件没有value prop就可以称为非受控组件
   2、非受控组件是一种反模式，它的值不受组件自身的state或props控制
   3、通常需要为其添加ref prop来访问渲染后的底层DOM元素
   ```

7. 高阶组件（HOC） （建议看官方文档的解释）

   ```js
   高阶组件是参数为组件，返回值为新组件的函数。
   const EnhancedComponent = higherOrderComponent(WrappedComponent);
   组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件。
   
   详细文档与例子：https://react.docschina.org/docs/higher-order-components.html
   ```

8.  Flux架构模式（涉及MVC/MVVM、Flux） 

   ```
   
   ```

9.  Redux设计概念、设计原则、方法、redux实现异步流的库 

   ```
   
   ```

10. 纯组件（Pure Component）与shouldComponentUpdate关系 

    ```
    
    ```

11. Redux中的<Provider/>组件与connect函数 

    ```
    
    ```

12.  React Fiber架构 

    ```
    
    ```

13.  React Hooks的作用及原理

    ```
    
    ```

    

