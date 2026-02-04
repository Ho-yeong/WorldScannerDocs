# WorldScanner User Guide (v1.1.0)

![thumbnail](./pics/thumbnail.jpg)

## 1. Introduction

**Plugin Name:** WorldScanner
**Version:** 1.1.0
**Supported Engines:** UE 5.5, 5.6
**Purpose:** Reveal objects inside a growing sphere - ideal for scanning, pulse, or detection effects.

### What's New in v1.1.0

- **ScannerComponent** - Attach scanning to any actor
- **Event System** - OnScanStart, OnScanComplete, OnScanStop delegates
- **Easing Options** - Linear, EaseIn, EaseOut, EaseInOut
- **Scan Direction** - Expand outward or Contract inward
- **Soft Stop** - Freeze scan at current radius with configurable hold time

### Core Components

| Component | Description |
|-----------|-------------|
| `AScannerController` | Standalone controller actor |
| `UScannerComponent` | Component for any actor (NEW) |
| `MF_ScanMask_MPC` | Material function for masking |
| `MPC_Scannerner` | Material Parameter Collection |
| `BP_ScanFX` | Visual FX sphere mesh |

---

## 2. Installation

1. Copy the **WorldScanner** folder into your project's `/Plugins/` directory.
2. Restart Unreal Editor.
3. Enable the plugin from **Edit > Plugins > Installed > WorldScanner**.
4. Restart again if prompted.

<!-- IMAGE: UI_PluginList.png - Plugins 창에서 WorldScanner 활성화된 상태 -->

---

## 3. Quick Start

### Option A: Using ScannerController (Actor-based)

1. Add `BP_ScannerController` into your Level.
2. Assign the `MPC_Scanner` asset in its details panel.
3. (Optional) Assign a `FollowActor` to track player or object.
4. Call `StartScan()` from Blueprint or C++.

<!-- IMAGE: LV_QuickStart_Controller.png - 레벨에 BP_ScannerController 배치, Details 패널에 MPC_Scanner 할당된 상태 -->

### Option B: Using ScannerComponent (NEW)

1. Open your actor Blueprint.
2. Add Component > **Scanner Component**.
3. Assign `MPC_Scanner` in the component's details.
4. Call `StartScan()` on the component.

<!-- IMAGE: BP_QuickStart_Component.png - 액터 BP에서 ScannerComponent 추가, Details에 MPC_Scanner 할당 -->

### Blueprint Example

```
[Input Action: Scan] --> [Get ScannerController] --> [StartScan]
```

<!-- IMAGE: BP_QuickStart_InputBinding.png - Input Action에서 StartScan 호출하는 간단한 BP 노드 -->

### C++ Example

```cpp
#include "ScanFunctionLibrary.h"
#include "ScannerController.h"

// Using Controller
if (AScannerController* Scanner = UScanFunctionLibrary::GetScannerController(GetWorld()))
{
    Scanner->StartScan();
}

// Using Component
if (UScannerComponent* ScanComp = Actor->FindComponentByClass<UScannerComponent>())
{
    ScanComp->StartScan();
}
```

---

## 4. Material Integration

To apply the reveal mask to your materials:

1. Open your target material.
2. Add `MF_ScanMask_MPC` material function.
3. Set Material **Blend Mode** to **Masked**.
4. Connect `MaskSoft` or `MaskHard` to **Opacity Mask**.

<!-- IMAGE: MT_Integration_Overview.png - 머티리얼 에디터에서 MF_ScanMask_MPC 노드 연결 전체 모습 -->

| Output | Description |
|--------|-------------|
| `MaskSoft` | Smooth gradient edge |
| `MaskHard` | Sharp cutoff edge |

![material_soft](./pics/material_soft.png)
![material_hard](./pics/material_hard.png)

---

## 5. Scan Parameters

### Core Settings

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **ScannerMPC** | `MaterialParameterCollection` | - | MPC controlling the reveal effect |
| **StartRadius** | `Float` | 50.0 | Initial scan radius (cm) |
| **ExpandSpeed** | `Float` | 1200.0 | Expansion rate (cm/s) |
| **MaxRadius** | `Float` | 2000.0 | Maximum expansion limit (cm) |

<!-- IMAGE: DT_Params_Core.png - Details 패널의 Scan 카테고리 (ScannerMPC, StartRadius, ExpandSpeed, MaxRadius 보이게) -->

### Easing Options (NEW)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **ScanEasing** | `EScanEasing` | Linear | Expansion speed curve |

| Easing | Effect |
|--------|--------|
| `Linear` | Constant speed |
| `EaseIn` | Slow start, fast end |
| `EaseOut` | Fast start, slow end |
| `EaseInOut` | Smooth acceleration and deceleration |

<!-- IMAGE: DT_Params_Easing.png - ScanEasing 드롭다운 펼친 상태 (4개 옵션 보이게) -->

<!-- IMAGE: GIF_Easing_Comparison.gif - Linear vs EaseOut 비교 (나란히 또는 순차적으로) -->

### Direction Options (NEW)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **ScanDirection** | `EScanDirection` | Expand | Scan direction mode |

| Direction | Effect |
|-----------|--------|
| `Expand` | Center to outside (default) |
| `Contract` | Outside to center (reverse) |

<!-- IMAGE: DT_Params_Direction.png - ScanDirection 드롭다운 펼친 상태 -->

<!-- IMAGE: GIF_Direction_Contract.gif - Contract 모드 동작 (바깥에서 안쪽으로) -->

### Timing Settings

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **HoldAtMaxSeconds** | `Float` | 1.0 | Hold duration at max radius (s) |
| **CooldownSeconds** | `Float` | 1.0 | Delay before next scan (s) |
| **SoftStopHoldSeconds** | `Float` | 0.0 | Hold duration after soft stop (0=infinite) (NEW) |
| **bSoftStopUsesCooldown** | `Bool` | true | Apply cooldown after soft stop (NEW) |

<!-- IMAGE: DT_Params_Timing.png - Timing 카테고리 펼친 상태 -->

### Visual FX

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **bAutoSpawnFX** | `Bool` | true | Auto-spawn visual FX actor |
| **ScanFXClass** | `ActorClass` | BP_ScanFX | Visual representation mesh |

### Sound FX

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **bEnableSFX** | `Bool` | true | Enable start sound |
| **SFXVolume** | `Float` | 0.6 | Sound volume (0.0-1.0) |
| **SFX_Start** | `SoundBase` | - | Sound played on scan start |

---

## 6. Event System (NEW)

### Available Events

| Event | Parameters | Timing |
|-------|------------|--------|
| **OnScanStart** | - | When scan begins |
| **OnScanComplete** | `FinalRadius (float)` | When max radius reached |
| **OnScanStop** | `bWasCompleted (bool)` | When scan fully stops |

### Blueprint Binding

<!-- IMAGE: BP_Events_Binding.png - BeginPlay에서 OnScanComplete 바인딩하는 노드 (Bind Event to OnScanComplete -> Custom Event) -->

```
[BeginPlay]
    |
    v
[Get ScannerController Reference]
    |
    v
[Bind Event to OnScanComplete]
    |
    v
[Custom Event: HandleComplete]
    |
    v
[Print String: "Scan Complete at {FinalRadius}"]
```

### C++ Binding

```cpp
// Bind to OnScanComplete
ScannerController->OnScanComplete.AddDynamic(this, &AMyActor::HandleScanComplete);

// Handler
void AMyActor::HandleScanComplete(float FinalRadius)
{
    UE_LOG(LogTemp, Log, TEXT("Scan completed at radius: %f"), FinalRadius);
}
```

---

## 7. Soft Stop (NEW)

Stop scan while preserving visual state.

### Usage

```cpp
// Hard stop (default) - immediately hide
Scanner->StopScan(true);

// Soft stop - keep current radius visible
Scanner->StopScan(false);
```

### Configuration

| Parameter | Effect |
|-----------|--------|
| `SoftStopHoldSeconds = 0` | Hold forever until next StartScan |
| `SoftStopHoldSeconds = 3.0` | Auto-hide after 3 seconds |
| `bSoftStopUsesCooldown = true` | Apply cooldown after hold expires |

<!-- IMAGE: GIF_SoftStop_Demo.gif - 스캔 중 soft stop -> 시각 상태 유지 -> 시간 후 사라짐 -->

---

## 8. State Machine

### Scan States

| State | Description |
|-------|-------------|
| `Idle` | Ready to start |
| `Expanding` | Actively growing radius |
| `HoldingAtMax` | Holding at maximum radius |
| `Cooldown` | Waiting before next scan |

### State Query Functions

```cpp
EScanState GetScanState();    // Get current state
bool IsScanning();            // true if Expanding or HoldingAtMax
bool IsInCooldown();          // true if in Cooldown state
```

<!-- IMAGE: DG_StateMachine.png - 상태 머신 다이어그램 (Idle -> Expanding -> HoldingAtMax -> Cooldown -> Idle) -->

---

## 9. Runtime Control (NEW)

### Radius Control

```cpp
// Get current radius
float CurrentRadius = Scanner->GetCurrentRadius();

// Set absolute radius (cm)
Scanner->SetRadius(1500.f);

// Set normalized radius (0.0 to 1.0)
// 0.0 = StartRadius, 1.0 = MaxRadius
Scanner->SetRadiusNormalized(0.5f);
```

### Blueprint Nodes

<!-- IMAGE: BP_RuntimeControl.png - GetCurrentRadius, SetRadius, SetRadiusNormalized 노드들 -->

---

## 10. ScannerComponent vs ScannerController

| Feature | ScannerController | ScannerComponent |
|---------|-------------------|------------------|
| Type | Actor | ActorComponent |
| Placement | Level | Any Actor |
| Position | Self or FollowActor | Owner Actor |
| Use Case | Level-wide effects | Per-actor effects |

### When to Use Component

- Player-attached scanning
- Vehicle or NPC scanning
- Multiple independent scanners
- Runtime spawned actors

### When to Use Controller

- Single level-wide scanner
- Editor-placed persistent scanner
- Simpler setup for basic use

---

## 11. Version History

| Version | Changes | Notes |
|---------|---------|-------|
| **v1.1.0** | ScannerComponent, Events, Easing, Direction, Soft Stop | Current |
| **v1.0.0 (Beta)** | Initial release: Single scan system, FX mesh, SFX, cooldown | Legacy |

---

## 12. Troubleshooting

### Scan not visible

- Check `ScannerMPC` is assigned
- Verify material uses `MF_ScanMask_MPC`
- Ensure material Blend Mode is **Masked**

### Scan starts from wrong position

- Controller: Check `FollowActor` assignment
- Component: Verify component is attached to correct actor

### Events not firing

- Ensure binding happens before scan starts (BeginPlay)
- Check delegate signature matches

### Cooldown blocking scans

- Wait for `IsInCooldown()` to return false
- Reduce `CooldownSeconds` value

---

## 13. Known Limitations

### Single Active Scanner

**Important:** Only one scanner (Controller or Component) should be actively scanning at a time.

When multiple scanners share the same MPC, they will conflict with each other because they update the same global parameters (ScanCenterWS, ScanRadius, ScanActive).

**Symptoms:**
- Starting a scan on Component while Controller is scanning will shift the scan center
- Objects revealed by one scanner may disappear when another starts

**Recommendations:**
- Use only **one** ScannerController OR ScannerComponent per level
- If you need multiple independent scanners, wait for v1.2.0 which will include a ScannerManager system

**Planned Fix:** v1.2.0 will introduce a centralized ScannerManager to handle MPC ownership and prevent conflicts.

---

## 14. Contact / Support

- **Email:** [ghdud0503@gmail.com](mailto:ghdud0503@gmail.com)
- **Issues:** [Open a GitHub Issue](https://github.com/Ho-yeong/WorldScannerDocs/issues)
- **Documentation:** [Studio3F YouTube](https://www.youtube.com/@STUDIO3F_G)

---

Copyright (c) 2025 Studio3F. All Rights Reserved.
