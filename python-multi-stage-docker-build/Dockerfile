# ------------------- Stage 1: Build Stage ------------------------------

FROM ubuntu AS devops-build

# Set the working directory in the image
WORKDIR /app

# Copy the files/dependencies/sourcecode from the local file system to the image file system
COPY requirements.txt /app/
COPY devops /app/

# Install the necessary packages
RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

# Use bash shell to run the commands
#SHELL ["/bin/bash", "-c"]

# Create a virtual environment and install the dependencies
RUN python3 -m venv venv1 && \
source venv1/bin/activate && \
pip install --no-cache-dir -r requirements.txt

# ------------------- Stage 2: Final Stage ------------------------------

# Use a slim Python 3.9 image as the final base image
FROM python:3.9-slim
# Use a distroless Python 3.9 image as the final base image
#FROM gcr.io/distroless/python3

# Set the working directory to /app
WORKDIR /app

# Copy the built dependencies from the devops-build stage
COPY --from=devops-build /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/

# Copy the application code from the devops-build stage
COPY --from=devops-build /app /app

# Expose the port that app runs on inside the container to the host machine at runtime so that the app is accessible from outside the container
EXPOSE 8000

# Run a command to start the application inside the container at runtime
CMD source venv1/bin/activate && python3 manage.py runserver 0.0.0.0:8000