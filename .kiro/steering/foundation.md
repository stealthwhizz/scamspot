# SafeText Development Standards

## Code Quality
- Write clean, self-documenting code
- Add comments for complex pattern matching logic
- Use meaningful variable names
- Keep functions focused and small

## Architecture  
- Single HTML file with embedded CSS/JS
- No external dependencies
- Client-side only (no backend)
- Mobile-first responsive design

## Pattern Detection
- Use efficient regex patterns
- Test all patterns thoroughly
- Handle edge cases gracefully
- Clear scoring algorithm

## UI/UX
- Large, readable fonts (16px minimum)
- Clear visual hierarchy
- Smooth animations (0.3s transitions)
- Accessible (ARIA labels, keyboard navigation)
- Color-coded risk levels (green/yellow/red)

## Performance
- Fast page load (<100KB total)
- Instant analysis (<1 second)
- Efficient DOM updates
- No unnecessary re-renders

## Security & Privacy
- No data storage or tracking
- No external API calls
- Safe pattern matching (no eval)
- Clear privacy messaging
