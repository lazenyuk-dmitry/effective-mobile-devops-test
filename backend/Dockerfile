FROM python:3.11-slim

WORKDIR /app

# Создаём непривилегированного пользователя
RUN useradd -m appuser

COPY app.py .

# Меняем владельца файлов
RUN chown -R appuser:appuser /app

# Переключаемся на непривилегированного пользователя
USER appuser

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8080')"

CMD ["python", "app.py"]
