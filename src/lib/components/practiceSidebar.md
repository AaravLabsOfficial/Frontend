# PracticeSidebar Documentation

## Overview
This SvelteKit component implements a sidebar with a question filter that allows users to select math topics and difficulty levels. It provides dropdown menus for filtering math practice questions based on subject area and complexity level.

## Dependencies
- SvelteKit UI components (Card, Select)
- Svelte state management ($bindable, $derived, $props)
- Tailwind CSS for styling

## Core Functionality

### Props Interface
```typescript
{
    topic: string;      // Selected math topic
    difficulty: string; // Selected difficulty level
}
```

### State Management
The component uses Svelte's state management features:
- Bindable props for topic and difficulty selections
- Derived state for displaying human-readable labels
- Select components for user interaction

### Topic Management
```typescript
const topics = [
    { value: 'algebra', label: 'Algebra' },
    { value: 'intermediate_algebra', label: 'Intermediate Algebra' },
    // ...
];
```
- Maintains mapping between internal values and display labels
- Supports 7 specific math topics plus an "All" option
- Uses derived state to display current selection

### Difficulty Management
```typescript
const difficulties = [
    { value: '1', label: '1' },
    { value: '2', label: '2' },
    // ...
];
```
- Provides 5 difficulty levels plus an "All" option
- Uses consistent value-label pattern for mapping
- Implements derived state for current selection display

## UI Components

### Card Container
- Uses shadcn/ui Card component
- Fixed width with minimum of 96 units
- Implements glow effect shadow
- Maintains consistent margins

### Topic Selector
- Uses shadcn/ui Select component
- Provides dropdown interface for topic selection
- Shows current selection in trigger button
- Lists all available topics in dropdown menu

### Difficulty Selector
- Matches Topic Selector implementation
- Numerical difficulty levels (1-5)
- Includes "All" option for unfiltered results
- Updates parent state on selection

## Layout and Styling
- Consistent spacing using Tailwind classes
- Clear section headers with semibold text
- Responsive layout within fixed width
- Visual hierarchy through font sizes and weights

## Usage
```svelte
<PracticeSidebar
    topic={topicState}
    difficulty={difficultyState}
/>
```

### State Updates
- Component emits value changes through bindable props
- Parent components can react to filter changes
- Maintains internal state for display purposes

### Customization
- Topic and difficulty lists can be modified
- Styling can be adjusted through Tailwind classes
- Card layout can be adapted to different contexts

## Best Practices
1. Always provide both topic and difficulty props
2. Use consistent value formats for filter options
3. Maintain mapping between values and labels
4. Handle "All" selection appropriately in parent components

## Implementation Notes
- Values are kept separate from display labels for internationalization
- Derived state ensures consistent display updates
- Select components handle all interaction logic
- Card component provides consistent visual framework