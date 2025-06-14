# Virtual TA - RAG Knowledge Base API
## TDS Project - 1 By 24f3000209

## Introduction

The **Virtual TA** is a **FastAPI** application designed to answer questions by leveraging a **Retrieval-Augmented Generation (RAG)** system. It queries a knowledge base composed of discourse chunks (e.g., forum posts) and markdown chunks (e.g., documentation) to find relevant information. The system uses embeddings for similarity search and a **Large Language Model (LLM)** to synthesize answers from the retrieved context. It also supports **multimodal queries**, allowing users to include images for a richer contextual understanding.

The application is **hosted on Render** and can be accessed at: <https://virtual-ta-tds-4dg5.onrender.com>

For interactive API documentation (Swagger UI), append `/docs` to the base URL: <https://virtual-ta-tds-4dg5.onrender.com/docs>

## Running Requirements

To run this application locally or deploy it, you will need:

* **Python 3.8+**: The application is built with Python.

* **pip**: For installing Python dependencies.

* **SQLite3**: The knowledge base uses a SQLite database. This is usually pre-installed with Python or can be easily added.

* **Environment Variables**: An `API_KEY` environment variable is required for authenticating with the AI Pipe proxy for embeddings and LLM calls. This key should be an OpenAI API key. Create a `.env` file in the root directory of the project and add your API key:

  ```
  API_KEY="your_openai_api_key_here"
  ```

## How to Use

The primary way to interact with the Virtual TA is through its `/query` endpoint. This endpoint accepts `POST` requests with a question and an optional Base64 encoded image.

### Querying with Text Only

You can send a text-based question to the API to retrieve information from the knowledge base.

**Using `npx` (for a quick test without `curl` or `httpie`):**

```
npx -y promptfoo eval --config project-tds-virtual-ta-promptfoo.yaml
```

**Equivalent `curl` command:**

```
curl -X POST "[https://virtual-ta-tds-4dg5.onrender.com/query](https://virtual-ta-tds-4dg5.onrender.com/query)" \
     -H "Content-Type: application/json" \
     -d '{"question": "What are the common challenges faced by students?"}'
```

### Querying with Text and Image (Multimodal)

The API supports multimodal queries where you can provide a Base64 encoded image along with your question. This is particularly useful for questions related to diagrams, screenshots, or any visual content present in the knowledge base.

**Steps** to include **an image:**

1. **Encode your image to Base64**: You'll need to convert your image file (e.g., a `.jpg` or `.png`) into a Base64 string.

   * **Store images for testing**: When testing locally or developing, it's recommended to have a dedicated folder (e.g., `test_images/`) to store your test images for easy access and encoding.

2. **Include** the Base64 string in the `image` field of your JSON **payload.**


**The `curl` command with an image (example - replace `your_base64_image_string`):**

```
curl -X POST "[https://virtual-ta-tds-4dg5.onrender.com/query](https://virtual-ta-tds-4dg5.onrender.com/query)" \
     -H "Content-Type: application/json" \
     -d '{"question": "Can you explain the diagram in this image?", "image": "your_base64_image_string"}'
```

**Note on image size:** Be mindful of the size of your Base64 encoded image, as very large images can impact request performance and may be subject to API limits.

### LLM Rubric API Key

When using tools like `llm-rubric` or similar LLM evaluation frameworks, you will need to configure them with **your own API key** to access the language model services. This is separate from the `API_KEY` used by the Virtual TA application itself for its internal operations.

For example, if `llm-rubric` requires an OpenAI API key:

```
export OPENAI_API_KEY="your_openai_api_key_for_rubric"
llm-rubric run ...
```

---

**Important Note on Hosting:**

This application is hosted on **Render's free tier**. Please be aware that applications on the free tier may experience a **"cold start"** delay. This means the very first request after a period of inactivity might take longer than subsequent requests as the server needs to wake up and load the application. Subsequent requests will typically be much faster.
