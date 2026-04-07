FROM python:3.10-slim

# Install system dependencies required for Playwright and building native packages
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    curl \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy requirements file first (for Docker cache)
COPY requirements.txt .

# Install dependencies using pip 
RUN pip install --no-cache-dir -r requirements.txt

# Install Chromium strictly for Langchain/Playwright Web Scrapers
RUN playwright install chromium
RUN playwright install-deps chromium

# Copy the rest of the backend codebase into the Docker container
COPY . .

# Many container platforms (Railway, Render) pass the port as an environment variable
ENV PORT=8000
EXPOSE 8000

# Start the FastAPI application
CMD uvicorn controllers.api:app --host 0.0.0.0 --port $PORT
