---
title: Effective Java Chapter 5
excerpt: |
  Effective Java Chapter 5
category: Books
feature_image: "https://picsum.photos/2560/600?image=872"
---
## 第5章 泛型

Java 1.5 发行版本中增加了泛型(**Generic**)。在没有泛型之前，从集合中读取到的每一个对象都必须进行转换。如果有人不小心插入了类型错误的对象，在运行时的转换处理就会出错。**有了泛型之后，可以告诉编译器每个集合中接受哪些对象类型。编译器自动地为你的插入进行转化，并在编译时告知是否插入了类型错误的对象。**这样可以使程序更加安全，也更加清楚。

### 第23 条：请不要在新代码中使用原生态类型(raw type)

**声明中具有一个或者多个类型参数(type parameter) 的类或者接口，就是泛型(generic) 类或者接口。**泛型类和接口统称为**泛型(generic type)。**

