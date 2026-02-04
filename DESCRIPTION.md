Reveal the world through an expanding sphere — perfect for scan, pulse, or detection effects.

DemoVideo

World Scanner provides a ready-to-use Material Function, MPC, and Controller/Component that reveal only objects within a growing sphere radius.
Easily integrate it into your gameplay mechanics or VFX sequences for scanning, radar, or energy pulse visuals.

🔹 Plug-and-play system (drop BP_ScannerController or add ScannerComponent)
🔹 ScannerComponent for attaching scan to any actor
🔹 Event system (OnScanStart, OnScanComplete, OnScanStop)
🔹 Easing options and scan direction control
🔹 Configurable expansion speed, cooldown, and duration
🔹 Optional translucent scan FX mesh and SFX support

v1.1.0 — Component support, Events, Easing, Direction, Soft Stop
----
Features:

Sphere-Based Reveal – Reveals actors within a growing sphere using MPC.
ScannerComponent – Attach scanning to any actor, follows owner.
Event System – OnScanStart, OnScanComplete, OnScanStop delegates.
Easing & Direction – EaseIn/Out curves. Expand or Contract modes.
Soft Stop – Freeze scan at current radius with hold duration.
Easy Integration – Drop Controller or add Component, call StartScan().
Customizable – Adjust speed, radius, hold, cooldown in Blueprint.
Optional FX & Sound – Includes M_ScanFX and SFX support.
Material Ready – Use MF_ScanMask_MPC for custom materials.
Optimized – All updates driven by MPC, not per-object tick.

Number of Blueprints: 4
Number of C++ Classes: 3
Network Replicated: No (planned for future version)
Supported Development Platforms:

Windows: Yes
Mac: Yes

Supported Target Build Platforms: 5.5 - 5.6
Documentation Link: https://github.com/Ho-yeong/WorldScannerDocs
Create Issues: https://github.com/Ho-yeong/WorldScannerDocs/issues
