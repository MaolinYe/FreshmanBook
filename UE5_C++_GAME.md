在这个示例中，我们将使用C++在Unreal Engine 5 (UE5)中开发一个简单的弹球游戏。在这个游戏中，玩家需要控制一个平台，使弹球不掉落，并击中场景中的砖块以得分。

### 创建项目: 首先，在UE5中创建一个新的C++项目。选择“游戏”模板，然后选择“空白”模板。为项目命名，例如“PongGame”。

### 添加游戏角色: 在项目中创建一个新的C++类，继承自ACharacter，并将其命名为“Paddle”。这将是玩家控制的平台。在Paddle.h中添加以下代码：
```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "Paddle.generated.h"

UCLASS()
class PONGGAME_API APaddle : public ACharacter
{
    GENERATED_BODY()

public:
    APaddle();

protected:
    virtual void BeginPlay() override;

public:
    virtual void Tick(float DeltaTime) override;
    virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

private:
    void MoveHorizontal(float Value);

    UPROPERTY(EditAnywhere)
    float MovementSpeed = 1000.0f;
};
```
在Paddle.cpp中添加以下代码：
```c++
#include "Paddle.h"
#include "Components/InputComponent.h"

APaddle::APaddle()
{
    PrimaryActorTick.bCanEverTick = true;
}

void APaddle::BeginPlay()
{
    Super::BeginPlay();
}

void APaddle::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}

void APaddle::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
    Super::SetupPlayerInputComponent(PlayerInputComponent);

    PlayerInputComponent->BindAxis("MoveHorizontal", this, &APaddle::MoveHorizontal);
}

void APaddle::MoveHorizontal(float Value)
{
    AddMovementInput(FVector(1.0f, 0.0f, 0.0f), Value * MovementSpeed * GetWorld()->GetDeltaSeconds());
}
```
### 创建球类: 创建一个新的C++类，继承自AActor，并将其命名为“Ball”。这将是场景中的弹球。在Ball.h中添加以下代码：
```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Ball.generated.h"

UCLASS()
class PONGGAME_API ABall : public AActor
{
    GENERATED_BODY()

public:
    ABall();

protected:
    virtual void BeginPlay() override;

public:
    virtual void Tick(float DeltaTime) override;

private:
    UPROPERTY(VisibleAnywhere)
    class UStaticMeshComponent* BallMesh;

    UPROPERTY(VisibleAnywhere)
    class UProjectileMovementComponent* BallMovement;
};
```
在Ball.cpp中添加以下代码：
```c++
#include "Ball.h"
#include "Components/StaticMeshComponent.h"
#include "GameFramework/ProjectileMovementComponent.h"

ABall::ABall()
{
    PrimaryActorTick.bCanEverTick = true;

    BallMesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("BallMesh"));
    RootComponent = BallMesh;

    BallMovement = CreateDefaultSubobject<UProjectileMovementComponent>(TEXT("BallMovement"));
    BallMovement->InitialSpeed = 1000.0f;
    BallMovement->MaxSpeed = 1000.0f;
    BallMovement->ProjectileGravityScale = 0.0f;
    BallMovement->Bounciness = 1.0f;
    BallMovement->Friction = 0.0f;
}

void ABall::BeginPlay()
{
    Super::BeginPlay();
}

void ABall::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}
```
### 创建关卡和UI: 在UE5编辑器中，创建一个新的关卡并添加一些砖块、地面和墙壁。将Paddle和Ball类拖放到关卡中，并将摄像机放置在合适的位置。创建一个简单的UI，显示玩家的得分。
### 添加游戏逻辑： 在项目中创建一个新的C++类，继承自AGameModeBase，并将其命名为“PongGameMode”。这将用于控制游戏的核心逻辑。在PongGameMode.h中添加以下代码：
```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "PongGameMode.generated.h"

UCLASS()
class PONGGAME_API APongGameMode : public AGameModeBase
{
    GENERATED_BODY()

public:
    APongGameMode();

    virtual void Tick(float DeltaTime) override;

private:
    void ResetBall();

    UPROPERTY(EditAnywhere)
    TSubclassOf<class ABall> BallClass;

    UPROPERTY()
    ABall* CurrentBall;
};
```
在PongGameMode.cpp中添加以下代码：
```c++
#include "PongGameMode.h"
#include "Ball.h"
#include "Kismet/GameplayStatics.h"

APongGameMode::APongGameMode()
{
    PrimaryActorTick.bCanEverTick = true;
}

void APongGameMode::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    if (CurrentBall == nullptr || CurrentBall->IsPendingKill())
    {
        ResetBall();
    }
}

void APongGameMode::ResetBall()
{
    FVector SpawnLocation = FVector(0.0f, 0.0f, 100.0f);
    FActorSpawnParameters SpawnParams;

    CurrentBall = GetWorld()->SpawnActor<ABall>(BallClass, SpawnLocation, FRotator::ZeroRotator, SpawnParams);
}
```
### 添加得分系统： 在PongGameMode类中添加一个得分变量，并在球击中砖块时增加得分。然后，将得分显示在UI上。
### 测试游戏： 最后，点击UE5编辑器中的“Play”按钮来测试游戏。如果一切正常，你应该能够控制平台，使球反弹并击中砖块。


