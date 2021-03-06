





# 8 无状态函数式定义

## 8.1 概述

    无状态函数式组件
        从React 0.14版本开始出现的。
    它是为了创建纯展示组件，
    这种组件只负责根据传入的props来展示，不涉及到要state状态的操作。
    在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；
    这种通过多个简单然后合并成一个大应用的设计模式被提倡。


特点

    1.组件不会被实例化，整体渲染性能得到提升
      因为组件被精简成一个render方法的函数来实现的，
      由于是无状态组件，所以无状态组件就不会在有组件实例化的过程，无实例化过程也就不需要分配多余的内存，从而性能得到一定的提升。
    2.组件不能访问this对象
      无状态组件由于没有实例化过程，所以无法访问组件this中的对象，
    3.组件无法访问生命周期的方法
    因为无状态组件是不需要组件生命周期管理和状态管理，所以底层实现这种形式的组件时是不会实现组件的生命周期方法。所以无状态组件是不能参与组件的各个生命周期管理的。
    4.无状态组件只能访问输入的props，同样的props会得到同样的渲染结果，不会有副作用  



## 特点

组件不会被实例化

    整体渲染性能得到提升
        因为组件被精简成一个render方法的函数来实现的，
        由于是无状态组件，所以无状态组件就不会在有组件实例化的过程，
        无实例化过程也就不需要分配多余的内存，从而性能得到一定的提升。
    组件不能访问this对象
        无状态组件由于没有实例化过程，所以无法访问组件this中的对象，
        例如：this.ref、this.state等均不能访问。若想访问就不能使用这种形式来创建组件
    组件无法访问生命周期的方法
        因为无状态组件是不需要组件生命周期管理和状态管理，
        所以底层实现这种形式的组件时是不会实现组件的生命周期方法。
        所以无状态组件是不能参与组件的各个生命周期管理的。
    无状态组件只能访问输入的props，
        同样的props会得到同样的渲染结果，不会有副作用
    


## 最佳实践

    无状态组件被鼓励在大型项目中尽可能以简单的写法来分割原本庞大的组件，
    未来React也会这种面向无状态组件在譬如无意义的检查和内存分配领域进行一系列优化，
    所以只要有可能，尽量使用无状态组件。
    在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；
    这种通过多个简单然后合并成一个大应用的设计模式被提倡。
    无状态组件的创建形式使代码的可读性更好，并且减少了大量冗余的代码，精简至只有一个render方法，大大的增强了编写一个组件的便利，除此之外无状态组件还有以下几个显著的特点：
 
      
## 8.3 定义

注意
    
    1.组件名开头要大写

ES5

    const MyComponent = function(props){
        return(
            <div>Hello {props.name}</div>;
        );
    }


ES6

    const MyComponent = (props) =>{
        return(
            <div>Hello {props.name}</div>;
        );
    }


## 8.4 调用

    直接使用html的形式调用,用属性传递参数

    ReactDOM.render(<HelloComponent name="Sebastian" />, mountNode)
    

## 8.5 传参
    
props

    1.组件定义时,只定义唯一的形参props
        (此时外部传来的所有参数都将成为props的属性)
    2.使用外部传来的参数,都是用props进行获取
    
    示例
    
        const MyComponent = (props) =>{
            return( <div>Hello {props.name}</div>; );
        }    
    
        <HelloComponent name="Sebastian" />

自定义

    1.组件定义时,指定具体的形参名,并使用大括号包装
    
    示例
    
        const MyComponent = ({name}) =>{
            return( <div>Hello {name}</div>; );
        }    
    
        <HelloComponent name="Sebastian" />    
 