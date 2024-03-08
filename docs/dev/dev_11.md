# 第 45 章 UML

```
UML 中的视图分为 5 大类(每一类的名称都有好几种说法，但表示的意思是差不多的，下面主要是按照 EA 中的分法）：
a)     用例视图(Use Case View)，强调从用户角度看到的或需要的系统功能，是被称为参与者的外部用户所能观察到的系统功能的模型图。
b)     动态视图(Dynamic View)，体现了系统的动态或者行为特征，也称为行为模型视图(Behavioral Model View)或并发视图(Concurrent View)。
c)     逻辑视图(Logical View)，展现系统的静态或结构组成及特征，也被称为结构模型视图(Structural Model View)或者静态视图(Static View)。
d)     组件视图(Component View)，体现了系统实现的结构和行为特征，也称为实现模型视图(Implementation Model View)。
e)     配置视图(Deployment View)，体现了系统实现环境的结构和行为特征，也被称为环境模型视图(Environment Model View)或者物理视图(Physical View)。

```

```
在 EA 中还有一个 Custom,其相当于设计者自己定义的一个视图，并不是 UML 的定义。
      UML 中的图有 9 种：
a)     用例图(Use Case Diagram)，描述系统功能；
b)     类图(Class Diagram)，描述系统的静态结构；
c)     对象图(Object Diagram)，描述系统在某个时刻的静态结构；
d)     时序图(Sequence Diagram)，按时间顺序描述系统元素间的交互；
e)     协作图(Collaboration Diagram)，按照时间和空间顺序描述系统元素间的交互和他们之间的关系；
f)     状态图(State Diagram)，描述了系统元素的状态条件和响应；
g)     活动图(Activity Diagram)，描述了系统元素的活动；
h)     组件图(Component Diagram)，描述了实现系统的元素的组织；
i)     配置图(Deployment Diagram)，描述了环境元素的配置，并把实现系统的元素映射到配置上。

```

```
在 UML 中视图是由图构成的，视图和图之间的对应关系：
用例视图：用例图
动态视图：时序图、协作图、状态图和活动图
逻辑视图：类图和对象图
组件视图：组件图
配置视图：配置图

```