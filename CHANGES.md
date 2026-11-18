# Guiced IntelliJ Plugin — 2.6.0.1 (unofficial community build)

> **Non-official build.** This release is maintained by Jerrymkjuju in a personal fork.
> It is not published, endorsed, or supported by the GuicedEE organization.
> It uses the same plugin ID (`com.guicedee.guiced`) as the upstream plugin and therefore
> **replaces** it; do not attempt to install both at the same time.

## Source and license

- Upstream project: https://github.com/GuicedEE/GuicedEEIntelliJPlugin
- Upstream license: Apache License 2.0 — see [LICENSE](LICENSE)
- This fork: https://github.com/Jerrymkjuju/GuicedEEIntelliJPlugin
- All changes below are distributed under the same Apache-2.0 terms.
- Every file modified for this build carries a prominent `MODIFIED by Jerrymkjuju on 2026-11-18` notice.

## What changed in 2.6.0.1

Based on upstream `master` at `ba40a12` (2.6.0 Preparation), plus the fix for upstream
[issue #1](https://github.com/GuicedEE/GuicedEEIntelliJPlugin/issues/1), "No suggestions for unused fields":

1. **`GuiceImplicitUsageProvider`**
   - Injection points are restricted to `PsiField`, `PsiMethod` and `PsiParameter`;
     entry points to methods/classes; `@Provides` to methods only.
   - Annotations are checked directly on the element's own modifier list instead of
     `AnnotationUtil`, so a class-level annotation can no longer mark an entire class
     (and all of its private members) as implicitly used.
   - Injection points are **no longer** claimed by `isImplicitUsage`. The platform treats
     every implicitly used element as an entry point and skips the "N usages" code-vision
     hint entirely, including the `no usages` case.
   - `isImplicitRead` / `isImplicitWrite` are kept for injection points to avoid the
     secondary "never read" / "never assigned" warnings.

2. **`GuiceUnusedDeclarationSuppressor`** (new) and its `plugin.xml` registration
   - Suppresses only the `unused` tool on injection points. Because it does not go through
     the platform's entry-point check, injected fields keep their reference count:
     `no usages` when nothing references them, `N usages` when they are used.

3. **Tests**
   - Three new test classes (16 tests) cover the narrowed detection, the suppressor, and
     platform-level unused highlighting. Full suite: **43 tests, 0 failures, 0 errors**.

## Compatibility

- `since-build = 252` (IntelliJ IDEA 2025.2+), no upper bound.
- Verified on IntelliJ IDEA Ultimate `IU-263.7307.200` (2026.3).

## Install

`Settings → Plugins → gear icon → Install Plugin from Disk…` → select the
`GEEIntelliJPlugin-2.6.0.1.zip` release asset → **Restart IDE**.

If you had a previous local build installed, uninstall it first or confirm the replacement
prompt. Report build-specific issues at https://github.com/Jerrymkjuju/GuicedEEIntelliJPlugin/issues.
