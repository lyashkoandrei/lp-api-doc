# Land Portal API Documentation

Beautiful API documentation with tabs for different services.

## 🚀 Deploy to GitHub Pages

### Automatic Deployment

1. **Create a repository** on GitHub
2. **Upload files** to the repository:
   - `index.html` - main page
   - `styles.css` - styles
   - `script.js` - JavaScript
   - `README.md` - documentation

3. **Enable GitHub Pages**:
   - Go to Settings → Pages
   - Select Source: "Deploy from a branch"
   - Select Branch: "main" (or "master")
   - Click Save

4. **Your site will be available** at:
   ```
   https://yourusername.github.io/lp-api-doc
   ```

### Manual Deployment

If you already have a repository:

```bash
# Clone the repository
git clone https://github.com/yourusername/lp-api-doc.git
cd lp-api-doc

# Add files
git add .
git commit -m "Add API documentation"
git push origin main
```

## 📁 Project Structure

```
lp-api-doc/
├── index.html          # Main page
├── styles.css          # Styles
├── script.js           # JavaScript
└── README.md           # Documentation
```

## 🎨 Features

- ✅ **Responsive design** - works on all devices
- ✅ **Tabs** for different API sections
- ✅ **Syntax highlighting** for code and JSON
- ✅ **Smooth scrolling** for anchor links
- ✅ **Beautiful code blocks** with custom scrollbars
- ✅ **Color indicators** for successful/error responses

## 🔧 Configuration

### Adding a New API Tab

1. **Add tab button** to the `.tabs` section:
```html
<button class="tab-button" onclick="openTab(event, 'new-api')">New API</button>
```

2. **Create tab content**:
```html
<div id="new-api" class="tab-content">
    <div class="api-section">
        <h2>New API</h2>
        <!-- Your content -->
    </div>
</div>
```

### Changing Styles

All styles are in the `styles.css` file. Main classes:

- `.tab-button` - tab buttons
- `.tab-content` - tab content
- `.code-block` - code blocks
- `.json-example` - JSON examples
- `.success-response` - successful responses
- `.error-response` - error responses

## 🌐 Browser Support

- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

## 📝 License

MIT License - use freely for your projects.

## 🤝 Contributing

1. Fork the repository
2. Create a branch for new feature
3. Make changes
4. Create Pull Request

## 📞 Support

If you have questions or suggestions, create an Issue in the repository.