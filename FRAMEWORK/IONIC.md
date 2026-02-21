# IONIC

Guidance for AI agents implementing and reviewing Ionic applications.

## Scope
- Define Ionic-specific UI, navigation, native-bridge, and mobile-delivery
  guardrails.
- Apply this file to Ionic Angular and Ionic React projects.

## Semantic Dependencies
- Inherit framework-layer contract from `FRAMEWORK/FRAMEWORK.md`.
- Inherit JavaScript baseline from `LANGUAGE/JAVASCRIPT/JAVASCRIPT.md`.
- Apply `LANGUAGE/TYPESCRIPT/TYPESCRIPT.md` when the Ionic project uses
  TypeScript.
- Inherit HTML/CSS semantics and accessibility from
  `LANGUAGE/HTML/HTML.md` and `LANGUAGE/CSS/CSS.md`.
- When using Ionic Angular, also apply `FRAMEWORK/ANGULAR.md`.
- When using Ionic React, also apply `FRAMEWORK/REACT.md`.
- Inherit cross-cutting constraints from
  `SECURITY/SECURITY.md`, `TEST/TEST.md`, and `CORE/LOGGING.md`.

## Defaults and Guardrails
- Prefer Ionic with Capacitor for modern mobile and web delivery.
- Prefer official Ionic UI components before creating custom equivalents.
- Keep UI concerns in pages/components and platform integration in dedicated
  services/adapters.
- Keep native plugin usage behind small abstractions to simplify testing and
  platform fallbacks.
- Keep loading, empty, offline, and error states explicit for each critical
  mobile flow.
- Keep permission usage minimal and only request permissions when needed by a
  user-initiated action.

## Navigation and Lifecycle
- Keep route definitions explicit and feature-oriented.
- Use Ionic page lifecycle hooks only for page-level side effects that cannot
  be modeled cleanly with framework-native patterns.
- Ensure navigation-triggered async work is cancelable to avoid stale updates
  after fast route changes.
- Keep back-navigation behavior deterministic across iOS, Android, and web.

## Native Bridge and Plugin Policy
- Prefer official Capacitor plugins before third-party plugins.
- Validate plugin maintenance, platform support, and permission footprint
  before adoption.
- Keep platform checks centralized in service boundaries, not scattered through
  page components.
- Provide graceful fallback behavior when a capability is unavailable on web
  or in restricted device contexts.

## Performance and UX
- Keep first-screen rendering lightweight and lazy-load deeper route trees.
- Avoid unnecessary re-renders and large synchronous work on interaction paths.
- Keep list rendering efficient (`trackBy`/stable keys, incremental loading).
- Keep touch interactions responsive and avoid blocking animations with heavy
  synchronous logic.

## Security
- Do not render untrusted HTML in Ionic views.
- Do not persist secrets in plaintext storage.
- Keep token/session storage strategy aligned with project security policy.
- Validate deep-link and external-intent inputs before navigation or execution.

## High-Risk Pitfalls
1. Direct plugin calls spread across page components.
2. Navigation race conditions that update disposed screens.
3. Missing fallback behavior for unavailable native capabilities.
4. Over-requesting permissions at app start.
5. Large first-load bundles from eager feature imports.
6. Unvalidated deep-link parameters used for navigation decisions.

## Do / Don't Examples
### 1. Native Plugin Access Boundary
```ts
// Don't: call plugins directly from page components.
export class CameraPageBad {
  async takePhoto(): Promise<void> {
    const result = await Camera.getPhoto({ quality: 80 });
    this.preview = result.webPath ?? "";
  }
}

// Do: isolate plugin access in a service boundary.
export class CameraService {
  async takePhoto(): Promise<string | null> {
    const result = await Camera.getPhoto({ quality: 80 });
    return result.webPath ?? null;
  }
}
```

### 2. Route-Triggered Async Safety
```ts
// Don't: ignore stale async updates during route switches.
ionViewWillEnter(): void {
  this.ordersApi.loadOpenOrders().then((orders) => {
    this.orders = orders;
  });
}

// Do: cancel/ignore stale work on view leave.
private alive = false;

ionViewWillEnter(): void {
  this.alive = true;
  void this.ordersApi.loadOpenOrders().then((orders) => {
    if (!this.alive) return;
    this.orders = orders;
  });
}

ionViewDidLeave(): void {
  this.alive = false;
}
```

### 3. Platform Checks
```ts
// Don't: scatter platform checks across UI code.
if (Capacitor.getPlatform() === "android") { /* ... */ }
if (Capacitor.getPlatform() === "ios") { /* ... */ }

// Do: centralize platform branching in one adapter.
export function isNativeMobile(): boolean {
  const platform = Capacitor.getPlatform();
  return platform === "ios" || platform === "android";
}
```

## Code Review Checklist for Ionic
- Are Ionic UI components used consistently before custom replacements?
- Are native plugin calls isolated behind service/adapter boundaries?
- Are permission prompts minimal and user-action driven?
- Are route/lifecycle side effects cancel-safe?
- Are web/device fallbacks defined for native capability gaps?
- Are deep-link/external-intent inputs validated?
- Are first-screen load and interaction paths performance-aware?

## Testing Guidance
- Follow general testing policy in `TEST/TEST.md`.
- Test route transitions for stale async updates and teardown safety.
- Test native-capability fallbacks on web and unsupported device contexts.
- Test permission-denied flows and user recovery paths.
- Test deep-link validation and unauthorized navigation protection.
- Test list-heavy screens on low-end device emulation for responsiveness.

## Override Notes
- Project-specific mobile requirements may add stricter delivery constraints,
  but plugin-boundary isolation, permission discipline, and fallback safety
  remain mandatory.
