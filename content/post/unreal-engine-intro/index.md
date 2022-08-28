---
title: "PersonalityManGame Blog 1: Unreal Engine Intro"
author: Esau Abraham
date: 2022-08-27
draft: false
categories: 
    - Unreal Engine
    - Game Development
    - C++
description: My exploration with coding in C++ with Unreal Engine 5
image: https://media.giphy.com/media/22VCh90itjL8rr5uza/giphy.gif
---

**Ever heard of Unreal Engine?**

![**Image of Unreal Engine Logo**](https://knowtechie.com/wp-content/uploads/2022/04/unreal-engine-5.jpg "An image of the Unreal Engine Logo")

![**Image from Unreal Engine 5 Teaser**](https://www.spieltimes.com/wp-content/uploads/2022/04/Unreal-Engine-5.jpg "An image from the Unreal Engine 5 teaser")

# Quick Explanation of Unreal
Unreal Engine is a 3D creation tool that primarily enables game developers to create 3D envionrments and experiences. In there own definition:
> Free to download, Unreal Engine comes with everything you need to build and ship successful multi-platform games and location-based entertainment for any genre. With full access to source code, a robust C++ API, and Blueprint visual scripting at your fingertips, you can develop your title any way you like.

# What Am I Doing With Unreal?
Me, wanted to start a new hobby game dev project, decided to start working on a first person game! After many years of using [Unity](https://unity.com/), I thought it would be a great chance to finally learn how to properly take advantage of Unreal Engine's power! It's known for being exclusively good for first person games.

# How Does Unreal Work?

## Coding Process
Unreal has 2 ways to code your games: **Blueprints** and **C++**

### Blueprints
Blueprints are a visual scripting interface that allows you to code using visual building blocks. It contains parts to it that reflect what you would see when making a class in C++: functions, variables, events, components

![**Blueprints exmaple**](https://docs.unrealengine.com/5.0/Images/programming-and-scripting/blueprints-visual-scripting/QuickStart/BPQS_6_Step4.png "Blueprint example")

### C++ Coding
Unreal Engine is built and compiled in C++, it comes with libraries that allow users to create their Unreal Objects, Classes, Modules, Game Systems, and more

![**Image of Visual Studio**](/content/post/unreal-engine-intro/cplusShot.png "C++ Enviornment Example in Visual Studios")

### Compatibility
The blueprint system and C++ enviornment is designed in a way that your code can easily be converted between the two types

![**Image of Blueprint to C++ option**](https://d3kjluh73b9h9o.cloudfront.net/optimized/3X/f/6/f6a555c08687118b57ffc5050f0a1b60e6fe6527_2_339x500.png "How to convert from Blueprint to C++ in Unreal")

## Core Components

There's more for me to learn about the core workflow of Unreal but here is what I've grasped.

There are 2 important pieces of Unreal Engine: **Actors** and **GameModes**

### Actors
Actors are objects that can be spawn/instantiated into the game world. It is an all encompassing role that can be any object, some examples are the camera, the player, even things that are not physical like starting location points.

![**Image of Chair Actors**](https://docs.unrealengine.com/4.26/Images/BuildingWorlds/LDQuickStart/TableAndChairs.jpg "An example of Actors: Chairs")

#### Pawn Actors
A pawn actor is a more specifc type of actor. This is an actor that can be in UE's words *possessed*. I would like to say, you are able to control it during runtime to do certain actions. An example of this is the character that the player controls. The player will take control of this pawn and make it perform actions that progress the game and triggere events. 

The entity that controls a pawn is not limited to just the player. An AI could be the one controlling the pawn, for example with enemies.

![**Gif of Enemy**](https://gamedevacademy.org/wp-content/uploads/2020/05/ezgif-6-6c0eb29d6cf7.gif "An Example of a Pawn Actor: Enemy AI")

#### Character Actors
A character actor is a *varient* of the pawn actor, where it's usage is still the same, but it comes with implementations of features such as movement related to land, water, sky. It also comes with skeleton mesh components which are allow complex animations to occur with the actor and a capsule which allows collision detection and interaction with the world. **This is recommended to be used for the actor that the player will control.**

![**Character Actor Example**](https://c.tenor.com/D0-dH0cw95YAAAAC/kingdomhearts-running.gif)

*An example of a character actor can be seen from the UE5 First-Person template:*
```cpp
// Copyright Epic Games, Inc. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "PersonalityManGameCharacter.generated.h"

class UInputComponent;
class USkeletalMeshComponent;
class USceneComponent;
class UCameraComponent;
class UAnimMontage;
class USoundBase;

// Declaration of the delegate that will be called when the Primary Action is triggered
// It is declared as dynamic so it can be accessed also in Blueprints
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnUseItem);

UCLASS(config=Game)
class APersonalityManGameCharacter : public ACharacter
{
	GENERATED_BODY()

	/** Pawn mesh: 1st person view (arms; seen only by self) */
	UPROPERTY(VisibleDefaultsOnly, Category=Mesh)
	USkeletalMeshComponent* Mesh1P;

	/** First person camera */
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = Camera, meta = (AllowPrivateAccess = "true"))
	UCameraComponent* FirstPersonCameraComponent;

public:
	APersonalityManGameCharacter();

protected:
	virtual void BeginPlay();

public:
	/** Base turn rate, in deg/sec. Other scaling may affect final turn rate. */
	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category="PlayerCameraSettings")
	float GamepadSensitivity;

	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = "PlayerCameraSettings")
	float MouseSensitivity;

	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = "PlayerMovementSettings")
	float WalkSpeed;

	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = "PlayerMovementSettings")
	float SprintSpeed;

	/** Delegate to whom anyone can subscribe to receive this event */
	UPROPERTY(BlueprintAssignable, Category = "Interaction")
	FOnUseItem OnUseItem;
protected:
	
	/** Fires a projectile. */
	void OnPrimaryAction();

	/** Handles moving forward/backward */
	void MoveForward(float Val);

	/** Handles strafing movement, left and right */
	void MoveRight(float Val);

	/**
	 * Called via mouse input to turn at a given rate.
	 * @param Rate	This is a normalized rate, i.e. 1.0 means 100% of desired turn rate
	 */
	void MouseTurnAtRate(float Rate);

	/**
	 * Called via mouse input to turn look up/down at a given rate.
	 * @param Rate	This is a normalized rate, i.e. 1.0 means 100% of desired turn rate
	 */
	void MouseLookUpRate(float Rate);

	/**
	 * Called via gamepad input to turn at a given rate.
	 * @param Rate	This is a normalized rate, i.e. 1.0 means 100% of desired turn rate
	 */
	void GamePadTurnAtRate(float Rate);

	/**
	 * Called via gamepad input to turn look up/down at a given rate.
	 * @param Rate	This is a normalized rate, i.e. 1.0 means 100% of desired turn rate
	 */
	void GamePadLookUpAtRate(float Rate);

	void SetSprint();

	void SetWalk();


	struct TouchData
	{
		TouchData() { bIsPressed = false;Location=FVector::ZeroVector;}
		bool bIsPressed;
		ETouchIndex::Type FingerIndex;
		FVector Location;
		bool bMoved;
	};
	void BeginTouch(const ETouchIndex::Type FingerIndex, const FVector Location);
	void EndTouch(const ETouchIndex::Type FingerIndex, const FVector Location);
	void TouchUpdate(const ETouchIndex::Type FingerIndex, const FVector Location);
	TouchData	TouchItem;
	
protected:
	// APawn interface
	virtual void SetupPlayerInputComponent(UInputComponent* InputComponent) override;
	// End of APawn interface

	/* 
	 * Configures input for touchscreen devices if there is a valid touch interface for doing so 
	 *
	 * @param	InputComponent	The input component pointer to bind controls to
	 * @returns true if touch controls were enabled.
	 */
	bool EnableTouchscreenMovement(UInputComponent* InputComponent);

public:
	/** Returns Mesh1P subobject **/
	USkeletalMeshComponent* GetMesh1P() const { return Mesh1P; }
	/** Returns FirstPersonCameraComponent subobject **/
	UCameraComponent* GetFirstPersonCameraComponent() const { return FirstPersonCameraComponent; }

};
```
### GameModes
The name "gamemode" is pretty self-explanitory on the use case of this module. It defines the rules of your game and "what" the game is. Some examples are "how is the player spawned into the game", "what systems are spawn", "how many players can be in the game at once", the list goes on.

>While certain fundamentals, like the number of players required to play, or the method by which those players join the game, are common to many types of games, limitless rule variations are possible depending on the specific game you are developing. Regardless of what those rules are, Game Modes are designed to define and implement them. There are currently two commonly-used base classes for Game Modes. - Unreal Engine Documentation

*An example of a gamemode can be seen from the UE5 First-Person template:*
```cpp
// Copyright Epic Games, Inc. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "PersonalityManGameGameMode.generated.h"

UCLASS(minimalapi)
class APersonalityManGameGameMode : public AGameModeBase
{
	GENERATED_BODY()

public:
	APersonalityManGameGameMode();
};
```
```cpp
// Copyright Epic Games, Inc. All Rights Reserved.

#include "PersonalityManGameGameMode.h"
#include "PersonalityManGameCharacter.h"
#include "UObject/ConstructorHelpers.h"

APersonalityManGameGameMode::APersonalityManGameGameMode()
	: Super()
{
	// set default pawn class to our Blueprinted character
	static ConstructorHelpers::FClassFinder<APawn> PlayerPawnClassFinder(TEXT("/Game/FirstPerson/Blueprints/BP_FirstPersonCharacter"));
	DefaultPawnClass = PlayerPawnClassFinder.Class;

}
```

# That's It For Now!
That's all the information I've acculmulated so far, look out for the next blog where I start working on that first-person project, specifically the movement system!