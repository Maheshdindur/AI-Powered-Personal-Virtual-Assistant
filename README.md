# AI-Powered Personal Virtual Assistant

A scalable and context-aware conversational AI agent built to act as a professional virtual representative on a portfolio website. This service uses the Gemini 2.0 Flash model with advanced Function Calling capabilities, deployed as a robust **FastAPI** service via **Docker**.

## 🚀 Key Features

* **Context-Aware Conversation:** Provides detailed and professional answers about my career, background, and skills by referencing a dynamically loaded professional summary and resume content.
* **Advanced Function Calling (Tools):** Integrates two crucial tools for lead management and system improvement:
    * `record_user_details`: Captures user emails, names, and context notes from the conversation for future outreach.
    * `record_unknown_question`: Logs any question the LLM cannot answer, facilitating continuous improvement and knowledge base expansion.
* **Asynchronous API:** Built with **FastAPI** for high performance and low latency, making it ideal for integration into any frontend application.
* **Cloud Native Ready:** Containerized using **Docker** for consistent, environment-agnostic deployment on platforms like **Google Cloud** or any other cloud provider.
* **Real-time Notifications:** Utilizes the `ntfy.sh` (or a similar service like Pushover, depending on the final configuration) service for push notifications whenever a lead is captured or an unknown question is logged.

## 💻 Technologies Used

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Language** | Python (3.10+) | Core development language. |
| **Web Framework** | **FastAPI** | Building the asynchronous API endpoints. |
| **Server** | **Uvicorn** | ASGI server for running the FastAPI application. |
| **AI Model** | Gemini 2.0 Flash | The underlying Large Language Model for conversational intelligence. |
| **Deployment** | **Docker** | Containerization for consistent and scalable deployment. |
| **Cloud Platform** | Google Cloud (GCP) | Hosting the Dockerized API service. |
| **Dependencies** | `openai` SDK | Python client used to interact with the Gemini API (via the OpenAI compatibility layer). |
| **Utilities** | `pydantic`, `pypdf`, `requests`, `python-dotenv` | Data validation, resume PDF parsing, external notifications, and environment management. |

## 🌐 Live Deployments

The API is deployed and accessible in multiple environments, showcasing flexibility in deployment strategy:

* **Hugging Face Space:**
    * **Link:** [https://huggingface.co/spaces/mahi1432/Virtual\_Me](https://huggingface.co/spaces/mahi1432/Virtual_Me)
* **Personal Portfolio Website:** Integrated directly into the portfolio via the **Google Cloud-hosted API**, demonstrating production integration.
     * **Link:** [https://maheshdindur.github.io/](https://maheshdindur.github.io/)
     

## ⚙️ Project Architecture

The project follows a standard microservice architecture:

1.  **FastAPI Endpoint (`/chat`):** Receives a user `message` and the conversation `history`.
2.  **LLM Interaction:** The core `Me` class constructs a detailed system prompt, including the resume and summary, and calls the Gemini API.
3.  **Function Calling Loop:** The agent processes the conversation. If a tool call (e.g., `record_user_details`) is detected, the `handle_tool_call` function executes the corresponding Python function (which sends a push notification). The tool's output is then sent back to the LLM for a final, natural language response.
4.  **Deployment:** The application is packaged into a minimal Python environment using Docker, exposed on port `8080`, and deployed to a Google Cloud service (e.g., Cloud Run or Compute Engine).

## 🛠️ Local Setup and Installation

### Prerequisites

* Python 3.10+
* Docker (Optional, for containerized run)
* An API Key for the Gemini API (Used via the `OPENAI_API_KEY` environment variable in the code).

### Steps

1.  **Clone the Repository:**
    ```bash
    git clone [YOUR-REPO-URL]
    cd ai-personal-assistant-api
    ```

2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Setup Environment Variables:**
    Create a file named `.env` in the root directory and populate it with your keys.

    ```bash
    # .env file content
    GOOGLE_API_KEY="YOUR_GEMINI_API_KEY"
    NTFY_TOPIC="YOUR_NTFY_TOPIC_FOR_NOTIFICATIONS" # Replace with your notification topic
    # The code also references files in a "me" directory (e.g., resume_for_Virtual_Assistant.pdf and summary.txt) 
    # which must be present for the agent to function.
    ```

4.  **Run the Server Locally:**
    ```bash
    uvicorn backend_api:app --reload --host 0.0.0.0 --port 8000
    ```
    The API will be available at `http://0.0.0.0:8000`.

## 🐳 Docker Deployment

For a robust and production-ready deployment, use the provided `Dockerfile`.

1.  **Build the Docker Image:**
    ```bash
    docker build -t personal-agent-api .
    ```

2.  **Run the Container:**
    Replace the environment variables below with your actual values.
    ```bash
    docker run -d \
      -p 8080:8080 \
      -e GOOGLE_API_KEY="YOUR_GEMINI_API_KEY" \
      -e NTFY_TOPIC="YOUR_NTFY_TOPIC_FOR_NOTIFICATIONS" \
      --name personal-agent \
      personal-agent-api
    ```
    The API is now running on port `8080`.

