# Use Alpine as the base image
FROM python:3.11-alpine AS base

# Install curl for healthchecks & dependencies for Python packages
RUN apk add --no-cache curl gcc musl-dev libffi-dev

# Set the application directory
WORKDIR /usr/local/app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# dev defines a stage for development, where it'll watch for filesystem changes
FROM base AS dev
RUN pip install --no-cache-dir watchdog
ENV FLASK_ENV=development
CMD ["python", "app.py"]

# final defines the stage for production
FROM base AS final

# Copy application code
COPY . .

# Expose port 80
EXPOSE 80

# Run the app with Gunicorn
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
