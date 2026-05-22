FROM python:3.12-slim

WORKDIR /app

COPY app.py .

RUN pip install flask

EXPOSE 5000

USER nobody

CMD ["python", "app.py"]
