

filter也是一个常用的操作，它用于把Array的某些元素过滤掉，然后返回剩下的元素。

和map()类似，Array的filter()也接收一个函数。
和map()不同的是，filter()把传入的函数依次作用于每个元素，
然后根据返回值是true还是false决定保留还是丢弃该元素。


示例 

    例如，在一个Array中，删掉偶数，只保留奇数，可以这么写：
    
    var arr = [1, 2, 4, 5, 6, 9, 10, 15];
    var r = arr.filter(function (x) {
        return x % 2 !== 0;
    });
    r; // [1, 5, 9, 15]
    
    回调函数
    filter()接收的回调函数，其实可以有多个参数。通常我们仅使用第一个参数，表示Array的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身：
    
    利用filter，可以巧妙地去除Array的重复元素
    
    r = arr.filter(function (element, index, self) {
        return self.indexOf(element) === index;
    });
