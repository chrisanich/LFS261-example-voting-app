# Base stage using the Python runtime image
FROM python:3.11-slim AS base

# Add curl for health check
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /usr/local/app

# Copy requirements and install dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Define a development stage for local development
FROM base AS dev
RUN pip install watchdog
ENV FLASK_ENV=development
CMD ["python", "app.py"]

# Define the final production stage
FROM base AS final

# Copy all source code to the working directory
COPY . .

# Expose port 80 and run the application with gunicorn
EXPOSE 80
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
