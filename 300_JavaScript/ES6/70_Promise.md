

在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。
通过Promise机制，扁平化的代码机构，大大提高了代码可读性；
用同步编程的方式来编写异步代码，保存线性的代码逻辑，
极大的降低了代码耦合性而提高了程序的可扩展性。

说白了就是用同步的方式去写异步代码。


发起异步请求

    fetch('/api/todos')
      .then(res => res.json())
      .then(data => ({ data }))
      .catch(err => ({ err }));
      
      
      