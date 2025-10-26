# Voice AI Assistant

This project is a voice AI assistant that can answer questions about a salon. It uses LiveKit Agents for the voice AI functionality, a FastAPI backend to handle business logic, and Firestore as a database.

## Setup

1.  **Install dependencies:**

    ```bash
    uv sync
    ```

    ```bash
    uv run python src/agent.py download-files
    ```

2.  **Set up your environment:**

    Copy the `.env.example` file to `.env.local` and fill in the LiveKit environment variables:

    ```
    LIVEKIT_URL=""
    LIVEKIT_API_KEY=""
    LIVEKIT_API_SECRET=""
    ```

3.  **Set up Firebase:**

    a.  Create a Firebase project and a Firestore database.
    b.  Download your Firebase service account key and save it as `firebase_credentials.json` in the root of the project.

4.  **Run the project:**

    ```bash
    uv run src/api.py
    ```
    ```bash
    uv run python src/agent.py console
    ```


## Design Notes

### Agent

The agent is built using the `livekit-agents` library. It is designed to be a helpful AI receptionist for a salon. The agent has the following capabilities:

*   **Answering questions:** The agent can answer questions about the salon's hours, location, and services. This information is defined in the agent's instructions.
*   **Knowledge base:** The agent can learn from answers provided by a human expert. When a question is answered, it is stored in a Firestore collection called `knowledge_base`. The agent fetches this knowledge base on startup and uses it to answer questions.
*   **Human escalation:** If the agent does not know the answer to a question, it will use the `ask_human_expert` tool to escalate the question to a human. The question is stored in a Firestore collection called `questions` with a status of `pending`. A human can then answer the question, and the agent will provide the answer to the user.

### API

The backend API is built with FastAPI. It provides the following endpoints:

*   `/questions`: Create and get questions.
*   `/questions/{question_id}`: Get and update a specific question.
*   `/knowledge-base`: Get the knowledge base.
*   `/`: Serves a simple HTML page that can be used to view and answer pending questions.

### Database

The project uses Firestore as its database. There are two main collections:

*   `questions`: Stores questions that have been asked by users. Each question has a `status` field that can be `pending`, `answered`, or `unresolved`.
*   `knowledge_base`: Stores questions that have been answered by a human expert. This is used to train the agent.
