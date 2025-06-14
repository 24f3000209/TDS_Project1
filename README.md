🧪 TDS Virtual TA - FastAPI App

This is a FastAPI-based backend that helps answer student questions by accepting text (and optionally images), and returning JSON responses.

▶️ How to Run the App Locally

1. Install Python Requirements

pip install -r requirements.txt

2. Set Up Environment Variables

Create a .env file in the project directory:

API_KEY=Bearer your_api_key_here

✅ Make sure to include the word Bearer.

3. Start the FastAPI Server

uvicorn app:app --reload

You’ll see output like:

Uvicorn running on http://127.0.0.1:8000

🔁 How to Run Promptfoo Tests

1. Install promptfoo (if not installed):

npm install -g promptfoo

or use it without installing:

npx -y promptfoo eval --config project-tds-virtual-ta-promptfoo.yaml

2. Make sure your YAML file contains this URL:

url: http://127.0.0.1:8000/api/

✅ The tests will call your local /api endpoint and evaluate its JSON output.

💬 Sample API Request (curl)

curl http://127.0.0.1:8000/api/ \
  -H "Content-Type: application/json" \
  -d '{"question": "Should I use gpt-4o-mini or gpt-3.5 turbo?"}'




