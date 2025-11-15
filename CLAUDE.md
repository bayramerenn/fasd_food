# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a React Native mobile application built with Expo Router and TypeScript. The app uses file-based routing and supports iOS, Android, and web platforms. The project is configured with React 19.1.0, React Native 0.81.5, and Expo SDK 54.

## Development Commands

### Starting the Development Server
```bash
npm start          # Start Expo dev server
npm run android    # Start on Android emulator
npm run ios        # Start on iOS simulator
npm run web        # Start web version
```

### Code Quality
```bash
npm run lint       # Run ESLint
```

### Utilities
```bash
npm run reset-project  # Moves starter code to app-example and creates blank app directory
```

## Architecture

### Routing Structure
This app uses Expo Router with file-based routing:
- `app/_layout.tsx` - Root layout with navigation theme provider and Stack navigator
- `app/(tabs)/_layout.tsx` - Tab navigation layout (bottom tabs)
- `app/(tabs)/index.tsx` - Home tab screen
- `app/(tabs)/explore.tsx` - Explore tab screen
- `app/modal.tsx` - Modal screen (accessed via Stack navigation)

The `unstable_settings.anchor` in `app/_layout.tsx` is set to `'(tabs)'`, making the tab navigator the initial route.

### Theming System
The app has a comprehensive theming system that supports light and dark modes:

**Theme Definition**: `constants/theme.ts` exports `Colors` object with light/dark color schemes and platform-specific `Fonts` configurations.

**Theme Hooks**:
- `use-color-scheme.ts` - Detects system color scheme (has separate `.web.ts` implementation)
- `use-theme-color.ts` - Hook that resolves theme-aware colors from the Colors constant

**Themed Components**: `ThemedText` and `ThemedView` automatically adapt to the current color scheme using the `useThemeColor` hook. Both accept optional `lightColor` and `darkColor` props to override defaults.

### Component Organization
- `components/` - Reusable components
  - `themed-*.tsx` - Theme-aware wrapper components
  - `haptic-tab.tsx` - Tab button with haptic feedback
  - `parallax-scroll-view.tsx` - Scroll view with parallax header effect
  - `ui/` - UI primitives (IconSymbol, Collapsible, etc.)

### Path Aliases
The project uses `@/*` as an alias for the root directory (configured in `tsconfig.json`).

## Key Configuration Details

### Expo Configuration (app.json)
- App uses the new architecture (`newArchEnabled: true`)
- Typed routes enabled (`experiments.typedRoutes: true`)
- React compiler enabled (`experiments.reactCompiler: true`)
- Edge-to-edge mode on Android (`edgeToEdgeEnabled: true`)
- Custom URL scheme: `fastfood://`

### TypeScript
- Strict mode enabled
- Uses Expo's base TypeScript config
- All `.ts` and `.tsx` files are included, plus `.expo/types/**/*.ts`

### Platform-Specific Files
Components may have platform-specific implementations using extensions like `.ios.tsx` and `.web.ts` (e.g., `use-color-scheme.web.ts`, `icon-symbol.ios.tsx`).
