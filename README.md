# Text Summarizer Website - Frontend

A beautiful, galaxy-themed frontend interface for an AI-powered text summarization website.

## Files Structure

```
text-summarizer-website/
├── index.html      # Main HTML structure
├── styles.css      # Galaxy-themed styling
├── script.js       # Frontend JavaScript logic
└── README.md       # This file
```

## Features

- ✨ Beautiful galaxy-themed design with animated stars
- 📝 Text input area for user content
- 🔘 Summarize button with loading state
- 📋 Summary display with copy functionality
- 📖 About section explaining the technology
- 📱 Fully responsive design

## How to Connect Frontend to Flask Backend

### Step 1: Update API URL in `script.js`

The frontend is configured to connect to a Flask backend. In `script.js`, the API URL is set to:

```javascript
const API_URL = 'http://localhost:5000/api/summarize';
```

If your Flask server runs on a different port or domain, update this URL accordingly.

### Step 2: Flask Backend Setup

Your Flask backend should have an endpoint that matches the frontend's expectations:

**Expected Flask Endpoint:**
- **URL:** `/api/summarize`
- **Method:** POST
- **Request Body:** JSON with `text` field
  ```json
  {
    "text": "Your long text here..."
  }
  ```
- **Response:** JSON with `summary` field
  ```json
  {
    "summary": "The generated summary here..."
  }
  ```

### Step 3: Example Flask Backend Code

Here's a sample Flask backend implementation:

```python
from flask import Flask, request, jsonify
from flask_cors import CORS
from transformers import pipeline

app = Flask(__name__)
CORS(app)  # Enable CORS to allow frontend to connect

# Load the model
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

@app.route('/api/summarize', methods=['POST'])
def summarize():
    try:
        data = request.get_json()
        text = data.get('text', '')
        
        if not text:
            return jsonify({'error': 'No text provided'}), 400
        
        # Generate summary
        summary = summarizer(text, max_length=130, min_length=30, do_sample=False)
        
        return jsonify({
            'summary': summary[0]['summary_text']
        })
    
    except Exception as e:
        return jsonify({'error': str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

### Step 4: Enable CORS

**Important:** Make sure to install and enable CORS in your Flask app:

```bash
pip install flask-cors
```

And add this to your Flask app:
```python
from flask_cors import CORS
CORS(app)
```

This allows your frontend (running on a different port or domain) to make requests to your Flask backend.

### Step 5: Running the Application

1. **Start Flask Backend:**
   ```bash
   python app.py
   ```
   The backend should run on `http://localhost:5000`

2. **Open Frontend:**
   - Simply open `index.html` in a web browser, OR
   - Use a local server (recommended):
     ```bash
     # Python 3
     python -m http.server 8000
     
     # Or using Node.js http-server
     npx http-server
     ```
   - Then navigate to `http://localhost:8000` in your browser

### Step 6: Testing the Connection

1. Open the frontend in your browser
2. Enter some text in the text box
3. Click "Summarize Text"
4. The frontend will send a POST request to `http://localhost:5000/api/summarize`
5. The backend processes the text and returns a summary
6. The summary is displayed in the frontend

## Troubleshooting

### CORS Errors
If you see CORS errors in the browser console:
- Make sure `flask-cors` is installed
- Ensure `CORS(app)` is added to your Flask app

### Connection Refused
- Verify Flask backend is running on port 5000
- Check if the API URL in `script.js` matches your Flask server URL
- Ensure no firewall is blocking the connection

### 404 Not Found
- Verify the Flask endpoint is `/api/summarize`
- Check that the endpoint accepts POST requests

## Customization

### Changing Colors
The galaxy theme colors are defined in `styles.css`. Main color variables:
- Primary purple: `#a78bfa`
- Pink accent: `#ec4899`
- Blue accent: `#60a5fa`
- Background: Dark blues and purples

### Modifying API Endpoint
Update the `API_URL` constant in `script.js` to point to your backend server.

## Browser Compatibility

- Modern browsers (Chrome, Firefox, Safari, Edge)
- Requires JavaScript enabled
- Uses Fetch API for HTTP requests

