````markdown
# Lesson 05: Essential CSS and Responsive UX

> Front-End Development — Year 1, Semester 2  
> Organise content. Arrange it. Adapt it.

This guide introduces essential CSS concepts through short examples, group research and an independent practical. In the practical, you will build and test a responsive Campus Workshop Directory using the concepts taught in this lesson.

The examples demonstrate individual concepts. A complete practical implementation is intentionally not provided: you will write your own HTML and CSS and explain your decisions.

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
- [After class](#after-class)
- [Source note](#source-note)

## Learning outcomes

By the end of this lesson, you should be able to:

1. Organise content with consistent spacing, text sizes and weights.
2. Explain what a declared width includes using `box-sizing`.
3. Use Flexbox for direction, alignment, wrapping and flexible sizing.
4. Adapt a layout at `768px` and optionally enhance it at `1024px`.
5. Inspect, test and explain a layout decision and a corrected issue.

## Lesson plan

| Activity | Duration | Intended result |
| --- | --- | --- |
| Explain and demonstrate | 30 min | Recognise the patterns |
| Group research | 25 min | Prepare a finding and example |
| Selected sharing | 25 min | Explain and compare |
| Tutor synthesis | 20 min | Consolidate and debug |
| Practical briefing | 10 min | Understand files and tasks |
| Supported practical | 35 min | Apply the CSS independently |
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

The following example uses a book catalogue heading:

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

### 5.4 Add a desktop enhancement

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

For this demonstration:

| Viewport | Matching rules | Heading layout |
| --- | --- | --- |
| `767px` | Base | Column |
| `768px` | Base + 768px query | Row |
| `1024px` | Base + both queries | Row, larger title and padding |
| `1200px` | Base + both queries | Row, larger title and padding |

`min-width` includes the exact boundary. These breakpoints are teaching examples, not universal device categories. Check whether the content actually fits.

Cards can wrap through Flexbox without a media query. The heading's deliberate direction change demonstrates why a media query is useful.

The `1024px` enhancement is optional in the practical. If you implement it, choose a change that helps your content rather than copying the demonstration without a reason.

## Step 6: Build the Campus Workshop Directory

### Your task

Create a text-only, responsive directory that helps students explore campus workshops.

Apply the concepts from this lesson to organise the content, arrange the workshop cards and adapt the layout for different viewport widths. You must write your own HTML and CSS.

Registration, filtering, APIs and JavaScript are outside this practical's scope.

### 6.1 Create the project structure

```text
campus-workshop-directory/
├── index.html
├── styles.css
├── README.md
└── checks.md
```

Use the files as follows:

| File | Purpose |
| --- | --- |
| `index.html` | Your page structure and content |
| `styles.css` | Your styling and responsive layout |
| `README.md` | A short project description and instructions for opening the page |
| `checks.md` | Your test results and explanations, using the template in Step 7 |

Write a project-specific README. You do not need to copy this lesson guide into your submission.

### 6.2 Include the required content

Your page must include:

- A page heading: Campus Workshop Directory.
- A short introductory sentence.
- At least three workshop cards.
- A title, difficulty level, duration and short description for each workshop.

You may use the following content or write your own:

| Workshop | Level | Duration | Description |
| --- | --- | --- | --- |
| HTML Foundations | Beginner | 2 hours | Build a structured web page with headings, paragraphs and sections. |
| Essential CSS | Beginner | 2 hours | Improve spacing, typography and visual hierarchy. |
| Responsive Layouts with Flexbox | Beginner | 3 hours | Arrange flexible cards and adapt a heading for narrow and wide screens. |

### 6.3 Meet the layout requirements

#### Content and spacing

- Use appropriate HTML elements for the page heading and workshop content.
- Establish a clear hierarchy between the page title, workshop titles and supporting details.
- Use consistent spacing inside and between cards.
- Make a deliberate choice about what declared widths include.

#### Flexbox

- Identify the correct parent element for arranging the cards.
- Use Flexbox to arrange the card collection.
- Allow cards to wrap when there is insufficient space.
- Give cards flexible sizing rather than fixed widths.
- Let the content determine each card's height.

#### Responsive behaviour

- Include the viewport declaration.
- Start with a layout suitable for a narrow screen.
- Below `768px`, stack the page title and introductory content.
- From `768px`, arrange the page title and introductory content in a row.
- Keep the content readable without unintended horizontal scrolling from `320px` to `1200px`.
- Optional: add an appropriate enhancement at `1024px` and explain why it helps.

The number of card columns does not need to change at the same width as the page heading. Decide whether the cards fit by inspecting the available space.

### 6.4 Build incrementally

1. Write the HTML and check the content order.
2. Add typography, spacing and card styling.
3. Arrange the cards using Flexbox.
4. Add the responsive heading change.
5. Test at the widths listed in Step 7 and between those widths.
6. Investigate and correct an issue.
7. Record your results and explain one layout decision.

Use the earlier lesson examples as references, but adapt them to your own structure rather than copying a complete page.

### 6.5 Submit your work

Submit all four project files.

Your submission must include:

- Your own HTML and CSS implementation.
- Actual test observations in `checks.md`.
- An explanation of one layout decision.
- An explanation of one issue you found and corrected.
- A short README describing the page and how to open it.

Do not mark a test as passed until you have checked it in a browser.

## Step 7: Test and record the results

### Test sequence

1. Open your page in a browser.
2. Open browser developer tools and enable a resizable viewport.
3. Check `320px`, `390px`, `767px`, `768px`, `1023px`, `1024px` and `1200px`.
4. Resize between the test widths as well: isolated screenshots can miss intermediate issues.
5. Look for unintended horizontal scrolling, squeezed text and inconsistent spacing.
6. Check that heading direction changes exactly at `768px`.
7. Check that cards wrap naturally; do not assume a fixed column count.
8. If you added a `1024px` enhancement, check that it works as intended.
9. Record actual observations and one corrected issue.

### checks.md template

Copy the following template into `checks.md`:

```markdown
# Responsive Layout Checks

## Test results

| Viewport | Expected behaviour | Actual result | Pass / fail |
| --- | --- | --- | --- |
| 320px | Stacked heading; readable cards; no horizontal scrolling | Not tested | Pending |
| 390px | Stacked heading; readable cards | Not tested | Pending |
| 767px | Heading remains a column | Not tested | Pending |
| 768px | Heading becomes a row | Not tested | Pending |
| 1023px | Row heading; readable content; no horizontal scrolling | Not tested | Pending |
| 1024px | Row heading; readable content; optional enhancement works if implemented | Not tested | Pending |
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

## Optional 1024px enhancement

- Implemented: Yes / No
- Change made, if applicable:
- Why it helps:
- Evidence from testing:
```

### Success checklist

- [ ] Clear titles, grouped details and consistent spacing.
- [ ] Correct flex container and flexible card sizing.
- [ ] Cards wrap without unintended horizontal scrolling from 320px to 1200px.
- [ ] Heading stacks below 768px and forms a row from 768px.
- [ ] Intermediate widths checked.
- [ ] One layout decision explained.
- [ ] One corrected issue explained.
- [ ] Optional 1024px enhancement tested, if implemented.
- [ ] Project README and actual test results completed.

The table above is a test plan. Replace “Not tested” and “Pending” with your actual observations and results. Do not invent a corrected issue or claim to have tested a width that you have not checked.

## Step 8: Debug systematically

1. Select the affected element in DevTools.
2. Inspect its content, padding, border and margin.
3. Identify its parent and check whether that parent should be the flex container.
4. Verify that `display: flex` is applied to the correct parent.
5. Check whether the media query matches the current viewport.
6. Look for crossed-out declarations and confirm which rule wins.
7. Change one cause at a time.
8. Retest at the width where the problem occurred and around the relevant breakpoint.

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

For sharing:

1. Explain the finding.
2. Show the example.
3. Explain what the observed result demonstrates.
4. Answer one class question.

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

- Complete your independent practical.
- Record your actual test results in `checks.md`.
- Finish your project README before submission.
- Check that all four required files are included.
- Optionally practise the taught Flexbox properties with Flexbox Froggy.
- Next lesson in the source slides: External APIs and Data Presentation.

## Source note

Adapted from `FED_Lesson_05_Essential_CSS_Responsive_UX.pdf` (51 pages). The source names MDN resources on Flexbox, media query fundamentals, box-sizing, CSS length units and fundamental text styling for further reading.

The Campus Workshop Directory task brief and testing template are added teaching materials. A complete implementation is intentionally not provided so that students can apply and demonstrate the lesson concepts independently.
````
