# Specification: Research Knowledge Base Application

This document outlines the specifications for a Go-based application that builds and maintains a knowledge base from research literature, leveraging Swarmgo (specifically, the library at https://github.com/prathyushnallamothu/swarmgo) for storage, Gemini for AI-powered querying, and a web interface for user interaction.

## 1. Core Functionality:

* **Data Ingestion:**
    * **Manual Upload:** Users can upload PDF, DOCX, TXT, and potentially other formats of academic articles, official reports, and research literature. The application should handle various file formats and character encodings gracefully.
    * **Web Search (Scheduled):** The application performs scheduled (daily/weekly) web searches for new research literature based on user-defined keywords, topics, or search queries. It should integrate with search engine APIs (e.g., Google Scholar API, or custom web scraping with careful respect for `robots.txt` and usage limits) to identify potential sources.
    * **Metadata Extraction:** Upon ingestion, the application automatically extracts rich metadata from the sources, including:
        * Title
        * Authors/Organizations
        * Publication Date
        * Journal/Report Name
        * Volume/Issue/Pages
        * DOI/URL/ISBN
        * Abstract (if available)
        * Keywords (if available)
    * **Text Extraction:** The application extracts the plain text content from the uploaded documents. Consider using libraries like `unioffice` (for DOCX), `go-pdf` (for PDF), or other suitable libraries.

* **Data Storage:**
    * **Swarm Integration:** The extracted text content is stored on the Swarm network using `swarmgo` (https://github.com/prathyushnallamothu/swarmgo). The metadata is stored in a separate database (see below) and linked to the Swarm hash of the corresponding text.
    * **Metadata Database:** A relational database (e.g., PostgreSQL, SQLite) or a NoSQL database (e.g., MongoDB) is used to store the extracted metadata. This database should be optimized for efficient querying and retrieval. Consider vectorizing the text and storing the vectors in the database to enable semantic search.

* **Knowledge Base Querying:**
    * **AI-Powered Queries:** Users can prompt the AI (Gemini) with questions related to the knowledge base. The application retrieves relevant text chunks from Swarm (based on metadata and potentially vector search) and provides them as context to Gemini.
    * **Response Display:** The AI's response is displayed to the user in the web interface.
    * **Citation/Source Linking:** The AI's response should be linked back to the original sources in the knowledge base, allowing users to verify information and explore further.

* **Web Interface:**
    * **Source Management:** Users can view, add, edit, and delete sources in the knowledge base.
    * **Browsing/Searching:** Users can browse the knowledge base by metadata fields (e.g., author, title, keywords) or perform keyword searches.
    * **Query Interface:** A user-friendly interface for prompting the AI with questions.
    * **Admin Panel:** An admin panel for managing users, scheduling web searches, and monitoring system performance.

## 2. Technical Specifications:

* **Programming Language:** Go
* **Web Framework:** Gin (or a similar Go web framework)
* **Swarm Client:** `swarmgo` (https://github.com/prathyushnallamothu/swarmgo)
* **AI Model:** Gemini (or a suitable LLM)
* **Database:** PostgreSQL (recommended), SQLite (for simpler deployments), or MongoDB (for more flexible schema). Consider TimescaleDB extension for time-series data related to publication dates.
* **Text Extraction Libraries:** `unioffice`, `go-pdf`, or other suitable libraries.
* **Web Search Integration:** APIs for Google Scholar or other relevant search engines, or custom web scraping (with careful consideration of `robots.txt` and rate limiting).
* **Vectorization:** Consider using sentence-transformers or similar libraries for creating text embeddings.
* **Scheduling:** A task scheduler library (e.g., `robfig/cron`) for scheduling web searches.
* **Deployment:** Docker and Docker Compose for easy deployment.

## 3. Functional Requirements:

* **User Authentication:** Secure user authentication and authorization.
* **Data Validation:** Input validation to prevent errors and ensure data integrity.
* **Error Handling:** Robust error handling and logging.
* **Scalability:** Design the application to be scalable to handle a growing knowledge base and increasing user load.
* **Performance:** Optimize the application for speed and efficiency, especially for querying and retrieval.
* **Security:** Implement security best practices to protect sensitive data.

## 4. Non-Functional Requirements:

* **Usability:** The web interface should be intuitive and easy to use.
* **Maintainability:** The code should be well-structured and easy to maintain.
* **Testability:** The application should be testable, with unit and integration tests.
* **Documentation:** Clear and comprehensive documentation.

## 5. Future Considerations:

* **Advanced AI Features:** Explore using more advanced AI features, such as summarization, topic modeling, and sentiment analysis.
* **Knowledge Graph:** Consider building a knowledge graph on top of the data to enable more complex queries and analysis.
* **API:** Develop an API for programmatic access to the knowledge base.

