FROM python:3.12-slim

WORKDIR /app

COPY app.py .

RUN pip install --upgrade pip && pip install flask

EXPOSE 5000

USER 65534

CMD ["python", "app.py"]
