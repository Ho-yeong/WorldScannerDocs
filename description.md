Reveal the world through an expanding sphere — perfect for scan, pulse, or detection effects.



DemoVideo



World Scanner provides a ready-to-use Material Function, MPC, and Controller/Component that reveal only objects within a growing sphere radius.
Easily integrate it into your gameplay mechanics or VFX sequences for scanning, radar, or energy pulse visuals.

🔹 Plug-and-play system (drop BP_ScannerController or add ScannerComponent and call StartScan())
🔹 NEW: ScannerComponent for attaching to any actor
🔹 NEW: Event system (OnScanStart, OnScanComplete, OnScanStop)
🔹 NEW: Easing options (Linear, EaseIn, EaseOut, EaseInOut)
🔹 NEW: Scan direction (Expand outward or Contract inward)
🔹 NEW: Soft stop with configurable hold time
🔹 Configurable expansion speed, cooldown, and duration
🔹 Optional translucent scan FX mesh (M_ScanFX)
🔹 Works with custom materials using MF_ScanMask_MPC
🔹 Includes SFX trigger and parameter controls



v1.1.0 — Major feature update with Component support, Events, and enhanced control options.


----
Features:




Sphere-Based Reveal Effect – Reveals actors and materials only within a growing spherical radius using a Material Parameter Collection.



ScannerComponent (NEW) – Attach scanning capability to any actor. Automatically follows the owner's position.



Event System (NEW) – OnScanStart, OnScanComplete, OnScanStop delegates for Blueprint and C++ integration.



Easing Options (NEW) – Choose from Linear, EaseIn, EaseOut, or EaseInOut expansion curves.



Scan Direction (NEW) – Expand outward from center or Contract inward from max radius.



Soft Stop (NEW) – Freeze scan at current radius with configurable hold duration.



Easy Integration – Drop BP_ScannerController into any level or add ScannerComponent to any actor and call StartScan().



Customizable Parameters – Adjust expansion speed, max radius, hold duration, and cooldown directly in Blueprint.



Optional FX & Sound – Includes translucent M_ScanFX and start SFX support.



Material Ready – Use MF_ScanMask_MPC and M_Master_Scannable for instant integration into custom materials.



Optimized – Lightweight system; all updates driven by MPC, not per-object tick.



Number of Blueprints: 4

Number of C++ Classes: 3

Network Replicated: No (planned for future version)

Supported Development Platforms:




Windows: Yes



Mac: Yes

Supported Target Build Platforms: 5.5 - 5.6

Documentation Link: https://github.com/Ho-yeong/WorldScannerDocs

Create Issues: https://github.com/Ho-yeong/WorldScannerDocs/issues


Ideal for: scanning abilities, detection waves, radar effects, energy pulses, or cinematic reveals.
