# Navbar Component Documentation

## Overview

This SvelteKit component implements a navigation bar (navbar) that allows users to navigate between different sections of the application. It uses a menu-style structure with buttons for each main section and displays a "Coming Soon" badge for sections that are not yet available.

## Dependencies

- SvelteKit UI components (`Menubar`, `Button`, `Badge`)
- Svelte state management (`$props`)
- Tailwind CSS for styling

## Core Functionality

### Props Interface

```typescript
{
    currentPage: string;  // Indicates the currently active page
}
```

### State Management

The component uses Svelte's `$props` to receive the `currentPage` property from the parent component. This is used to highlight the active page in the navbar. The component does not manage any internal state.

### Page Navigation

- Uses an anchor tag (`<a>`) within each button for navigation
- The `href` attribute is dynamically set based on the `currentPage` prop
- If the `currentPage` matches the button's destination, the `href` is set to "#" to prevent page refresh
- Otherwise, the `href` directs to the corresponding application route, such as `/` for Home and `/practice` for Practice.

### Navigation Links
- "Home" links to `/` or stays on page if selected
- "Practice" links to `/practice` or stays on page if selected
- "Lessons" is a placeholder that displays "Coming Soon" badge and has no functional routing

## UI Components

### Menubar Container

- Uses a `Menubar.Root` component to manage the navigation menu.
- Provides consistent spacing using Tailwind classes for padding (`pb-6 pl-10 pr-10 pt-6`).
- Adds a drop shadow effect using the `drop-shadow-glow` class.

### Navigation Buttons

- Uses `Button` components from `shadcn/ui` with the `variant="ghost"` style for a subtle look.
- Applies the `navbarLink` class to style navigation items.
- Highlights the active page with bold text (`font-bold`) by dynamically adding the `font-bold` class based on the `currentPage` prop.
- Contains anchor tags (`<a>`) for navigation.

### Coming Soon Badge

- Uses a `Badge` component with `variant="destructive"` style to indicate a section is not yet available.
- Positioned using the `ml-2` class (margin left).

## Layout and Styling

- Consistent spacing using Tailwind CSS classes (`pb-6`, `pl-10`, `pr-10`, `pt-6`).
- Clear visual hierarchy with semibold text using the `font-bold` class for the active page.
- Subtle appearance with the `ghost` button style and consistent font weight for inactive navigation buttons.
- Drop shadow for visual separation from page content.

## Usage

```svelte
<Navbar currentPage="Home"></Navbar>
```

or

```svelte
<Navbar currentPage="Practice"></Navbar>
```

### State Updates

- Component does not manage internal state; all changes occur in parent component through `currentPage`
- Parent components update the `currentPage` prop, which triggers a re-render and updates the highlighted button.
- Navigation links update based on `currentPage`.

### Customization

- Styling can be modified through Tailwind classes in the `navbarLink` class.
- Buttons and badges can be changed or replaced using different components or themes.
- More links and sections can be added by adding more navigation buttons.

## Best Practices

1. Always provide a `currentPage` prop when using the `Navbar` component.
2. Use consistent class names for navigation link and active link styling.
3. Maintain proper navigation functionality by dynamically setting the `href` attributes based on the `currentPage` property.
4. If adding more navigation links, ensure the `currentPage` logic is updated to correctly highlight the active link.
5. Ensure your `currentPage` prop is valid based on your routes.
6. When developing sections, remove badge once the section is functional and the menu link should be active.

## Implementation Notes

- The anchor tags (`<a>`) within the buttons are used for navigation to different sections of the application.
- The `currentPage` prop is used for indicating the active section of the page and updating the styling.
- The "Coming Soon" badge serves as an indication to users of sections that are not yet functional, which helps manage user expectations.
- `variant="ghost"` style gives the buttons a cleaner look and avoids visual distraction, especially when users are not interacting with the links.

This detailed documentation should be very helpful for anyone using or modifying the `Navbar` component. If there's anything specific you'd like to delve into further, please let me know.
