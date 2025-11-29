# 🛡️ ScamSpot - SMS Scam Detector

A free, offline-capable web application that helps users identify suspicious SMS messages by analyzing them for common scam patterns. Built as a single HTML file with no dependencies.

## ✨ Features

- **Instant Analysis** - Get results in under 1 second
- **Pattern Detection** - Identifies 11+ common scam indicators
- **Risk Scoring** - Clear color-coded risk levels (LOW/MEDIUM/HIGH)
- **Actionable Advice** - Specific recommendations based on risk level
- **Scammer Insights** - Explains what attackers are trying to achieve
- **100% Private** - No data collection, no tracking, works completely offline
- **Mobile-First** - Optimized for checking messages on your phone
- **Accessible** - WCAG AA compliant with keyboard navigation and screen reader support
- **Shareable** - Single file that's easy to distribute to family and friends

## 🚀 Quick Start

### Option 1: Open Directly
Simply open `index.html` in any modern web browser. That's it!

### Option 2: Host Locally
```bash
# Using Python 3
python -m http.server 8000

# Using Node.js
npx serve

# Then open http://localhost:8000
```

### Option 3: Deploy
Upload `index.html` to any web hosting service. No build process or server required.

## 📱 How to Use

1. **Paste Message** - Copy a suspicious SMS into the text area
2. **Click "Check for Scam"** - Analysis happens instantly
3. **Review Results** - See risk score, red flags, and recommendations
4. **Take Action** - Follow the advice based on the risk level

### Example Messages to Test

**High Risk Scam:**
```
Dear customer, your account will be blocked in 24 hours. 
Verify immediately: bit.ly/verify123 
Share OTP to continue
```

**Low Risk Legitimate:**
```
Your OTP for Amazon order is 456789. 
Valid for 10 minutes. 
Do not share with anyone. 
-AMAZON
```

## 🔍 What It Detects

### High-Risk Patterns (25 points each)
- Requests for OTP, PIN, password, CVV
- Urgency language ("immediately", "within 24 hours", "account blocked")
- Threats ("legal action", "warrant", "police")
- Prize claims ("you won", "lottery winner")
- Financial requests ("send money", "pay now")

### Medium-Risk Patterns (15 points each)
- Bank impersonation without official sender ID
- Government agency impersonation
- Shortened URLs (bit.ly, tinyurl, etc.)
- Generic greetings ("Dear customer", "Dear user")

### Low-Risk Indicators (-10 points each)
- Official sender codes (BK-, VM-, AMAZON, etc.)
- Legitimate security warnings ("do not share")

## 🎯 Risk Levels

| Score | Level | Color | Meaning |
|-------|-------|-------|---------|
| 0-30% | LOW RISK | 🟢 Green | Likely legitimate |
| 31-60% | MEDIUM RISK | 🟡 Yellow | Proceed with caution |
| 61-100% | HIGH RISK | 🔴 Red | Likely scam - do not respond |

## 🛠️ Technical Details

### Architecture
- **Single HTML file** - All code, styles, and markup in one file
- **No dependencies** - Pure vanilla JavaScript (ES6+)
- **No backend** - 100% client-side processing
- **File size** - Under 100KB total

### Browser Support
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

### Technology Stack
- HTML5 with semantic markup
- CSS3 with Flexbox for responsive layout
- Vanilla JavaScript (ES6+)
- No frameworks, no libraries, no external resources

## 🔒 Privacy & Security

- **No data collection** - Messages are never stored or transmitted
- **No tracking** - No analytics, cookies, or third-party scripts
- **Works offline** - No internet connection required after initial load
- **XSS protection** - Uses `textContent` instead of `innerHTML`
- **Open source** - Inspect the code yourself

## ♿ Accessibility

- ARIA labels on all interactive elements
- Keyboard navigation support (Tab, Enter, Ctrl+Enter)
- Screen reader compatible
- Color-blind friendly (doesn't rely solely on color)
- Minimum 16px font size
- 44x44px touch targets for mobile
- Respects `prefers-reduced-motion`

## 📂 Project Structure

```
.
├── index.html              # Main application (single file)
├── README.md              # This file
└── .kiro/
    └── specs/
        └── scamspot/
            ├── requirements.md  # User stories & acceptance criteria
            ├── design.md        # Architecture & correctness properties
            └── tasks.md         # Implementation checklist
```

## 🧪 Testing

The application was developed using property-based testing to ensure correctness:

- **Pattern detection completeness** - All patterns are detected
- **Score normalization** - Scores always between 0-100
- **Risk level classification** - Correct thresholds applied
- **Case-insensitive matching** - Works regardless of capitalization
- **Input validation** - Handles empty/invalid inputs gracefully

## 🤝 Contributing

This is a single-file application designed for simplicity and portability. If you'd like to suggest improvements:

1. Test your changes thoroughly
2. Ensure the file remains under 100KB
3. Maintain accessibility standards
4. Keep it dependency-free

## 📋 Common Use Cases

- **Personal Use** - Check suspicious messages you receive
- **Family Protection** - Share with elderly relatives vulnerable to scams
- **Education** - Teach others about common scam tactics
- **Offline Access** - Save to your phone for use without internet

## ⚠️ Limitations

- **Pattern-based only** - Uses regex matching, not AI/ML
- **English language** - Optimized for English SMS messages
- **No link checking** - Doesn't verify if URLs are malicious
- **No sender verification** - Can't validate actual sender identity
- **Educational tool** - Not a replacement for security software

## 💡 Tips for Best Results

1. **Paste the entire message** - Include all text for accurate analysis
2. **Check sender ID** - Compare with known official sender codes
3. **Verify independently** - Contact organizations through official channels
4. **Trust your instincts** - If something feels off, it probably is
5. **Share with others** - Help protect friends and family

## 📞 When in Doubt

- **Never share** OTP, PIN, password, or CVV
- **Don't click links** in suspicious messages
- **Contact directly** using official phone numbers/websites
- **Report spam** to your mobile carrier
- **Block sender** if confirmed scam

## 📄 License

This project is free to use, modify, and distribute. No attribution required.

## 🙏 Acknowledgments

Built with a focus on accessibility, privacy, and helping protect vulnerable users from SMS scams.

---

**Remember:** ScamSpot is a tool to help identify suspicious patterns. Always use your judgment and verify through official channels when in doubt.

Stay safe! 🛡️
