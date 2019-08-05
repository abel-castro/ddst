FROM python:3.7-alpine

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

COPY ./requirements /requirements

# install dependencies
RUN apk update \
    && apk add --virtual build-deps gcc python3-dev musl-dev \
    && apk add postgresql-dev \
    # Pillow dependencies
    jpeg-dev zlib-dev \
    && pip install --upgrade pip && pip install --no-cache-dir -r /requirements/dev.txt \
    && apk del build-deps

# copy files
COPY ./compose/entrypoint.sh /entrypoint.sh
RUN sed -i 's/\r//' /entrypoint.sh && chmod +x /entrypoint.sh

COPY ./compose/local/start /start
RUN sed -i 's/\r//' /start && chmod +x /start

COPY ./app/ /app/

WORKDIR /app

ENTRYPOINT ["/entrypoint.sh"]
