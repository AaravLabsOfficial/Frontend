# Math Practice Component Documentation

## Overview
This SvelteKit component implements an interactive math practice system with LaTeX rendering support. It allows users to solve math problems, submit their answers, and receive feedback through an AI analysis system.

## Dependencies
- SvelteKit UI components (Card, Button, Input)
- KaTeX for math rendering
- Environment variables for API configuration
- Svelte state management ($state, $derived, $effect)

## Core Functionality

### Math Rendering System
```typescript
function renderMathSegment(input: string, displayMode: boolean): string
```
Renders individual math segments using KaTeX with configurable display mode.

```typescript
function renderMath(input: string): string
```
Processes text containing math expressions, supporting multiple formats:
- Inline math: `$...$` and `\(...\)`
- Display math: `$$...$$` and `\[...\]`
Converts newlines to HTML breaks and sequentially processes each math format.

### State Management
The component maintains several reactive states:
- Question-related states:
  - `question`: Current question text
  - `steps`: User's solution steps
  - `answer`: User's final answer
  - `fullAnswer`: Complete solution
  - `renderedQuestion/Steps/Answer`: KaTeX-rendered versions

- Analysis states:
  - `stepsAnalysis`: AI feedback on solution steps
  - `answerAnalysis`: AI feedback on final answer
  - `stepsCorrect`: Boolean indicating steps validity
  - `answerCorrect`: Boolean indicating answer correctness

### Props Interface
```typescript
{
    topic: string;              // Math topic category
    difficulty: string;         // Difficulty level
    fake?: boolean;            // Mock mode flag
    solvedProp?: boolean;      // Solution state
    questionProp?: string;     // Pre-set question
    fullAnswerProp?: string;   // Pre-set solution
    stepsProp?: string;        // Pre-set steps
    answerProp?: string;      // Pre-set answer
    stepsAnalysisProp?: string; // Pre-set steps feedback
    answerAnalysisProp?: string; // Pre-set answer feedback
    stepsCorrectProp?: boolean; // Pre-set steps validity
    answerCorrectProp?: boolean; // Pre-set answer validity
}
```

### API Integration
The component interacts with an external API for:
1. Fetching questions based on topic and difficulty
2. Submitting solutions for AI analysis
3. Receiving feedback on solution steps and final answer

### Key Functions

#### `renderQuestion()`
- Fetches new questions from API
- Filters out questions with complex formatting (ASY/LaTeX environments)
- Updates component state with new question data

#### `solveQuestion()`
- Validates user input (non-empty steps and answer)
- Submits solution to API for analysis
- Updates UI with feedback and correctness indicators

#### `nextQuestion()`
- Resets component state
- Fetches new question in normal mode
- Redirects to practice page in fake mode

## UI States

### Practice Mode
Displays:
- Question with LaTeX rendering
- Topic and difficulty level
- Input fields for steps and answer
- Live LaTeX preview
- Submit and skip buttons

### Review Mode
Shows:
- Correctness indicator
- Original question
- User's steps with feedback
- User's answer with feedback
- Complete solution
- Next question button

## Error Handling
- Input validation for empty steps/answers
- Visual error indicators
- Skips questions with unsupported formatting

## Style Integration
- Uses Tailwind CSS for styling
- Implements conditional styling for feedback (green/red backgrounds)
- KaTeX stylesheet integration for math rendering

## Usage Notes
1. Component requires KaTeX CSS to be loaded in browser environment
2. API URL must be provided via environment variables
3. Supports both real and mock ("fake") modes for display purposes
4. Maintains topic and difficulty state across question changes
5. Implements responsive design for various screen sizes
