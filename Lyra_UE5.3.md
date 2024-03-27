Lyra UE5.3
=
## 打印调试消息
```c++
	GEngine->AddOnScreenDebugMessage(-1, 5.0f, FColor::Red, TEXT("We are using FPSCharacter."));
```
## 计分
Lyra通过GameMode加载Experience，游戏的玩法逻辑通过在Experience中动态挂载Actor的方式实现，游戏对局当中的得分记录在一个映射里面，这个映射是一个Gameplay标签和整数的映射，如果需要添加得分类型或者广播消息就需要添加Gameplay标签，角色的伤害和死亡事件都通过广播消息转发，要改变Gameplay标签映射的值需要通过LyraTeamSubsystem的AddTeamTagStack方法或者LyraPlayerState的AddStatTagStack方法实现
## 技能映射
新增Input-Action的输入操作的数据资产，然后到输入映射的IMC_Default中绑定新输入的映射，接着在InputData_Hero中新增Ability Input Actions绑定输入标签，然后到AbilitySet中新增Granted Gameplay Ability绑定输入标签
## 检测碰撞的物体
将物体转化为某个类对象，如果该类对象不为空说明撞的就是它  
onHit是碰撞，overlap是检测覆盖
## 生成物体携带信息
设置该物体的owner
## 全局信息
广播消息或者设置gameplay标签
## UE崩溃
UE崩溃的原因多是指针原因，即访问的内存空间不合法
```c++
	Destroy();
	GetOwner()->Destroy();
```
## 蓝图Set Timer无效
在Tick调用里面Set Timer会重复Set导致无效