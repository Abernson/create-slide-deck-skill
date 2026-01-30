---
name: create-slide-deck
description: Converts markdown files into beautiful HTML/CSS slide presentations. Generates self-contained presentations with click navigation, image support, and modern design. Use when the user wants to create slides, convert markdown to presentation, generate HTML slides, or create a PowerPoint-like presentation from markdown.
---

# Create Slide Deck

Converts markdown files into self-contained HTML/CSS slide presentations with click navigation.

## Workflow

1. **Get input**: Ask user for the markdown file path if not provided
2. **Get output location**: Ask where to save the presentation (default: `./slides/`)
3. **Read** the source markdown file
4. **Analyze structure**: Review the markdown and identify layout opportunities (see Layout Analysis below)
5. **Discuss layout**: If the content has ambiguous structure, ask the user how they want it laid out
6. **Create** output folder with `images/` subfolder
7. **Parse** markdown: `# H1` = section title slide, `## H2` = content slide
8. **Copy** any local images to `images/` subfolder
9. **Generate** HTML using the template in [template.html](template.html)
10. **Write** `index.html` to output folder
11. **Report** completion with the file path

## Layout Analysis

Before generating slides, analyze the markdown structure:

### When to Ask About Layout

Ask the user about layout preferences when you see:
- **Multiple bold sections** (`**Section**`) under one heading - could be columns
- **No H2 headings** - content might need to be split across slides differently
- **Long lists** that could be split into columns
- **Text + image together** - use content-image layout (text left, image right)

### Example Interaction

If you see markdown like:
```markdown
# Quarterly Report
**Wins**
- Item 1
- Item 2

**Challenges**
- Item 1
- Item 2
```

Ask: "I see your content has two sections (Wins, Challenges). Would you like these as:
1. Two-column layout on one slide
2. Separate slides for each section
3. Single slide with stacked sections"

## Input Format

Markdown with two-level hierarchy:
- `# H1` = Section title slide (dedicated slide with just the title, centered)
- `## H2` = Content slide (slide with H2 as heading + content below)
- `**Bold**` subsections = May indicate columns (ask user)

### Example 1: Standard Structure

```markdown
# Introduction
## Welcome
This is the opening slide with some intro text.

## Agenda
- Topic 1
- Topic 2
- Topic 3

# Conclusion
## Questions?
Thank you for your attention
```

### Example 2: Column-Friendly Structure

```markdown
# Quarterly Report

**Relational Shifts**
- Maintained stakeholder connections
- Enhanced editing workflows

**Wins**
- Won $20k in new business
- Scaled service volume successfully
```

For Example 2, ask: "Would you like 'Relational Shifts' and 'Wins' as two columns on one slide, or as separate slides?"

## Output Structure

```
output-folder/
├── index.html      # Self-contained presentation
└── images/         # Copied images from markdown
```

## Markdown Conversion Rules

| Markdown | HTML Output |
|----------|-------------|
| `# Title` | **Section title slide**: `<section class="slide slide-title">` with centered `<h1>` |
| `## Heading` | **Content slide**: `<section class="slide">` with `<h2>` + content |
| `### Subheading` | `<h3>` (within current slide) |
| `**bold**` | `<strong>` |
| `*italic*` | `<em>` |
| `- item` | `<ul><li>` |
| `1. item` | `<ol><li>` |
| `![alt](src)` | `<img alt="alt" src="images/filename">` |
| `` `code` `` | `<code>` |
| Code blocks | `<pre><code>` |
| Paragraphs | `<p>` |

### Slide Types

1. **Section title slides** (`# H1`): Large centered title only, used to introduce new sections
2. **Content slides** (`## H2`): H2 heading with content below (paragraphs, lists, images, etc.)
3. **Multi-column slides**: Use when content has parallel sections (see below)
4. **Content + Image slides**: Text on left, image prominently on right (see below)

### Multi-Column Layout

When content has multiple bold sections that should appear side-by-side:

```html
<div class="columns">
  <div class="column">
    <h3>Section One</h3>
    <ul>
      <li>Item 1</li>
      <li>Item 2</li>
    </ul>
  </div>
  <div class="column">
    <h3>Section Two</h3>
    <ul>
      <li>Item 1</li>
      <li>Item 2</li>
    </ul>
  </div>
</div>
```

Use columns when:
- Two `**Bold**` sections with parallel content
- Side-by-side comparisons (pros/cons, before/after)
- User explicitly requests columns

### Content + Image Layout

When a slide has both text content and an image, use the content-image layout with text on the left and the image prominently displayed on the right:

```html
<div class="content-image">
  <div class="content-side">
    <h2>Slide Title</h2>
    <p>Explanatory text goes here...</p>
    <ul>
      <li>Supporting point</li>
      <li>Another point</li>
    </ul>
  </div>
  <div class="image-side">
    <img src="images/photo.png" alt="Description">
  </div>
</div>
```

Use content-image layout when:
- A slide has text/bullets AND an image together
- You want the image to be prominent (it gets ~60% of the width)
- The image illustrates or complements the text content

The image side is designed to be generous - the image can expand to 70vh height and has enhanced shadow styling for visual impact. On mobile, the layout stacks with content above the image.

### Handling Content Without H2 Headings

If markdown has `# H1` followed by content with no `## H2`:
1. Create the section title slide from `# H1`
2. Create a content slide for the remaining content
3. If there are `**Bold**` subsections, ask user if they want columns or separate slides

## Image Handling

1. Scan markdown for `![alt](path)` patterns
2. For each image:
   - **Local path**: Copy file to `output/images/`, update src to `images/filename.ext`
   - **URL (http/https)**: Keep as-is (external reference)
3. Generate `<img>` tags with proper alt text

## HTML Generation

Generate a single `index.html` file with:

1. **Embedded CSS** - All styles inline in `<style>` tag (see [template.html](template.html))
2. **Slide sections** - Each slide wrapped in `<section class="slide" id="slide-N">`
3. **Navigation** - Invisible click zones (left half = previous, right half = next)

### Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{First H1 title or "Presentation"}</title>
  <style>
    /* Copy CSS from template.html */
  </style>
</head>
<body>
  <div class="slides">
    <!-- Section title slide (from # H1) -->
    <section class="slide slide-title" id="slide-1">
      <div class="slide-content">
        <h1>Section Title</h1>
      </div>
      <a href="#slide-2" class="nav-zone next" aria-label="Next slide"></a>
    </section>
    
    <!-- Content slide (from ## H2) -->
    <section class="slide" id="slide-2">
      <a href="#slide-1" class="nav-zone prev" aria-label="Previous slide"></a>
      <div class="slide-content">
        <h2>Content Slide Title</h2>
        <p>Content goes here...</p>
      </div>
      <a href="#slide-3" class="nav-zone next" aria-label="Next slide"></a>
    </section>
    
    <!-- Content + Image slide -->
    <section class="slide" id="slide-3">
      <a href="#slide-2" class="nav-zone prev" aria-label="Previous slide"></a>
      <div class="slide-content">
        <div class="content-image">
          <div class="content-side">
            <h2>Slide Title</h2>
            <p>Text content on the left...</p>
          </div>
          <div class="image-side">
            <img src="images/photo.png" alt="Description">
          </div>
        </div>
      </div>
      <a href="#slide-4" class="nav-zone next" aria-label="Next slide"></a>
    </section>
    
    <!-- More slides... -->
  </div>
</body>
</html>
```

### Navigation Rules

- **Click zones are invisible** - No visible arrows or dots
- **Right side of screen** = Next slide (pointer shows →)
- **Left side of screen** = Previous slide (pointer shows ←)
- First slide: Only next zone (no prev)
- Last slide: Only prev zone (no next)

## Design System

### Colors (CSS Variables)

```css
:root {
  --bg-color: #1a1a2e;
  --text-color: #eaeaea;
  --accent-color: #4fc3f7;
  --subtle-color: #666;
}
```

### Typography

- **Font**: System font stack (loads instantly)
- **H1**: 3rem, bold
- **H2**: 2rem
- **Body**: 1.5rem
- **Code**: Monospace, slightly smaller

### Layout

- **Aspect ratio**: 16:9
- **Content**: Centered with max-width constraint
- **Slides**: Full viewport, stacked

## Browser Compatibility

Uses CSS `:has()` selector. Supported in:
- Chrome 105+ (Aug 2022)
- Firefox 121+ (Dec 2023)
- Safari 15.4+ (Mar 2022)
- Edge 105+ (Aug 2022)

## Complete CSS Reference

See [template.html](template.html) for the complete HTML/CSS template to use as the base.
