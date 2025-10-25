# Project 01: Hash Explorer Tool

**Chapter:** 01 - Blockchain Fundamentals
**Difficulty:** Beginner
**Estimated Time:** 2-3 hours

---

## 📋 Project Description

Build an interactive web application that demonstrates the avalanche effect in cryptographic hashing. Users can input any text and see how even tiny changes create completely different SHA-256 hashes.

---

## 🎯 Learning Objectives

- Understand the avalanche effect in hashing
- Work with cryptographic hash functions
- Create interactive educational tools
- Visualize blockchain concepts

---

## 🛠️ Technology Stack

**Frontend:**
- HTML5
- CSS3 (or Tailwind CSS)
- JavaScript (Vanilla or React)

**Backend/Libraries:**
- CryptoJS library for SHA-256 hashing
- OR Node.js `crypto` module

**Tools:**
- VS Code
- Git/GitHub
- Live Server extension (for testing)

---

## ✅ TODO List

### Phase 1: Setup
- [ ] Create project folder structure
- [ ] Set up HTML boilerplate
- [ ] Include CryptoJS from CDN or install via npm
- [ ] Create basic CSS styling

### Phase 2: Core Functionality
- [ ] Create input textarea for user text
- [ ] Implement SHA-256 hash calculation on input
- [ ] Display hash output in hexadecimal format
- [ ] Add character counter for input

### Phase 3: Avalanche Effect Demo
- [ ] Create side-by-side comparison feature
- [ ] Show original input vs modified input (e.g., "Hello" vs "hello")
- [ ] Highlight character differences in input
- [ ] Show both hashes and highlight differences
- [ ] Add "Compare" button to toggle comparison mode

### Phase 4: Additional Features
- [ ] Add preset examples (blockchain messages, transactions)
- [ ] Implement hash history (last 5 hashes)
- [ ] Add copy-to-clipboard functionality
- [ ] Show hash statistics (length, character distribution)

### Phase 5: Polish
- [ ] Add responsive design for mobile
- [ ] Include educational tooltips
- [ ] Add dark mode toggle
- [ ] Create user guide section

---

## 💡 Hints

**Hint 1: Using CryptoJS**
```javascript
// Include CryptoJS in HTML:
// <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>

// Calculate SHA-256:
const hash = CryptoJS.SHA256("your text here").toString();
```

**Hint 2: Avalanche Effect Comparison**
- Compare "Blockchain" vs "blockchain" (just lowercase 'b')
- Show that hash is completely different
- Use color coding to highlight changes
- Calculate how many characters changed in the hash (should be ~32 out of 64)

**Hint 3: Visual Difference Highlighting**
- Loop through both hashes character by character
- Apply different CSS class where characters differ
- Use red/green colors for visual impact

**Hint 4: Interesting Test Cases**
- Genesis block message: "The Times 03/Jan/2009..."
- Single character change: "Hello" → "Hello!"
- Whitespace matters: "Hello" → "Hello "
- Empty string hash

**Hint 5: Educational Additions**
- Explain what SHA-256 means (256-bit = 64 hex characters)
- Show binary representation
- Explain collision resistance
- Link to Bitcoin Genesis block on explorer

---

## 🎨 Bonus Challenges

1. **Hash Multiple Algorithms**: Add MD5, SHA-1, SHA-512 comparison
2. **Hash File Upload**: Allow users to upload files and hash them
3. **Block Mining Simulator**: Let users find hash with N leading zeros
4. **Animation**: Animate the avalanche effect with transitions
5. **API Integration**: Connect to blockchain explorer API to show real block hashes

---

## 📚 Reference Links

- **CryptoJS Documentation:** https://cryptojs.gitbook.io/docs/
- **SHA-256 Specification:** https://en.wikipedia.org/wiki/SHA-2
- **Web Crypto API:** https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API

---

## ✨ Success Criteria

- ✅ Input changes immediately update hash
- ✅ Comparison mode clearly shows avalanche effect
- ✅ UI is intuitive and educational
- ✅ Works on desktop and mobile
- ✅ Code is clean and well-commented
- ✅ Includes at least 3 preset examples

---

**Remember:** NO CODE implementation here - figure it out yourself! Use hints and references as guides.

---

*Project for Blockchain Learning Series - Chapter 01*
*Last Updated: October 25, 2025*