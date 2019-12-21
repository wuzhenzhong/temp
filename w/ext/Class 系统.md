# Ext.js Class 系统



Ext JS是一个JavaScript框架，它具有面向对象编程的功能。
Ext是封装Ext JS中所有类的命名空间。

**在Ext JS中定义类**

Ext提供了300多个类，我们可以用于各种功能。

Ext.define（）用于在Ext JS中定义类。

**语法:**

```
Ext.define(class name, class members/properties, callback function);
```

类名称是根据应用程序结构的类名称。 appName.folderName.ClassName
studentApp.view.StudentView。

类属性/成员 - 定义类的行为。

回调函数是可选的。 当类正确加载时，会调用它。

**Ext JS类定义示例**

```
Ext.define(studentApp.view.StudentDeatilsGrid, {
   extend : 'Ext.grid.GridPanel',
   id : 'studentsDetailsGrid',
   store : 'StudentsDetailsGridStore',
   renderTo : 'studentsDetailsRenderDiv',
   layout : 'fit',
   columns : [{
      text : 'Student Name',
      dataIndex : 'studentName'
   },{
      text : 'ID',
      dataIndex : 'studentId'
   },{
      text : 'Department',
      dataIndex : 'department'
   }]
});
```

**创建对象**

像其他基于OOPS的语言一样，我们也可以在Ext JS中创建对象。

不同的方式创建对象在Ext JS。

使用new关键字:

```
var studentObject = new student();
studentObject.getStudentName();
```

使用Ext.create（）:

```
Ext.create('Ext.Panel', {
   renderTo : 'helloWorldPanel',
   height : 100,
   width : 100,
   title : 'Hello world',
   html : 	'First Ext JS Hello World Program'		
});
```

**Ext JS中的继承**

继承是将类A中定义的功能用于类B的原理。

在Ext JS继承可以使用两种方法 。

Ext.extend:

```
Ext.define(studentApp.view.StudentDetailsGrid, {
   extend : 'Ext.grid.GridPanel',
   ...
});
```

这里我们的自定义类StudentDetailsGrid使用Ext JS类GridPanel的基本功能。

使用Mixins:

Mixins是在没有扩展的情况下在类B中使用类A的不同方式。

```
mixins : {
   commons : 'DepartmentApp.utils.DepartmentUtils'
},
```

Mixins我们添加在控制器中，我们声明所有其他类，如存储，视图等。在这种方式，我们可以调用DepartmentUtils类，并在控制器或在这个应用程序中使用其功能。