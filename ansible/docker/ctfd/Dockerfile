FROM ctfd/ctfd:latest

# Switch to root to install dependencies
USER root

# Install Python dependencies for plugins (Whale)
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Back to ctfd user
USER ctfd
