# Plumix — FAQ

Frequently asked questions for articles, interviews, and website Q&A sections.

_Updated 2026-11-23._

---

**Q: What is Plumix?**

Plumix is a UI framework for .NET that brings Flutter's architecture to C#. It implements the same Widget/Element/RenderObject model that Flutter uses, so developers can build declarative, cross-platform UIs in C# using the exact same patterns they'd use in Flutter/Dart.

---

**Q: How is Plumix different from MAUI, Avalonia, or Uno Platform?**

Those frameworks have their own UI models. Plumix specifically targets Flutter's architecture — its goal is that a Flutter widget can be mechanically ported to C# without redesigning it. On Android and iOS Plumix runs on its own hosts, without Avalonia; on desktop and in the browser Avalonia is the platform layer (windowing, input, GPU drawing). Either way, layout, paint, gestures, navigation, and widget lifecycle are all implemented inside Plumix itself, not delegated to another toolkit's controls.

---

**Q: Do I need to know Flutter to use Plumix?**

No, but it helps. If you know Flutter, Plumix will feel immediately familiar — same widget primitives, same lifecycle, same patterns. If you don't know Flutter, Plumix is still a capable declarative UI framework. The learning curve is similar to learning Flutter itself, and Flutter's extensive documentation applies conceptually.

---

**Q: Does Plumix have its own engine?**

On mobile, yes — in the same sense Flutter does. `Plumix.Android` and `Plumix.iOS` are ports of the Flutter engine's Android and iOS embeddings (text input, keyboard, accessibility, platform channels, state restoration) that render through Impeller, Flutter's own renderer, via the NImpeller bindings. On desktop and in the browser Plumix runs on Avalonia, which already solves native windowing, input and GPU drawing there. In both cases the split matches Flutter's: the framework layer is Plumix's, the host only supplies the platform.

---

**Q: What platforms does Plumix support?**

Android 7.0+ and iOS 15+ (native hosts on Impeller), Windows, macOS and Linux (Avalonia desktop) and WebAssembly (Avalonia browser). `dotnet new plumix-app` creates one shared widget project with desktop, Android and iOS projects.

---

**Q: What is the current status?**

Plumix 1.0 is in preview (`1.0.0-preview.8`). The core framework and the Material library are strict ports of Flutter 3.47 — widget reconciliation, render pipeline, gestures, navigation, scrolling, text input, accessibility. The native Android and iOS hosts have shipped; the Cupertino port and cross-host stability are in progress before a 1.0 release candidate.

---

**Q: Is Plumix production-ready?**

Not yet. Preview versions are suitable for exploration, prototyping, and contributing; the NetBench demo app is built on them. Expect API changes before the 1.0 API freeze.

---

**Q: What is the Dart reference sample in the repo?**

Every C# sample in Plumix has a mirror implementation in `dart_sample/` — the same UI built in Flutter/Dart. This serves as a source of truth for widget behavior. If the C# and Dart samples look and behave the same, the port is correct.

---

**Q: Can I contribute?**

Yes. The project is open source on GitHub: https://github.com/Plumix-Net/Plumix. Bug reports, feature discussions, and pull requests are welcome.

---

**Q: Where do I find additional packages?**

https://github.com/Plumix-Net/Plumix.Packages — community and supplemental packages for the Plumix ecosystem.

---

**Q: What .NET version is required?**

.NET 11. Plumix targets `net11.0`; the mobile hosts also `net11.0-android` and `net11.0-ios`.

---

**Q: How does theming work?**

Plumix implements Material Design 3's theming system: a `ThemeData` object (with `ColorScheme`, text styles, and per-component theme data) propagates via `Theme` InheritedWidget, and all Material components resolve their colors and styles from it. This matches Flutter's theming API closely.

---

**Q: Is there an IDE extension or tooling?**

Not yet. Standard .NET/C# tooling works fine — Visual Studio, VS Code with C# Dev Kit, JetBrains Rider.

---

**Q: What is the license?**

Plumix is licensed under the MIT License — free for both open-source and commercial use. See `LICENSE` in the repository.
