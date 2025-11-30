# Technology Stack

## Core Technologies

- **HTML5**: Semantic markup with accessibility features
- **CSS3**: Embedded styles using flexbox for responsive layout
- **Vanilla JavaScript**: ES6+ features, no frameworks or libraries

## Architecture Constraints

- **Single file**: All code, styles, and markup in one HTML file
- **No external dependencies**: No npm packages, CDN links, or external resources
- **No backend**: Pure client-side processing
- **File size limit**: Must be under 100KB total

## Browser Support

Target modern browsers with ES6 support:
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Testing

- **Unit tests**: For specific examples and edge cases
- **Property-based tests**: Using fast-check library (100+ iterations per property)
- **Property test format**: Comment tag `// Feature: SafeText, Property {number}: {description}`

## Common Commands

Since this is a single-file application with no build system:

- **Development**: Open the HTML file directly in a browser
- **Testing**: Run tests with your chosen test runner (if tests are in separate files)
- **Distribution**: Simply copy/share the single HTML file

## Performance Requirements

- Analysis must complete in under 1 second
- Page load must complete in under 3 seconds on mobile 4G
- All regex patterns compiled once at initialization
