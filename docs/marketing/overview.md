# Plumix — Project Overview

> Flutter-inspired UI framework for .NET. Build declarative, widget-based applications in C# using the same architecture as Flutter — Widget, Element, RenderObject — on .NET 11, with native Impeller hosts on Android and iOS and Avalonia on desktop and in the browser.

_Updated 2026-11-23._

**Website:** https://plumix.net/
**GitHub:** https://github.com/Plumix-Net/Plumix
**Packages:** https://github.com/Plumix-Net/Plumix.Packages
**NuGet:** https://www.nuget.org/packages/Plumix/

---

## One-liner

> Flutter's widget model, in C#, for every platform .NET reaches.

---

## Short description (tweet / GitHub About / NuGet tagline)

Plumix is a Flutter-inspired UI framework for .NET. Write declarative, widget-based UIs in C# with Flutter's Widget/Element/RenderObject architecture — on Android, iOS, desktop and the web.

---

## Medium description (landing page hero / Product Hunt / README)

Plumix brings Flutter's proven UI architecture to the .NET ecosystem. Instead of adapting to a new mental model, Flutter developers can write C# using the same primitives they already know — `StatelessWidget`, `StatefulWidget`, `BuildContext`, `RenderObject` — and ship to desktop, web, Android, and iOS from a single codebase.

Layout, paint and widget lifecycle are entirely framework-owned; a thin host supplies the surface, input and platform services — the same architectural split Flutter uses between the framework and the engine. On Android and iOS that host is Plumix's own, rendering through Impeller like Flutter's engine; on desktop and in the browser it is Avalonia.

---

## Long description (blog intro / Product Hunt story / conference abstract)

Modern cross-platform UI development is dominated by two mental models: the component-based model (React, SwiftUI) and Flutter's widget tree model. Flutter's approach — immutable widgets, a reconciled element tree, and a framework-owned render pipeline — has proven itself for building complex, high-performance UIs. But it's locked to Dart.

Plumix is a faithful port of Flutter's architecture to C# and .NET. The goal is not to create "Flutter-like" abstractions on top of existing .NET UI frameworks, but to reimplement the actual architecture: the widget/element reconciliation loop, the `RenderObject`/`RenderBox` layout model, the gesture arena, the navigator stack, the sliver scrolling protocol — all of it, in idiomatic C#.

This means Flutter developers can bring their knowledge directly to .NET. A `StatefulWidget` in Plumix works the same way as in Flutter. `setState` triggers the same rebuild cycle. `InheritedWidget` propagates data the same way. The goal is that porting a Flutter widget to Plumix is a mechanical translation, not a reimagination.

Under the hood, a host only supplies the platform: on Android and iOS, Plumix's native hosts are ports of the Flutter engine's embeddings — text input, keyboard, accessibility, platform channels, state restoration — that render through Impeller; on desktop and in the browser, Avalonia provides the window, input and Skia drawing backend. Everything above the canvas is framework-owned: layout passes, paint phases, compositing layers, semantics, focus management, gesture recognition.

---

## Key value propositions

- **Flutter developers join .NET without a new mental model.** Same widget architecture, same lifecycle, same patterns — just C# instead of Dart.
- **Mechanical Dart → C# porting.** Plumix's fidelity goal means a Flutter widget can be translated line-by-line into C#. No paradigm shifts.
- **Cross-platform from day one.** Android, iOS, desktop (Windows, macOS, Linux) and the browser (WebAssembly) — one widget tree, one `dotnet new plumix-app`.
- **Native mobile hosts.** Android and iOS render through Impeller, Flutter's own renderer, with no Avalonia or web view in between.
- **.NET 11 and modern C#.** Nullable reference types, records, collection expressions, top-level programs. No legacy baggage.
- **Material Design 3 out of the box.** Full theming system, component library, adaptive widgets.

---

## Feature highlights

### Core framework
- Widget/Element/RenderObject architecture, faithful to Flutter
- `StatelessWidget`, `StatefulWidget`, `State`, `setState`, `BuildContext`
- `InheritedWidget`, `InheritedModel`, `InheritedNotifier`
- Full widget reconciliation loop (`BuildOwner`, `PipelineOwner`)

### Layout & rendering
- `RenderBox` with full 2D box model (min/max constraints)
- Flex layout: `Row`, `Column`, `Flexible`, `Expanded`, `Spacer`
- Stack, positioned, aligned, aspect ratio, fitted box layouts
- Clip, decoration, opacity, transform, offstage render proxies
- Compositing layer tree (offset, opacity, transform, clip)

### Scrolling & lists
- `Scrollable`, `ScrollView`, `SingleChildScrollView`, `CustomScrollView`
- `ListView`, `GridView`, `Viewport`
- Full Sliver protocol: `SliverList`, `SliverGrid`, `SliverPadding`, `SliverFillViewport`
- `ScrollController`, physics, keep-alive

### Gestures & input
- Gesture arena with conflict resolution
- `TapGestureRecognizer`, `LongPressGestureRecognizer`, `DragGestureRecognizer`
- `GestureDetector`, `Listener`, `PointerRouter`
- Full focus system: `Focus`, `FocusScope`, tab traversal

### Navigation
- `Navigator` with push/pop/replace semantics
- Typed and named routes, `ModalRoute`, `PageRoute`
- `NavigatorObserver`, back button handling
- Hero transitions with custom flight shuttles

### Accessibility & semantics
- Semantics annotations on all framework widgets
- TalkBack and VoiceOver through the native hosts' ports of the engine's accessibility bridges; Avalonia automation peers on desktop

### Material library
- Every widget family of Flutter's Material library: structure (`Scaffold`, `AppBar`, `Drawer`, navigation bars and rails), buttons, inputs and selection, pickers, dialogs and sheets, menus, chips, `DataTable`, progress and feedback
- Material 2 and Material 3 theming: `ThemeData`, `ColorScheme`, per-component themes
- The 8,825-icon `Icons` catalog and localizations for 119 locales

### Cupertino library
- Strict port of Flutter's Cupertino library (M6, closed 2026-10-27): `CupertinoApp`, navigation and tab bars, pickers, text fields, sheets, context menus, iOS-style routes; Material `.Adaptive` factories compose the Cupertino widgets as Flutter does

### Platform targets
| Platform | Host | Status |
|----------|------|--------|
| Android 7.0+ | `Plumix.Android` — native, Impeller | ✅ Supported |
| iOS 15+ | `Plumix.iOS` — native, Impeller on Metal | ✅ Supported |
| Windows, macOS, Linux | `Plumix.Avalonia` — Avalonia, Skia | ✅ Supported |
| Browser (WASM) | `Plumix.Avalonia` — Avalonia.Browser | ✅ Supported |

---

## NuGet packages

| Package | Description |
|---------|-------------|
| [`Plumix`](https://www.nuget.org/packages/Plumix/) | Core framework — Widget/Element/RenderObject architecture, layout, gestures, navigation, scrolling |
| [`Plumix.Material`](https://www.nuget.org/packages/Plumix.Material/) | Material Design 3 component library and theming |
| [`Plumix.Cupertino`](https://www.nuget.org/packages/Plumix.Cupertino/) | iOS/macOS Cupertino-style adaptive components |
| [`Plumix.Android`](https://www.nuget.org/packages/Plumix.Android/) | Native Android host on Impeller |
| [`Plumix.iOS`](https://www.nuget.org/packages/Plumix.iOS/) | Native iOS host on Impeller and Metal |
| [`Plumix.Avalonia`](https://www.nuget.org/packages/Plumix.Avalonia/) | Avalonia host for desktop and browser |
| [`Plumix.Templates`](https://www.nuget.org/packages/Plumix.Templates/) | `dotnet new plumix-app` project template |

Start a new app:
```
dotnet new install Plumix.Templates
dotnet new plumix-app -n HelloPlumix
```

---

## Current status

**Version:** `1.0.0-preview.9` (with `Plumix.Templates`)
**Phase:** M1–M6 done; native Android and iOS hosts shipped (`1.0.0-preview.6`–`.9`); next — `v1.0.0-rc.1`, the API freeze

| Area | Status |
|------|--------|
| Core framework (Widget/Element/RenderObject) | ✅ Complete |
| Render pipeline (layout/paint/compositing/semantics) | ✅ Complete |
| Gesture system | ✅ Complete |
| Navigation (routes, hero transitions, observers) | ✅ Complete |
| Scrolling & slivers | ✅ Complete |
| Focus & accessibility | ✅ Complete |
| Text input & editing | ✅ Complete |
| Material Design 3 library | ✅ Complete (M4) |
| Cupertino library | ✅ Complete (M6) |
| Native Android and iOS hosts | ✅ Shipped (`Plumix.Android`, `Plumix.iOS`) |
| Cross-host parity and stability | ✅ Complete (M5) |
| Project template (`dotnet new plumix-app`) | ✅ Shipped (`Plumix.Templates`) |
| v1.0 release candidate (API freeze) | 📋 Planned |

---

## Tech stack

- **Language:** C#, .NET 11
- **Hosts:** Plumix's native Android and iOS hosts on [Impeller](https://docs.flutter.dev/perf/impeller) (through NImpeller bindings); [Avalonia](https://avaloniaui.net/) on desktop and in the browser
- **Target frameworks:** `net11.0`; the hosts also `net11.0-android` and `net11.0-ios`
- **Reference implementation:** [Flutter](https://flutter.dev/) (Dart source used as porting source of truth)
- **Packages:** Published to NuGet

---

## Who is it for?

- **Flutter / Dart developers** moving to .NET who want to keep their mental model
- **.NET developers** who want Flutter's architecture without leaving C#
- **Teams** building cross-platform desktop/web/mobile apps on .NET
- **Developers** who want to port existing Flutter widgets to a C# ecosystem

---

## Who is building it?

Plumix is an open source project. Contributions, bug reports, and discussion are welcome via GitHub.

- GitHub: https://github.com/Plumix-Net/Plumix
- Additional packages: https://github.com/Plumix-Net/Plumix.Packages
- Website: https://plumix.net/

---

## Code example

```csharp
using Plumix.Material;
using Plumix.Rendering;
using Plumix.Widgets;

public sealed class CounterApp : StatefulWidget
{
    public override State CreateState() => new _CounterState();
}

sealed class _CounterState : State<CounterApp>
{
    int _count = 0;

    public override Widget Build(BuildContext context)
    {
        return new Scaffold(
            appBar: new AppBar(title: new Text("Plumix Counter")),
            body: new Center(
                child: new Column(
                    mainAxisAlignment: MainAxisAlignment.Center,
                    children:
                    [
                        new Text("You have pushed the button this many times:"),
                        new Text(
                            _count.ToString(),
                            style: Theme.Of(context).TextTheme.DisplaySmall
                        ),
                    ]
                )
            ),
            floatingActionButton: new FloatingActionButton(
                onPressed: () => SetState(() => _count++),
                child: new Icon(Icons.Add)
            )
        );
    }
}
```

---

## Comparison with alternatives

| | Plumix | MAUI | Avalonia | Uno Platform |
|-|--------|------|----------|--------------|
| Flutter parity | ✅ Architecture-level | ❌ | ❌ | ❌ |
| Dart → C# porting | ✅ Mechanical | ❌ | ❌ | ❌ |
| Owns render pipeline | ✅ | Partial | Partial | Partial |
| Material Design 3 | ✅ | ✅ | Community | ✅ |
| .NET 11 | ✅ | ✅ | ✅ | ✅ |
| WASM | ✅ | ✅ | ✅ | ✅ |
| Mobile | ✅ | ✅ | ✅ | ✅ |
