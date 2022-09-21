# pull official base image
FROM python:3.8-slim-buster

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_DISABLE_VERSION_CHECK=on \
    POETRY_VERSION=1.2.0 \
    POETRY_HOME="/opt/poetry" \
    POETRY_NO_INTERACTION=1 \
    PATH=/usr/local/bin:$PATH \
    ENVIRONMENT=dev \
    TESTING=0

# install system dependencies
RUN apt-get update \
    && apt-get -y install --no-install-recommends \
        netcat gcc postgresql \
    && pip install --no-cache-dir "poetry==$POETRY_VERSION" \
    && apt-get clean

# install python dependencies
WORKDIR /usr/src/app
COPY ./pyproject.toml ./poetry.lock ./
RUN poetry config virtualenvs.create false \
    && poetry install  --no-root

# add app
COPY . .

# add entrypoint.sh
COPY ./entrypoint.sh .
RUN chmod +x /usr/src/app/entrypoint.sh

# run entrypoint.sh
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]