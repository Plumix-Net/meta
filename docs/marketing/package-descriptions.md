# Plumix — Package Descriptions

Copy-paste descriptions for NuGet, package registries, and release notes.

---

## Plumix (core)

**NuGet ID:** `Plumix`
**NuGet URL:** https://www.nuget.org/packages/Plumix/

### Short (NuGet tagline, ~160 chars)
Flutter-inspired UI framework for .NET. Widget/Element/RenderObject architecture in C# with Flutter's own API shapes. Cross-platform: desktop, web, Android, iOS.

### Medium (NuGet description field)
Plumix is a Flutter-inspired UI framework for .NET that implements Flutter's Widget/Element/RenderObject architecture in C#.

Build declarative, cross-platform UIs with the same primitives as Flutter — `StatelessWidget`, `StatefulWidget`, `BuildContext`, `RenderObject`, `InheritedWidget` — and target Windows, macOS, Linux, WebAssembly, Android, and iOS from a single codebase.

The framework owns its own render pipeline (layout, paint, compositing, semantics) and its own value types (`Offset`, `Size`, `Rect`, `FontWeight`, images, shaders): its public API names no host type. Windowing, input and drawing come from a host package — `Plumix.Avalonia` (desktop, web, iOS, Android) or the native `Plumix.Android`. Gesture recognition, navigation, scrolling, focus management, and the full widget lifecycle are all implemented inside Plumix.

**What's included in the core package:**
- Widget/Element/RenderObject architecture (`StatelessWidget`, `StatefulWidget`, `State`, reconciliation)
- Full `RenderBox` layout model — flex, stack, alignment, constraints, aspect ratio
- Scrolling: `ListView`, `GridView`, `SingleChildScrollView`, `CustomScrollView`, full Sliver protocol
- Navigation: `Navigator`, routes, `ModalRoute`, hero transitions, `NavigatorObserver`
- Gesture system: tap, long-press, drag, gesture arena, `GestureDetector`
- Focus & accessibility: `Focus`, `FocusScope`, tab traversal, semantics
- Text input: `EditableText`, `TextInput`, selection, IME
- Inherited widgets: `MediaQuery`, `DefaultTextStyle`, `Directionality`, `IconTheme`
- Animation primitives: `AnimationController`, `Ticker`

---

## Plumix.Avalonia

**NuGet ID:** `Plumix.Avalonia`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Avalonia/

### Short
Avalonia host for Plumix: run a Flutter-style widget app in an Avalonia window, browser canvas, iOS view or Android activity.

### Medium
Plumix.Avalonia hosts a Plumix widget tree on Avalonia. Derive your app from `PlumixApplication` (or place a `PlumixHost` control in any Avalonia window) and Plumix renders, handles input, text input, accessibility and windowing through Avalonia on Windows, macOS, Linux, WebAssembly, iOS and Android.

Mixing Plumix screens with Avalonia controls? `Plumix.Hosting.AvaloniaInterop` converts colors, insets, matrices and geometry between the two.

---

## Plumix.Android

**NuGet ID:** `Plumix.Android`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Android/

### Short
Native Android host for Plumix, without Avalonia: renders through Impeller and ports the Flutter engine's Android embedding.

### Medium
Plumix.Android runs a Plumix widget app on Android without Avalonia. Derive your Activity from `PlumixActivity` and return your app from `CreateApp()`: Plumix renders through Impeller (the `Plumix.Impeller` backend) on its own raster thread, and the host ports the Flutter engine's Android embedding — touch, mouse and stylus input, the keyboard, IME and autofill, accessibility, platform channels, predictive back, state restoration and system fonts.

## Plumix.iOS

**NuGet ID:** `Plumix.iOS`
**NuGet URL:** https://www.nuget.org/packages/Plumix.iOS/

### Short
Native iOS host for Plumix, without Avalonia: renders through Impeller on Metal and ports the Flutter engine's iOS embedding.

### Medium
Plumix.iOS runs a Plumix widget app on iOS without Avalonia. Derive your view controller from `PlumixViewController` and return your app from `CreateApp()`: Plumix renders through Impeller on Metal (the `Plumix.Impeller` backend) on its own raster thread, and the host ports the Flutter engine's iOS embedding — touch input, text input and the keyboard, hardware keys, VoiceOver, the platform plugin (status bar, haptics, clipboard, share), Dynamic Type and system fonts.

## Plumix.Templates

**NuGet ID:** `Plumix.Templates`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Templates/

### Short
dotnet new templates for Plumix: plumix-app creates shared Flutter-style widgets with desktop, Android and iOS projects.

### Medium
Plumix.Templates installs the `plumix-app` template: `dotnet new plumix-app -n MyApp` creates a shared project for the app's widgets and a thin host project per platform — desktop on Avalonia (`PlumixApplication`), Android on the native host (`PlumixActivity`) and iOS on the native host (`PlumixViewController`). `--android false`, `--ios false` and `--desktop false` leave a platform out; `--application-id` sets the Android application id and iOS bundle id. The template pins the Plumix version it was released with.

## Plumix.Impeller

**NuGet ID:** `Plumix.Impeller`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Impeller/

### Short
Impeller rendering backend for Plumix's native hosts: display lists, text, images and shaders on Flutter's renderer.

### Medium
Plumix.Impeller draws a Plumix app with Impeller, the renderer of the Flutter engine, through the NImpeller bindings (`Plumix.NImpeller`). It plays the framework's display lists and layers, shapes text with HarfBuzz into Impeller glyph runs, decodes images and compiles fragment programs. Native host packages such as `Plumix.Android` and `Plumix.iOS` reference it; apps do not use it directly.

## Plumix.Material

**NuGet ID:** `Plumix.Material`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Material/

### Short
Material Design 3 component library for Plumix. Scaffold, buttons, cards, navigation, forms, theming — faithful to Flutter's Material widgets.

### Medium
Material Design 3 component library for the Plumix framework.

Implements Flutter's Material widget layer in C#: the same components, theming system, and behavioral semantics, so Flutter developers can port Material UIs to .NET without redesigning them.

**Components:**
- **Structure:** `Scaffold`, `AppBar`, `Drawer`, `BottomNavigationBar`, `SafeArea`
- **Buttons:** `ElevatedButton`, `FilledButton`, `OutlinedButton`, `TextButton`, `IconButton`
- **Controls:** `Checkbox`, `Switch`, `Radio`, `FloatingActionButton` (standard, small, large, extended)
- **Content:** `Card` (elevated/filled/outlined), `ListTile` (1/2/3-line), `Tooltip`
- **Theming:** `ThemeData`, `ColorScheme`, `TextTheme`, per-component theme data classes (M3 tokens)

Requires `Plumix` (core).

---

## Plumix.Cupertino

**NuGet ID:** `Plumix.Cupertino`
**NuGet URL:** https://www.nuget.org/packages/Plumix.Cupertino/

### Short
iOS/macOS Cupertino-style adaptive widgets for Plumix. Write platform-adaptive UIs that look native on Apple platforms.

### Medium
Cupertino (iOS/macOS) adaptive widget library for the Plumix framework.

Provides iOS/macOS-styled components that match Flutter's Cupertino widget library, enabling platform-adaptive UIs that automatically use the correct visual style per platform.

**Current components:**
- `CupertinoCheckbox` — iOS-style checkbox
- `CupertinoRadio` — iOS-style radio button

More components are added each release alongside Material coverage.

Requires `Plumix` (core).

---

## Release notes template

```
## Plumix X.Y.Z

### New
- ...

### Changed
- ...

### Fixed
- ...

**Install:**
dotnet add package Plumix --version X.Y.Z
dotnet add package Plumix.Material --version X.Y.Z
dotnet add package Plumix.Cupertino --version X.Y.Z
```

---

## Installation

```bash
# New app from the template (desktop, Android and iOS projects)
dotnet new install Plumix.Templates
dotnet new plumix-app -n MyApp

# Core framework only (widgets; add a host to run them)
dotnet add package Plumix
dotnet add package Plumix.Avalonia

# With Material Design 3
dotnet add package Plumix.Avalonia
dotnet add package Plumix.Material

# Full stack
dotnet add package Plumix.Avalonia
dotnet add package Plumix.Material
dotnet add package Plumix.Cupertino
```

Minimum requirement: **.NET 11**
