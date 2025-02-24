---
title: Games
---

{{< toc >}}

## Unity
Unity is a cross-platform game engine.

- [Unity](https://unity.com/)

Unity games can be one of the following types:

- Mono
- IL2CPP

### Mono
Mono games can be decompiled using one of the tools listed under .NET in the [Decompilers](../decompilers/#net) section.

The game code can be found in the `Assembly-CSharp.dll` file which can be found in the `Managed` folder.

### IL2CPP
To reverse engineer IL2CPP games, the following tools can be used:

- [IL2CPP Dumper](https://github.com/Perfare/Il2CppDumper)
- [Cpp2IL](https://github.com/SamboyCoding/Cpp2IL)

### Extracting Assets
Unity assets can be extracted using one of the following tools:

- [AssetRipper](https://github.com/AssetRipper/AssetRipper)
- [Unity Assets Bundle Extractor](https://github.com/SeriousCache/UABE)

### Loaders
Unity games can be loaded with plugins using one of the following loaders:

- [MelonLoader](https://melonloader.net/)
- [BepInEx](https://github.com/BepInEx/BepInEx)

Using one of these loaders, a plugin like [UnityExplorer](https://github.com/sinai-dev/UnityExplorer) can be used to explore the game's objects.

---

## Godot
Godot is a cross-platform game engine.

- [Godot](https://godotengine.org/)

[gdsdecomp](https://github.com/GDRETools/gdsdecomp) can be used to reverse engineer Godot games.
