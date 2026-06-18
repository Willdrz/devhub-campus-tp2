FROM python:3.12-slim AS build

WORKDIR /app

COPY requirements.txt ./
RUN pip install --user --no-cache-dir -r requirements.txt

COPY . .

FROM python:3.12-slim AS runtime

LABEL org.opencontainers.image.source="https://github.com/willdrz/devhub-campus-tp2"

WORKDIR /app

COPY --from=build /root/.local /root/.local
COPY --from=build /app/app ./app

RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
USER appuser

ENV PORT=8080 \
    LOG_LEVEL=info \
    PYTHONUNBUFFERED=1 \
    PATH=/root/.local/bin:$PATH

EXPOSE 8080

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
