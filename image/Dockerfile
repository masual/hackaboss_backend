# We specify the base image we need for our backend application
FROM python:3.7-slim

# We specify Dockerfile's maintener
LABEL maintainer="manuel.suarez@ednon.es" \
      organization="ednonlabs.com"

# Information about app port
EXPOSE 8000

# Ensures that the python output is sent straight to container log
ENV PYTHONUNBUFFERED=1

# Install ping to check db is up
RUN apt update -y && apt install iputils-ping -y

# Upgrade Pip
RUN pip install --upgrade pip

# Create an unprivileged user
RUN useradd -ms /bin/bash ednonlabs

# Set the current working directory inside the container
WORKDIR /opt/ednonhackaboss

# Copy our code to /opt/ednonhackaboss/
COPY --chown=ednonlabs:root ednonhackaboss/ /opt/ednonhackaboss/
COPY --chown=ednonlabs:root image/entrypoint/initdb.sh /opt/ednonhackaboss/

# Set execution permission
RUN chmod 755 /opt/ednonhackaboss/initdb.sh

# Install dependencies
RUN pip install -r requirements.txt

# Use an unprivileged user
USER ednonlabs

# Configure PostgreSQL and wait for it
ENTRYPOINT ["/opt/ednonhackaboss/initdb.sh"]

# We run our App
CMD ["python", "./manage.py", "runserver", "0.0.0.0:8000"]
