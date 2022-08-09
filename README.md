# paReactStateMan
为react实现类似vue的状态监听，组件对关心的数据变化进行监听和响应，支持层级状态

### example

组件中对感兴趣的状态进行监听

```ts
//life
    componentDidMount() {
        PaStateMan.regist_comp(this,(registval,state)=>{
            registval(state.course_cur.course_id);//注册需要的状态属性
            registval(state.course_list);
        })
        PaStateMan.getstate().courseProxy().updateCourseList()
    }
    componentWillUnmount() {
        PaStateMan.unregist_comp(this)
    }

```

对状态的操作以及获取通过代理进行，保护数据（即将业务逻辑与界面分开）

```ts
export class PaStateProxy{
    addcnt(){
        this.state.cnt++;
    }
    get cnt(){
        return this.state.cnt
    }
    set cnt(cnt){}
    private coursep;
    courseProxy(){
        return this.coursep
    }
    constructor(private state:PaState) {
        this.coursep=new CourceStoreProxy(this.state)
    }
}
```

全局的pastateman进行动态的管理监听，状态变更时通知程序，以及通过他获取stateproxy

