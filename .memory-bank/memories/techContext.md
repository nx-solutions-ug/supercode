# Tech Context: SuperCode Framework

## Technologies Used
- **Frontend**: React 18+, TypeScript 5+, Vite 5+
- **Styling**: TailwindCSS 3+, Shadcn/ui components, Tailwind Animate
- **Build System**: Vite with TypeScript, ESLint, PostCSS
- **UI Libraries**: Radix UI primitives, Lucide React icons, Class Variance Authority
- **Testing**: Playwright for visual testing and screenshot validation
- **Deployment**: GitHub Pages with custom domain support
- **Package Manager**: npm with Node.js 16+

## Development Setup
- Node.js 16.0.0+ and npm 7.0.0+ required
- TypeScript strict mode enabled with full type checking
- ESLint configuration with TypeScript and React rules
- Path mapping configured for clean imports (@/* aliases)
- Pre-commit hooks for code quality validation

## Technical Constraints
- GitHub Pages static hosting (client-side routing)
- TypeScript strict mode compliance required
- Maximum bundle size optimization for performance
- Mobile-first responsive design requirements
- Cross-browser compatibility (ES2020+ support)

## Dependencies
- **Core**: @opencode-ai/plugin@^0.15.8
- **React Ecosystem**: react@^18.2.0, react-dom@^18.2.0, react-router-dom@^6.20.0
- **UI Components**: @radix-ui/* packages for accessibility
- **Utilities**: class-variance-authority, clsx, tailwind-merge
- **Development**: @typescript-eslint/*, vite, typescript
- **Build Tools**: autoprefixer, postcss, tailwindcss, vite-plugin-generate-file

## Tool Usage Patterns
- **Behavioral Flags**: --ultrathink, --orchestrate, --safe, --performance for task control
- **Agent Commands**: /spawn, /implement, /build, /improve for workflow execution
- **Memory Management**: /load, /save, /initialize for context persistence
- **Quality Assurance**: ESLint, TypeScript, automated testing through agents
- **Deployment**: npm scripts for build, preview, and deployment automation