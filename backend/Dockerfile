FROM python:3.8
#https://hub.docker.com/_/django/ -> se uso el Dockerfile que se encuentra alli como base, y se lo fue modificando para que funcione, luego para optimizar layers.
#docker run --name app-backend -p 8000:8000 -d app-backend  -> para correr el container.
#para instalar dependencias necesarias para mas adelante
RUN apt-get update && apt-get install -y --no-install-recommends \
    postgresql-client \
    libpq-dev \
    gcc \
    build-essential \
    graphviz \
    libgraphviz-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app
RUN pip install --upgrade pip \
    && pip install psycopg2 \
    psycopg2-binary \
    pygraphviz

COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .

ENV SECRET_KEY="soylasecret"
EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]