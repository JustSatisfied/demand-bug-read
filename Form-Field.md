#### Form组件的编写-模型


- 不考虑样式和交互方式 只考虑组件的订阅和通知


传统的模式一般都是
~~~~~
<Form><Field><Input></Input></Field></Form> 
~~~~~

这里就会将定义的Field模型下自己自定义的事件函数进行合并，并且自己会有自己的name，这里的name可以是一个regexp，因为模型里可能会有List的情况  比如列表的情况

首先要明确一个Field组件需要的通用功能 以便后面业务组件进行包装


- 截取子组件的特性属性和事件
- 有自己的唯一name或者ID 
- 维护自己的Meta
- 能具备校验Rule规则

1. 截取子组件的特性属性和事件  我们可以从this.props.children当中拿取到组件的相关属性和事件 
在这里我们需要注意一个trriger的触发方式 一般是onChange 这里需要区分的是originChange和FieldChange 前者是我们写的业务onChange 后者是Field自带，因为我们需要在截取到originChange之后 触发FieldChange 在FieldChange里 我们需要设置Field上Meta属性 比如dirty 还有其他对比value行为，在这里会还会触发到Form上的更新 因为每一个Field都会注册到Form实例上 

2. 每一个Form实例都会去维护name
我们可以把name做成一个链式表达的正则表达  比如 我们会有列表表单 那么就是Form.List.*  那么我们就可以认为这是一个列表表单，甚至可以做成双重嵌套的Field组件 
~~~~
<Field name=List>
  <Field name="a"></Field>
</Field>
///当然这么设计就需要更多的标识符号来表明List是一个嵌套型的组件 而非一个表单组件
~~~~
3. 维护自己的Meta 类似于
~~~~
const Meta={
    ditry:true //组件已经被修改
    touched:true //组件被触发
    error:''//组件的状态
} 
~~~~

4. 能具备校验Rule规则


~~~~
///首先必须是class组件 因为function是不具备this实例

class Field {
    constructor(){

    }
    
    getChild(props){
      const {child}=props;
      
    }
}
~~~~