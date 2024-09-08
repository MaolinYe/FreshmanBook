# UE5 C++
### UBT 
UBT (Unreal Build Tool) 是一个用于构建和打包UE5项目的命令行工具。它负责处理项目的构建过程，包括编译源代码、链接库文件、生成可执行文件等。UBT使用C++和Python编写，并提供了一套用于管理和配置项目构建的脚本接口。
### UHT
UHT (Unreal Header Tool) 是一个用于处理UE5项目中头文件的预处理工具。它主要负责解析C++头文件并生成反射信息，以支持Unreal Engine的反射系统。UHT会分析头文件中的类、函数、成员变量等信息，并将其转化为可用于运行时反射的数据结构。这样，UE5引擎就可以在运行时动态地访问和操作这些类和对象。
## UE反射系统
虚幻引擎反射系统使用宏为提供引擎和编辑器各种功，封装你的类。在使用 虚幻引擎（UE） 时，可以使用标准的C++类、函数和变量。
### 垃圾回收
虚幻实现垃圾回收机制，不再被引用或已被显式标记为销毁的`UObject`将定期清除。引擎构建一个引用图表以确定哪些`UObject`仍在使用，哪些是孤立的。在该图表根部是一组指定为"根集"的`UObject。任何`UObject`都可以添加到根集。当进行垃圾回收时，引擎将从根集开始，搜索已知`UObject`引用树来跟踪所有引用的`UObject。任何未被引用的`UObject`（意味着未在树搜索中找到这些对象）将被假设为不再需要，因此被删除。

一个实际的影响是，你通常需要保持对希望保持活跃的任何Object的`UPROPERTY`引用，或者将指向它的指针存储在`TArray`或其他引擎容器类中。Actor及其组件通常属于例外情况，因为Actor通常被链接回到根集的Object引用（例如它们所属的关卡），而Actor的组件被Actor自身引用。Actor可以显式标记为销毁，方法是调用它们的`Destroy`函数，这是从进行中游戏移除Actor的标准方法。组件可以使用`DestroyComponent`函数显式销毁，但它们通常在拥有它们的Actor从游戏中移除时被销毁。

虚幻引擎4中的垃圾回收速度快，效率高，内置大量的优化功能，能够尽量降低开销，如多线程可访问性分析可以标识孤立Object，优化的反加密代码能够尽快从容器中移除Actor。还有一些其他功能以调节，以更精准地控制如何以及何时执行垃圾回收，大部分都可以在 项目设置（Project Settings） 中的 引擎 - 垃圾回收（Engine - Garbage Collection） 下找到。

https://docs.unrealengine.com/5.3/zh-CN/unreal-object-handling-in-unreal-engine/
### UE智能指针
https://docs.unrealengine.com/5.3/zh-CN/smart-pointers-in-unreal-engine/
### UE容器
#### TArray 虚幻引擎中的数组
https://docs.unrealengine.com/5.3/zh-CN/array-containers-in-unreal-engine/
#### TMap 虚幻引擎的映射
https://docs.unrealengine.com/5.3/zh-CN/map-containers-in-unreal-engine/
#### TSet 虚幻引擎中的哈希集合
https://docs.unrealengine.com/5.3/zh-CN/set-containers-in-unreal-engine/
### 断言 assert
断言"必须一律为true，否则程序会停止运行，虚幻引擎提供 assert 等同项的三个不同族系：check、verify 和 ensure  
https://docs.unrealengine.com/5.3/zh-CN/asserts-in-unreal-engine/

### 常见的宏
在Unreal Engine（UE）中，宏（Macro）是一种预处理器指令，用于在编译时执行文本替换。它们通常用于定义常量、简化代码、控制条件编译等。以下是一些常见的UE宏及其作用：  
1. `UCLASS`：用于声明一个C++类是UObject的子类，以便UE引擎可以识别和管理该类。  
2. `UFUNCTION`：用于声明一个C++成员函数是UObject的成员函数，以便UE引擎可以识别和调用该函数。  
3. `UPROPERTY`：用于声明一个C++成员变量是UObject的属性，以便UE引擎可以识别和访问该属性。  
4. `USTRUCT`：用于声明一个C++结构体是UObject的结构体，以便UE引擎可以识别和管理该结构体。  
5. `UENUM`：用于声明一个C++枚举类型是UObject的枚举类型，以便UE引擎可以识别和管理该枚举类型。  
6. `UDELEGATE`：用于声明一个C++委托类型是UObject的委托类型，以便UE引擎可以识别和管理该委托类型。  
7. `UMETA`：用于为UObject的属性、函数等添加元数据，以便UE引擎可以根据元数据执行特定操作。  
8. `GENERATED_BODY`：用于自动生成UObject所需的代码，通常与`UCLASS`、`UFUNCTION`、`UPROPERTY`等宏一起使用。  
9. `UE_LOG`：用于记录日志信息，可以在运行时查看和调试。  
10. `check`：用于检查条件是否满足，如果不满足则输出错误信息。  
11. `ensure`：用于确保条件是否满足，如果不满足则输出错误信息并返回。  
12. `static_assert`：用于在编译时检查条件是否满足，如果不满足则编译失败。

### 旋转角
旋转的pitch、yaw和roll分别对应于三维空间中的X轴、Y轴和Z轴。这三个轴通常称为欧拉角（Euler angles）。

Pitch（俯仰）：绕X轴旋转，表示物体在垂直方向上的倾斜程度。正值表示向上倾斜，负值表示向下倾斜。  
Yaw（偏航）：绕Y轴旋转，表示物体在水平方向上的旋转程度。正值表示向右旋转，负值表示向左旋转。  
Roll（滚转）：绕Z轴旋转，表示物体在自身平面内的旋转程度。正值表示顺时针旋转，负值表示逆时针旋转。

欧拉角在描述物体旋转时非常直观，但在实际应用中可能会遇到万向节锁（gimbal lock）等问题。为了避免这些问题，可以使用四元数（quaternions）、轴角（axis-angle）等其他表示方法来描述物体的旋转。  

万向节锁是一个涉及三维空间旋转的问题，常见于使用欧拉角表示旋转的系统中。欧拉角通过三个角度来表示物体在三维空间的方向，通常对应于围绕三个垂直轴的旋转，如俯仰、偏航、滚转。当两个旋转轴对齐时，万向节锁问题就会出现。在这种情况下，系统失去了一个旋转自由度，导致无法独立表示围绕这两个轴的旋转。这种对齐通常发生在某个轴的旋转角达到90度时。例如，在飞机俯仰角达到90度(即垂直向上或向下)时，偏航和滚转轴将会对齐，飞机将无法独立控制偏航和滚转动作。