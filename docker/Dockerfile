FROM python:3.9-alpine

RUN apk add --no-cache git postgresql-dev gcc libc-dev
RUN apk add --no-cache gcc g++ make libffi-dev python3-dev build-base

RUN pip install gunicorn
RUN pip install psycopg2
RUN pip install ipdb
RUN pip install ipython


WORKDIR /app

RUN git clone https://github.com/decide-update-4-1/decide-update-4.1.git .
RUN pip install -r requirements.txt

WORKDIR /app/decide

# local settings.py
ADD docker-settings.py /app/decide/local_settings.py

RUN ./manage.py collectstatic

#CMD ["gunicorn", "-w 5", "decide.wsgi", "--timeout=500", "-b 0.0.0.0:5000"]

# Test para Jenkins 
# Instalar las herramientas necesarias para ejecutar los tests
RUN apk add --no-cache --virtual .build-deps \
    gcc \
    musl-dev \
    && pip install --no-cache-dir pytest \
    && apk del .build-deps
    
WORKDIR /app
# Copiar el archivo requirements.txt y luego instalar las dependencias
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Copiar los archivos del proyecto
COPY . /app/

# Ejecutar los tests
CMD ["pytest"]
