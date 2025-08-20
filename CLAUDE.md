# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Slidev presentation project. Slidev is a presentation framework for developers that uses Markdown for slide content and Vue.js components for interactive elements.

## Development Commands

```bash
# Install dependencies
pnpm install

# Start development server (opens browser automatically)
pnpm dev

# Build for production
pnpm build

# Export slides to PDF, PPTX, or PNG
pnpm export
```

Development server runs on: http://localhost:3030

## Project Structure

- **slides.md**: Main presentation file containing all slides in Markdown format with YAML frontmatter configuration
- **components/**: Custom Vue components (e.g., Counter.vue) that can be embedded in slides
- **pages/**: Additional markdown pages that can be imported into the main presentation
- **snippets/**: External TypeScript/JavaScript code snippets that can be referenced in slides

## Key Technologies

- **Slidev CLI**: Core presentation framework
- **Vue 3**: For reactive components and interactions
- **UnoCSS**: Atomic CSS utilities used in components and slides
- **pnpm**: Package manager (version 10.12.1)
- **Node.js**: Version 24.6.0 (managed by Volta)

## Slide Configuration

Slides are configured through YAML frontmatter in slides.md:
- `theme`: Presentation theme (currently using 'seriph')
- `transition`: Slide transition effects
- `mdc`: MDC Syntax support enabled
- `drawings`: Presentation drawing features

## Working with Slides

- Edit slides.md to modify presentation content
- Each slide is separated by `---`
- Components can be imported and used directly in Markdown
- Slide notes are added as HTML comments at the end of each slide