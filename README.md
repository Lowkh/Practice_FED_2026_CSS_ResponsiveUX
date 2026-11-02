# Lesson 05: Essential CSS and Responsive UX

> Front-End Development — Year 1, Semester 2  
> Organise content. Arrange it. Adapt it.

This GitHub-ready guide adapts the uploaded lesson slides into step-by-step notes and a worked practical. The Campus Workshop Directory code below is a newly written teaching example, not a copy of starter files supplied with the slides.

## Contents

- [Learning outcomes](#learning-outcomes)
- [Lesson plan](#lesson-plan)
- [Step 1: Understand the box model](#step-1-understand-the-box-model)
- [Step 2: Choose units and establish hierarchy](#step-2-choose-units-and-establish-hierarchy)
- [Step 3: Identify the flex container](#step-3-identify-the-flex-container)
- [Step 4: Arrange and size flex items](#step-4-arrange-and-size-flex-items)
- [Step 5: Add mobile-first media queries](#step-5-add-mobile-first-media-queries)
- [Step 6: Build the Campus Workshop Directory](#step-6-build-the-campus-workshop-directory)
- [Step 7: Test and record the results](#step-7-test-and-record-the-results)
- [Step 8: Debug systematically](#step-8-debug-systematically)
- [Group research](#group-research)
- [Exit check](#exit-check)

## Learning outcomes

By the end of this lesson, you should be able to:

1. Organise content with consistent spacing, text sizes and weights.
2. Explain what a declared width includes using `box-sizing`.
3. Use Flexbox for direction, alignment, wrapping and flexible sizing.
4. Adapt a layout at `768px` and `1024px`.
5. Inspect, test and explain a layout decision and a corrected issue.

## Lesson plan

| Activity | Duration | Intended result |
| --- | --- | --- |
| Explain and demonstrate | 30 min | Recognise the patterns |
| Group research | 25 min | Prepare a finding and example |
| Selected sharing | 25 min | Explain and compare |
| Tutor synthesis | 20 min | Consolidate and debug |
| Practical briefing | 10 min | Understand files and tasks |
| Supported practical | 35 min | Apply the CSS |
| Exit check | 5 min | Explain and continue |

## Step 1: Understand the box model

Every card has content, padding, a border and margin.

| Part | Purpose | Example |
| --- | --- | --- |
| Content | The text inside the card | A heading and description |
| Padding | Space inside the border | `padding: 1rem` |
| Border | The edge of the box | `border: 1px solid #69717a` |
| Margin | Space outside the border | `margin-bottom: 1rem` |

```css
.book-card {
  padding: 1rem;
  border: 1px solid #69717a;
  margin-bottom: 1rem;
}
```

### Explain what width includes

With `content-box`, the declared width covers only the content:

```css
.book-card {
  box-sizing: content-box;
  width: 200px;
  padding: 20px;
  border: 5px solid;
}
```

The border-box width is `200 + 20 + 20 + 5 + 5 = 250px`.

With `border-box`, padding and border fit inside the declared width:

```css
.book-card {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid;
}
```

The border-box width remains `200px`; content width becomes `150px`. Margin remains outside in both cases.

**Checkpoint:** If text touches its own card border, adjust padding, not the gap between cards.

## Step 2: Choose units and establish hierarchy

Use `rem` for the lesson's text and spacing, and `px` for its breakpoints.

- `rem` follows the root (`html`) font size.
- `px` is a CSS pixel.
- Increasing the root font size increases values expressed in `rem`.

```css
body {
  font-family: Arial, sans-serif;
  font-size: 1rem;
  line-height: 1.5;
}

.book-title {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0 0 0.5rem;
}
```

### Build a clear reading order

1. Make the page title the strongest heading.
2. Give all card titles a consistent style.
3. Keep supporting details close together.
4. Separate cards with a consistent gap.
5. Avoid making every line equally large and bold.

`font-size` changes text size; `font-weight` changes its emphasis. Semantic HTML identifies headings, while CSS controls their appearance.

## Step 3: Identify the flex container

Start with the HTML structure rather than adding Flexbox to every element.

```html
<section class="book-list" aria-label="Books">
  <article class="book-card">
    <h2 class="book-title">HTML Foundations</h2>
    <p>Build a structured page.</p>
  </article>
  <article class="book-card">
    <h2 class="book-title">CSS Essentials</h2>
    <p>Style and organise content.</p>
  </article>
</section>
```

1. Find the items you want to arrange: the articles.
2. Find their direct parent: `.book-list`.
3. Apply `display: flex` to that parent.
4. Keep nested headings and paragraphs in normal flow unless they need another layout.

```css
.book-list {
  display: flex;
  gap: 1rem;
}
```

The section becomes the flex container. Its direct children become flex items. Enabling Flexbox alone does not make the cards equal widths.

## Step 4: Arrange and size flex items

### 4.1 Set direction before alignment

```css
.book-list {
  display: flex;
  flex-direction: row;
  gap: 1rem;
  justify-content: flex-start;
  align-items: stretch;
}
```

For the ordinary horizontal writing mode used in these examples:

| Direction | Main axis | Cross axis |
| --- | --- | --- |
| `row` | Horizontal | Vertical |
| `column` | Vertical | Horizontal |

- `justify-content` distributes available space along the main axis.
- `align-items` aligns items along the cross axis.
- `gap` separates items; it does not create padding inside them.
- `space-between` needs spare space to produce a visible distribution.

Try changing `row` to `column` and observe how the axes change.

### 4.2 Allow wrapping

```css
.book-list {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}
```

Wrapping allows another line when necessary. It does not guarantee a fixed number of columns.

### 4.3 Give cards a flexible starting size

```css
.book-card {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 16rem;
}
```

The shorthand is:

```css
.book-card {
  flex: 1 1 16rem;
}
```

Read it in this order:

1. `1`: take a share of available space.
2. `1`: allow shrinking when space is insufficient.
3. `16rem`: use this as the starting size along the main axis.

The final size is not guaranteed to be `16rem`. A card alone on the final line can grow to fill that line.

### Container or item?

| Requirement | Apply to |
| --- | --- |
| Enable Flexbox | Container |
| Set direction, wrapping, gaps or alignment | Container |
| Set growth, shrinkage or starting size | Item |

## Step 5: Add mobile-first media queries

### 5.1 Configure the viewport

Place this inside the HTML `head`:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

### 5.2 Start with a stacked heading

```css
.catalogue-heading {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
```

Base styles apply at every width. They do not stop applying on larger screens.

### 5.3 Change only what needs to change at 768px

```css
@media (min-width: 768px) {
  .catalogue-heading {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}
```

The query adds a conditional change; it does not replace the stylesheet. The base `gap` still applies.

### 5.4 Add the desktop enhancement

```css
@media (min-width: 1024px) {
  .page {
    padding: 2rem 3rem;
  }

  h1 {
    font-size: 2.25rem;
  }
}
```

| Viewport | Matching rules | Heading layout |
| --- | --- | --- |
| `767px` | Base | Column |
| `768px` | Base + 768px query | Row |
| `1024px` | Base + both queries | Row, larger title and padding |
| `1200px` | Base + both queries | Row, larger title and padding |

`min-width` includes the exact boundary. These breakpoints are teaching examples, not universal device categories. Check whether the content actually fits.

Cards can wrap through Flexbox without a media query. The heading's deliberate direction change demonstrates why a media query is useful.

## Step 6: Build the Campus Workshop Directory

### Scope

Build a text-only responsive directory. Registration, filtering, APIs and JavaScript are outside this practical's scope.

The following is a complete worked example adapted from the lesson concepts. The lesson makes the `1024px` practical enhancement optional; it is included here for demonstration.

### 6.1 Create the project structure

```text
campus-workshop-directory/
├── index.html
├── styles.css
├── README.md
└── checks.md
```

Use this guide as `README.md`. Create the HTML and CSS using the following code, and copy the test template in Step 7 into `checks.md`.

### 6.2 Add index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Campus Workshop Directory</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <main class="page">
    <header class="catalogue-heading">
      <div>
        <p class="eyebrow">Learn something new</p>
        <h1>Campus Workshop Directory</h1>
      </div>
      <p class="catalogue-summary">
        Explore practical workshops for students.
      </p>
    </header>

    <section class="workshop-list" aria-label="Available workshops">
      <article class="workshop-card">
        <h2 class="workshop-title">HTML Foundations</h2>
        <p class="workshop-details">Beginner · 2 hours</p>
        <p>Build a structured web page with headings, paragraphs and sections.</p>
      </article>

      <article class="workshop-card">
        <h2 class="workshop-title">Essential CSS</h2>
        <p class="workshop-details">Beginner · 2 hours</p>
        <p>Improve spacing, typography and visual hierarchy.</p>
      </article>

      <article class="workshop-card">
        <h2 class="workshop-title">Responsive Layouts with Flexbox</h2>
        <p class="workshop-details">Beginner · 3 hours</p>
        <p>Arrange flexible cards and adapt a heading for narrow and wide screens.</p>
      </article>
    </section>
  </main>
</body>
</html>
```

### 6.3 Add styles.css

```css
/* Include padding and borders in declared widths. */
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  font-size: 1rem;
  line-height: 1.5;
  color: #27313b;
  background: #f5f7fa;
}

.page {
  max-width: 75rem;
  margin: 0 auto;
  padding: 1rem;
}

/* Base layout: heading content stacks. */
.catalogue-heading {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.catalogue-heading > * {
  min-width: 0;
}

.eyebrow {
  margin: 0 0 0.5rem;
  font-size: 0.875rem;
  font-weight: 700;
}

h1 {
  margin: 0;
  font-size: 1.75rem;
  line-height: 1.2;
  overflow-wrap: anywhere;
}

.catalogue-summary {
  margin: 0;
  max-width: 28rem;
}

/* The parent arranges the card collection. */
.workshop-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: stretch;
}

/* Each child controls its flexible starting size. */
.workshop-card {
  flex: 1 1 16rem;
  min-width: 0;
  padding: 1rem;
  border: 1px solid #69717a;
  border-radius: 0.5rem;
  background: #ffffff;
  overflow-wrap: anywhere;
}

.workshop-title {
  margin: 0 0 0.5rem;
  font-size: 1.25rem;
  font-weight: 700;
  line-height: 1.3;
}

.workshop-details {
  margin: 0 0 0.75rem;
  font-size: 0.875rem;
  color: #485360;
}

.workshop-card p:last-child {
  margin: 0;
}

/* Deliberate heading change at 768px. */
@media (min-width: 768px) {
  .catalogue-heading {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  .catalogue-summary {
    flex: 0 1 20rem;
  }
}

/* Optional practical enhancement at 1024px. */
@media (min-width: 1024px) {
  .page {
    padding: 2rem 3rem;
  }

  h1 {
    font-size: 2.25rem;
  }
}
```

### 6.4 Explain the implementation

1. The viewport declaration prepares the page for responsive layouts.
2. Global `border-box` makes width calculations easier to reason about.
3. The page container provides outer spacing and limits content width.
4. Heading sizes, weights and margins create a consistent reading order.
5. `.workshop-list` arranges its direct children and allows wrapping.
6. `flex: 1 1 16rem` gives each card a flexible starting size.
7. The heading starts in a column and changes to a row at `768px`.
8. At `1024px`, the example increases page padding and title size.
9. `min-width: 0` and `overflow-wrap: anywhere` are additional safeguards in this worked example for narrow items and long text. Still test the actual content rather than assuming overflow is impossible.
10. No fixed card heights are used, allowing descriptions and long titles to take the space they need.

### 6.5 Preview the page

Open `index.html` in a browser. Resize the viewport and inspect the heading, card widths, spacing and text wrapping. No JavaScript or build step is required for this example.

## Step 7: Test and record the results

### Test sequence

1. Open browser developer tools and enable a resizable viewport.
2. Check `320px`, `390px`, `767px`, `768px`, `1023px`, `1024px` and `1200px`.
3. Resize between the test widths as well: isolated screenshots can miss intermediate issues.
4. Look for unintended horizontal scrolling, squeezed text and inconsistent spacing.
5. Check that heading direction changes exactly at `768px`.
6. Check that cards wrap naturally; do not assume a fixed column count.
7. Record actual observations and one corrected issue.

### checks.md template

```markdown
# Responsive Layout Checks

## Test results

| Viewport | Expected behaviour | Actual result | Pass / fail |
| --- | --- | --- | --- |
| 320px | Stacked heading; readable cards; no horizontal scrolling | Not tested | Pending |
| 390px | Stacked heading; readable cards | Not tested | Pending |
| 767px | Heading remains a column | Not tested | Pending |
| 768px | Heading becomes a row | Not tested | Pending |
| 1023px | Row heading; base page padding and title size | Not tested | Pending |
| 1024px | Row heading; larger padding and title | Not tested | Pending |
| 1200px | Cards use available space without horizontal scrolling | Not tested | Pending |
| Intermediate widths | No unintended horizontal scrolling | Not tested | Pending |

## Layout decision

- Decision:
- Why it suits this content:
- CSS declaration responsible:
- Evidence from testing:

## Corrected issue

- Observed problem:
- Cause found in DevTools:
- Change made:
- Widths retested:
- Result:
```

### Success checklist

- [ ] Clear titles, grouped details and consistent spacing.
- [ ] Correct flex container and flexible card sizing.
- [ ] Cards wrap without unintended horizontal scrolling from 320px to 1200px.
- [ ] Heading stacks below 768px and forms a row from 768px.
- [ ] Intermediate widths checked.
- [ ] One layout decision explained.
- [ ] One corrected issue explained.
- [ ] README and checks completed.

The table above is a test plan, not a claim that the example has been browser-tested.

## Step 8: Debug systematically

1. Select the affected element in DevTools.
2. Inspect its content, padding, border and margin.
3. Identify its parent and check whether that parent should be the flex container.
4. Verify that `display: flex` is applied to the correct parent.
5. Check whether the media query matches the current viewport.
6. Look for crossed-out declarations and confirm which rule wins.
7. Change one cause at a time and retest at 767px, 768px and 1024px.

### Specificity recap

For the ordinary competing rules used in this lesson:

```css
.book-card .book-title { color: #6e298d; }
.book-title { color: #27313b; }
```

The first selector is more specific, so purple wins despite its earlier position.

```css
.book-title { color: #6e298d; }
.book-title { color: #27313b; }
```

These selectors have equal specificity, so the later declaration wins. This recap concerns the simple examples above, not every possible CSS cascade situation.

### Quick diagnosis table

| Symptom | Check |
| --- | --- |
| Text touches a card edge | Card padding |
| Cards touch each other | Container gap |
| Cards cannot move onto another line | Container `flex-wrap` |
| Alignment seems to use the wrong direction | `flex-direction` and the axes |
| Last card is wider | Expected growth on its own flex line |
| Heading does not change at 768px | Viewport, query syntax, selector and competing rules |
| Border makes a declared width larger than expected | `box-sizing` |

## Group research

Work in pairs or teams of four for 25 minutes. Each group prepares a finding, a working example and an explanation of the observed result.

| Topic | Focused question | Evidence to prepare |
| --- | --- | --- |
| Spacing and hierarchy | Which size, weight or spacing changes improve scanning? | Before and after, without changing content or colour |
| Direction and alignment | Why do alignment directions change between row and column? | Both arrangements, naming each axis |
| Wrapping and sizing | Why do cards resize, and why can the last card be wider? | Two viewport widths; explain grow, shrink and basis |
| Mobile-first changes | What should change at 768px, and what should stay the same? | Below, at and above the breakpoint |

For sharing: explain the finding, show the example, explain what it demonstrates and answer one class question.

## Exit check

1. Text touches its own card edge. Which property should change?
2. Which element receives `display: flex` when arranging a collection of cards?
3. What does `flex: 1 1 16rem` mean?
4. Does `min-width: 768px` match at exactly 768px?
5. Why might a final-row card be wider than the others?
6. Which base rules still apply after a media query matches?
7. Describe one layout decision and one issue you corrected.

<details>
<summary>Suggested answers for questions 1–6</summary>

1. Padding.
2. The direct parent of the cards.
3. Grow: 1; shrink: 1; starting size: 16rem.
4. Yes.
5. It can use the spare space on its own line through `flex-grow: 1`.
6. All applicable base declarations that are not overridden by winning declarations.

</details>

## After class

- Complete the practical and record your checks.
- Finish README.md and checks.md before submission.
- Optionally practise the taught Flexbox properties with Flexbox Froggy.
- Next lesson in the source slides: External APIs and Data Presentation.

## Source note

Adapted from `FED_Lesson_05_Essential_CSS_Responsive_UX.pdf` (51 pages). The source names MDN resources on Flexbox, media query fundamentals, box-sizing, CSS length units and fundamental text styling for further reading. The complete directory implementation and testing template in this guide are added teaching materials.
