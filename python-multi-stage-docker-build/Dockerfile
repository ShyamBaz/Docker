# ------------------- Stage 1: Build Stage ------------------------------

FROM python:3.9 AS devops-build

# Set the working directory in the image
WORKDIR /app

# Copy the files/dependencies/sourcecode from the local file system to the image file system
COPY requirements.txt /app/
COPY devops /app/

# Install the dependencies
RUN pip install --no-cache-dir -r requirements.txt

# ------------------- Stage 2: Final Stage ------------------------------

# Use a slim Python 3.9 image as the final base image
FROM python:3.9-slim

# Set the working directory to /app
WORKDIR /app

# Copy the built dependencies from the devops-build stage
COPY --from=devops-build /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/

# Copy the application code from the devops-build stage
COPY --from=devops-build /app /app

# Expose the port that app runs on inside the container to the host machine at runtime so that the app is accessible from outside the container
EXPOSE 8000

# Run a command to start the application inside the container at runtime
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]