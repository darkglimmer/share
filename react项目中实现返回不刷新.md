## react项目中实现返回不刷新

react在切换组件时，前一个组件会经历完整的组件销毁过程，后一个组件会经历完整的组件加载过程。

因此当从详情页返回到列表页的时候会发现列表页重新渲染回到了初始的状态，并不是一个合理的用户体验。这个问题只有单页应用会面临，因为对于多标签页的应用来说，每个标签页的内容互不影响。

### 实现思路

- 存储的选择

localStorage

- 保存的时机

react生命周期中有一个回调<code>componentWillUnmount()</code>，它在组件卸载之前调用，我们可以在这里保存所有状态以备后用。

- 如何保存

react-router默认基于浏览器hash进行路由，例如这样的url代表一次列表页访问：http://localhost:8080/#/?_k=1j7gct，其中/#/?_k=1j7gct叫做一个location，react-router会帮我们维护一个location的栈结构叫做history，代表了我们此前的访问路径。

那么_k=xxx是什么东西呢？当我们面临：从组件自身跳转到组件自身的访问路径时，history里就变成了这样[/#, /#]，没法标识2个location的差异，因此react-router会帮我们给每个location加上一个全局唯一的随机码_k。

假设这样一个访问路径，首先进入列表页/#/?_k=1j7gct（history=[/#/?_k=1j7gc]），之后跳转到详情页/#/msg-detail-page/18?_k=nuqncq（history=[/#/?_k=1j7gc,/#/msg-detail-page/18?_k=nuqncq]）。此时，我们后退到列表页，其实就是从history里pop出/#/msg-detail-page/18?_k=nuqncq，因此我们可以知道列表页地址仍旧保持在/#/?_k=1j7gc，因此_k=1j7gc就顺理成章的成为了我们作为缓存key的标识了。

### 实现代码
+ 备份数据

```
    componentWillUnmount() {
        // 备份当前的页面状态
        if (!this.state.isLoading) {
            let data = {
                items: this.state.items,
                page: this.page,
                y: this.iScrollInstance.y,
            };
            window.sessionStorage.setItem(this.props.location.key, JSON.stringify(data));
        } else {
            window.sessionStorage.removeItem(this.props.location.key);
        }
    }
```

+ 恢复数据

```
export default class MsgListPage extends React.Component {
    constructor(props, context) {
        super(props, context);

        // 尝试加载备份的数据
        this.tryRestoreComponent();
        this.itemsChanged = false;  // 本次渲染是否发生了文章列表变化，决定iscroll的refresh调用
        this.isTouching = false; // 是否在触屏中
```
在这里调用tryRestoreComponent()，它尝试从sessionStorage里恢复数据：

```
    tryRestoreComponent() {
        let data = window.sessionStorage.getItem(this.props.location.key);

        // 恢复之前状态
        if (data) {
            data = JSON.parse(data);
            this.state = {
                items: data.items,
                pullDownStatus: 0,  // 下拉状态
                pullUpStatus: 0,    // 上拉状态
                isLoading: false,   // 是否处于首屏加载中
            };
            this.page = data.page;
        } else {
            this.state = {
                items: [],          // 文章列表
                pullDownStatus: 3,  // 下拉状态
                pullUpStatus: 0,    // 上拉状态
                isLoading: true,    // 是否处于首屏加载中
            };
            this.page = 1;  // 当前翻页
        }
    }
```
### [redux](http://cn.redux.js.org/docs/introduction/CoreConcepts.html)

