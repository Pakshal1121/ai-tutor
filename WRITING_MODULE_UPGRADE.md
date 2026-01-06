# Writing Module Upgrade - Complete Implementation Guide

## Overview
The Writing module has been upgraded to match the Speaking module's conversational, human-like coaching philosophy.

## New Routing Structure

```
/writing                          → Mode selection (NEW)
/writing/examiner                 → Examiner mode (EXISTING - unchanged)
/writing/coach                    → Coach welcome (UPGRADED)

/writing/coach/task-1/
  ├── understand/                 → Explain the task (NEW)
  ├── plan/                       → Planning guidance (NEW)
  ├── write/                      → Supported writing (NEW)
  └── feedback/                   → Explanation-first feedback (NEW)

/writing/coach/task-2/
  ├── understand/                 → Explain the task (NEW)
  ├── plan/                       → Planning guidance (NEW)
  ├── write/                      → Supported writing (NEW)
  └── feedback/                   → Explanation-first feedback (NEW)
```

## Files Created/Modified

### ✅ COMPLETED
1. `/app/writing/page.tsx` - Mode selection page
2. `/app/writing/coach/page.tsx` - Upgraded welcome with selections
3. `/app/writing/coach/task-1/understand/page.tsx` - Task explanation page

### 📋 TO BE IMPLEMENTED (Following the same pattern)
4. `/app/writing/coach/task-1/plan/page.tsx`
5. `/app/writing/coach/task-1/write/page.tsx`
6. `/app/writing/coach/task-1/feedback/page.tsx`
7. `/app/writing/coach/task-2/understand/page.tsx`
8. `/app/writing/coach/task-2/plan/page.tsx`
9. `/app/writing/coach/task-2/write/page.tsx`
10. `/app/writing/coach/task-2/feedback/page.tsx`

## Design Patterns

### Color Scheme
- **Coach Mode**: Emerald (#10B981) - friendly, supportive
- **Examiner Mode**: Blue (#3B82F6) - formal, professional

### Layout Pattern
```
┌─────────────────────────────────────────┐
│  Progress Bar (Step X of 4)            │
├─────────────────────────────────────────┤
│  Page Title & Description               │
├───────────┬─────────────────────────────┤
│  Avatar   │  Main Content Area          │
│  Coach    │  - Task/Question            │
│  Tips     │  - Explanations             │
│  Sticky   │  - Interactive Elements     │
│           │  - AI Placeholder Sections  │
└───────────┴─────────────────────────────┘
```

### Component Reuse
- ✅ `AvatarStage` - Coach presence
- ✅ `PrimaryButton` - CTAs and navigation
- ✅ Framer Motion - Smooth transitions
- ✅ Tailwind CSS - Consistent styling

## Key Features by Screen

### 1. Mode Selection (`/writing`)
- Two cards: Examiner vs Coach
- Clear value proposition for each
- Matches Speaking module pattern

### 2. Coach Welcome (`/writing/coach`)
- Target band selection (6/7/8+)
- Task selection (Task 1 / Task 2)
- Focus area selection (Structure/Vocabulary/Coherence/Grammar)
- Friendly avatar and conversational copy

### 3. Understand Screen
**Purpose**: Explain like a human teacher

**Sections**:
- Task display with highlighted keywords
- "What this question is really asking" breakdown
- Numbered steps with visual cards
- Common mistakes section (amber warning boxes)
- AI diagram placeholder (purple gradient card)

**Coach Language Examples**:
- "Let me break down what the examiner wants..."
- "Pay attention to these important words..."
- "Many students make this mistake..."

### 4. Plan Screen
**Purpose**: Teach planning before writing

**Sections**:
- Overview statement input
- Key features to describe (textarea inputs)
- Example outline (expandable)
- Useful language suggestions
- Coach tips: "This is how Band 7+ answers are planned"

**Interactive Elements**:
- Text inputs for planning notes
- Toggle to show/hide example outlines
- Vocabulary helpers

### 5. Write Screen
**Purpose**: Supported writing, not judgment

**Sections**:
- Large writing textarea
- Paragraph indicators/counters
- Side panel with:
  - Vocabulary suggestions (collapsible)
  - Example sentences
  - Real-time coach hints
- Word count tracker
- NO error markers, NO red underlines

**Coach Support**:
- Encouraging language
- Contextual tips appear as user writes
- "You're doing well" style feedback

### 6. Feedback Screen
**Purpose**: Explanation-first, not scoring

**Sections**:
- Overall qualitative feedback (no numbers)
- Paragraph-by-paragraph breakdown
- "Why this works" explanations (emerald cards)
- "How to reach next band" tips (blue cards)
- Improved example snippets
- Rewrite button (encourages iteration)

**NO**:
- ❌ Numeric scores
- ❌ Harsh criticism
- ❌ Red error markers
- ❌ Judgmental language

## Implementation Guidelines

### For Each New Page:

1. **Progress Indicator** (top of page)
   ```tsx
   <div className="w-full h-2 bg-slate-200 rounded-full">
     <div className="h-full w-X/4 bg-emerald-600 rounded-full" />
   </div>
   ```

2. **Three-Column Layout**
   - Left: Avatar + sticky coach tips (1/3 width on desktop)
   - Right: Main content scrollable area (2/3 width)

3. **Avatar States**
   - Understand: `state="speaking"` (explaining)
   - Plan: `state="thinking"` (planning together)
   - Write: `state="idle"` (present but not intrusive)
   - Feedback: `state="thinking"` (analyzing)

4. **Navigation Pattern**
   ```tsx
   <div className="flex justify-between">
     <PrimaryButton onClick={previousStep} variant="ghost">
       ← Back
     </PrimaryButton>
     <PrimaryButton onClick={nextStep} className="bg-emerald-600">
       Continue →
     </PrimaryButton>
   </div>
   ```

5. **AI Placeholder Sections**
   Always include comment:
   ```tsx
   {/* TODO: Backend AI Integration Point
       This section will display AI-generated:
       - Explanations
       - Examples
       - Feedback
   */}
   ```

## Task 1 vs Task 2 Differences

### Task 1 (Graph Description)
- Focus: Data interpretation
- Keywords: summarise, main features, comparisons
- Planning: Overview + key data points
- Language: Comparative, descriptive, objective

### Task 2 (Essay)
- Focus: Argument development
- Keywords: discuss, opinion, agree/disagree
- Planning: Thesis + body paragraphs + conclusion
- Language: Argumentative, examples, reasoning

## State Management

All user inputs should be stored in React state:
```tsx
const [targetBand, setTargetBand] = useState<number | null>(null);
const [selectedTask, setSelectedTask] = useState<'task-1' | 'task-2' | null>(null);
const [focusArea, setFocusArea] = useState<string | null>(null);
const [userEssay, setUserEssay] = useState('');
const [planningNotes, setPlanningNotes] = useState({});
```

## Responsive Design

- **Desktop**: 3-column layout (avatar | main content)
- **Tablet**: 2-column with narrower avatar panel
- **Mobile**: Single column, avatar at top

## Animation Guidelines

Use Framer Motion sparingly:
- Page entry: `initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }}`
- Stagger delays: `transition={{ delay: 0.1 * index }}`
- Hover: `whileHover={{ scale: 1.02 }}`
- Tap: `whileTap={{ scale: 0.98 }}`

## Copy Tone Guidelines

### ✅ DO Use:
- "Let's work together..."
- "I'll help you understand..."
- "Many students find this tricky..."
- "Here's why this works..."
- "You're making good progress..."

### ❌ DON'T Use:
- "You must..."
- "This is wrong..."
- "You failed to..."
- "Incorrect approach..."
- Technical jargon without explanation

## Testing Checklist

- [ ] Mode selection works
- [ ] Coach welcome stores all selections
- [ ] Navigation flows logically (understand → plan → write → feedback)
- [ ] Avatar states match context
- [ ] Progress bar updates correctly
- [ ] All links work (no 404s)
- [ ] Mobile responsive
- [ ] Color scheme consistent (emerald for coach)
- [ ] No Lorem Ipsum text
- [ ] All AI placeholders commented

## Next Steps

1. Implement remaining coach pages following the pattern in `task-1/understand/`
2. Test full user journey
3. Add Task 2 pages (similar structure, different content)
4. Polish copy and coach tips
5. Add more AI placeholder sections
6. Prepare for backend integration

## Backend Integration Points

When backend is ready, these sections will need API connections:
1. **Understand**: AI-generated task breakdowns, diagram generation
2. **Plan**: AI planning suggestions, outline feedback
3. **Write**: Real-time vocabulary suggestions, grammar hints
4. **Feedback**: Actual AI scoring, detailed paragraph analysis

