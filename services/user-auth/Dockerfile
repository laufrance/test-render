# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set the working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements first to leverage Docker cache
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY . .

# Create directory for SSL certificates
RUN mkdir -p /etc/ssl/certs

# Set environment variables
ENV PYTHONPATH=/app
ENV PGSSLMODE=require

# Expose the port that FastAPI will run on
EXPOSE $PORT

# Command to run the application
CMD gunicorn app.main:app --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:$PORT