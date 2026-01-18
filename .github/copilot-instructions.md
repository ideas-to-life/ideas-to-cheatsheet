# Copilot Instructions for ideas-to-cheatsheet

## Project Overview
This is a single-file HTML reference guide for Claude Code users. The project generates a searchable, categorized cheatsheet with styling, filtering, and search functionality for commands, tips, and workflows.

## Architecture & Key Patterns

### File Structure
- **`claude-cheatsheet.html`** (591 lines) - The entire application in one file
  - Embedded CSS styling (`<style>` block, lines ~1-215)
  - HTML structure with semantic sections (`<body>`, `<header>`, `<div class="content">`)
  - Vanilla JavaScript for interactivity (`<script>` block, lines ~570+)

### Core Concepts
- **Categories system**: Content organized by `data-category` attributes (`getting-started`, `commands`, `workflows`, `git`, `tips`)
- **Search/Filter mechanism**: Real-time filtering based on search term + active category filter
- **Responsive design**: Mobile-first CSS with gradient backgrounds and hover effects
- **Click-to-copy**: Command blocks support copying to clipboard on click

## Content Structure

### Data Organization
Content is structured using semantic HTML sections:
```html
<div class="section" data-category="CATEGORY_NAME">
  <h2 class="section-title">Title</h2>
  <div class="card"><!-- or .tip for tips --></div>
</div>
```

### Content Types
- **Cards** (`.card`): Command blocks, tables, and general content
- **Tips** (`.tip`): Yellow highlighted tips with `.tip-title` headers
- **Commands** (`.command`): Monospace code blocks with click-to-copy

## Development Patterns

### Adding Content
1. Add a new `<div class="section" data-category="...">` block
2. Include proper semantic HTML (`.section-title`, `.card`, `.command`)
3. Ensure content is filterable (search works by including searchable text in cards)

### Styling
- Color scheme: Purple gradient (`#667eea`, `#764ba2`) for primary, dark slate (`#2d3561`) for text
- Hover effects: Cards translate and change background on hover
- Responsive breakpoint: 768px media query handles mobile

### JavaScript Functionality
- **Search**: Case-insensitive, searches all text within sections
- **Filtering**: Click filter buttons to show only matching category
- **Highlighting**: Yellow highlight (`#fff3cd` background) on cards when search matches
- **Copy to clipboard**: `.command` blocks copy cleaned text (whitespace normalized)

## Adding Features
- **New filter categories**: Add button with `data-filter` attribute + corresponding `data-category` section
- **New content cards**: Follow existing `.card` or `.tip` markup patterns
- **Styling changes**: Modify CSS variables or class styles in `<style>` block

## Testing Checklist
- Search functionality filters content correctly
- Filter buttons toggle active state and show/hide categories
- No results message appears when no matches found
- Copy-to-clipboard works on command blocks
- Responsive design works on mobile (test at 768px)
- All links in footer open correctly

## Performance Notes
- Single-file architecture keeps deployment simple (just copy HTML)
- No external dependencies - uses vanilla JavaScript only
- CSS uses `-webkit-` prefixes for older browser support
