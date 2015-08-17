###jquery 使用积累

####jquery.each()

jQuery.each( collection, callback(indexInArray, valueOfElement) )

描述: 一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1。其他对象通过其属性名进行迭代。
	
$.each()函数和 $(selector).each()是不一样的，那个是专门用来遍历一个jQuery对象。

$.each()函数可用于迭代任何集合，无论是“名/值”对象（JavaScript对象）或数组。

在迭代数组的情况下，回调函数每次传递一个数组索引和相应的数组值作为参数。

（该值也可以通过访问this关键字得到，但是JavaScript将始终将this值作为一个Object ，即使它是一个简单的字符串或数字值。）该方法返回其第一个参数，这是迭代的对象。