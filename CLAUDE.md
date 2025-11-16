# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **fast food ordering mobile application** built with Expo Router, TypeScript, and NativeWind (Tailwind CSS for React Native). The app uses file-based routing and supports iOS, Android, and web platforms. The project is configured with React 19.1.0, React Native 0.81.5, and Expo SDK 54.

**Application Features**:
- Food product categories (Burger, Pizza, Wrap, Burrito)
- Customizable orders with toppings (Avocado, Bacon, Cheese, etc.)
- Side dish selection (Fries, Onion Rings, etc.)
- Promotional offers system

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
- `app/_layout.tsx` - Root layout with Stack navigator and font loading
- `app/(tabs)/` - Tab-based navigation group (home, cart, search, profile)
- `app/(auth)/` - Authentication screens group (sign-in, sign-up)
- `app/globals.css` - Global Tailwind CSS styles (imported in index.tsx)

**Font Loading**: The root layout (`app/_layout.tsx`) loads 5 Quicksand font weights using `expo-font` and `useFonts` hook. The splash screen remains visible until fonts are loaded.

### Styling with NativeWind
The app uses **NativeWind v4** for styling, which brings Tailwind CSS to React Native:

**Configuration Files**:
- `tailwind.config.js` - Tailwind configuration with NativeWind preset
- `metro.config.js` - Metro bundler configured with `withNativeWind()` wrapper
- `app/globals.css` - Contains Tailwind directives (`@tailwind base/components/utilities`)

**Usage**: Use Tailwind className directly on React Native components:
```tsx
<View className="flex-1 items-center justify-center bg-white">
  <Text className="text-xl font-bold text-blue-500">Hello</Text>
</View>
```

**Content Paths**: Tailwind scans `./app/**/*.{js,jsx,ts,tsx}` and `./components/**/*.{js,jsx,ts,tsx}` for class names.

**Critical Babel Configuration**: NativeWind requires specific Babel setup in `babel.config.js`:
- `jsxImportSource: "nativewind"` in babel-preset-expo
- `nativewind/babel` preset

**Metro Configuration**: The Metro bundler is configured with NativeWind integration in `metro.config.js`:
- Uses `withNativeWind()` wrapper around the default Expo config
- Input CSS file specified as `./app/globals.css`

### Custom Design System
The project has a custom Tailwind theme configured in `tailwind.config.js`:

**Colors**:
- `primary`: `#FE8C00` (orange) - Main brand color
- `error`: `#F14141` - Error states
- `success`: `#2F9B65` - Success states
- Custom gray palette: `gray-100`, `gray-200`
- `dark-100`: Dark mode color

**Typography**:
- Font family: **Quicksand** (5 weights available)
- Usage: `font-quicksand`, `font-quicksand-bold`, `font-quicksand-semibold`, `font-quicksand-light`, `font-quicksand-medium`
- Font files located in `assets/fonts/`

**Custom CSS Classes** (defined in `app/globals.css`):
- **Utility classes**: `flex-center`, `flex-between`, `flex-start`
- **Typography classes**: `h1-bold`, `h3-bold`, `base-bold`, `base-semibold`, `base-regular`, `paragraph-bold`, `paragraph-semibold`, `paragraph-medium`, `body-medium`, `body-regular`, `small-bold`
- **Component classes**: `cart-btn`, `cart-badge`, `cart-item`, `custom-btn`, `custom-header`, `label`, `input`, `filter`, `menu-card`, `profile-field`, `searchbar`, `tab-icon`, `offer-card`, `profile-avatar`

### Path Aliases
The project uses `@/*` as an alias for the root directory (configured in `tsconfig.json`).

### Asset Management
Assets are centrally managed through `constants/index.ts`:
- All icons and images are imported and exported as the `images` object
- Static data structures: `CATEGORIES`, `offers`, `sides`, `toppings`
- Import pattern: `import { images, CATEGORIES, offers } from '@/constants'`
- Type declarations for image imports in `images.d.ts` (.png, .jpg, .jpeg, .gif, .svg)

### Component Structure
Components are organized in the `components/` directory:
- Components use the `@/constants` import for accessing images and data
- Components follow React Native patterns with NativeWind styling
- Example: `CartButton.tsx` demonstrates the component pattern with custom CSS classes

## Key Configuration Details

### Expo Configuration (app.json)
- App uses the **new architecture** (`newArchEnabled: true`) - Fabric renderer and TurboModules enabled
- Typed routes enabled (`experiments.typedRoutes: true`) - Type-safe navigation
- React compiler enabled (`experiments.reactCompiler: true`) - Automatic optimization
- Edge-to-edge mode on Android (`edgeToEdgeEnabled: true`)
- Custom URL scheme: `fastfood://`

### TypeScript
- Strict mode enabled
- Uses Expo's base TypeScript config
- Path alias `@/*` configured for root imports
- Type declarations: `nativewind-env.d.ts`, `images.d.ts`
- All `.ts` and `.tsx` files are included, plus `.expo/types/**/*.ts`

### Platform-Specific Files
Components may have platform-specific implementations using extensions like `.ios.tsx` and `.web.ts`.

### Key Dependencies
- **Navigation**: React Navigation v7 with bottom tabs support (installed but not yet implemented)
- **Animations**: React Native Reanimated v3.17.4 and Gesture Handler v2.28.0
- **Performance**: `react-native-worklets` for UI thread JavaScript execution
- **Image Handling**: `expo-image` for optimized image loading

## Notes

- The original starter code with themed components has been moved to `app-example/` directory
- Current app uses NativeWind for all styling instead of the themed component system
