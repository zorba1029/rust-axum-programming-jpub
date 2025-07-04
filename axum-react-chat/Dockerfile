# 1. 프론트엔드 빌드 스테이지
FROM node:20-alpine AS frontend
WORKDIR /app
COPY frontend .
RUN yarn install
RUN yarn run vite build --outDir dist

# 2. 러스트 빌드 스테이지
FROM rust:latest AS backend
WORKDIR /app
COPY backend .
RUN cargo build --release --bin docker

# 3. 최종 프로덕션 스테이지
FROM debian:bookworm-slim
WORKDIR /app

# SSL 라이브러리 설치
RUN apt-get update && apt-get install -y openssl && rm -rf /var/lib/apt/lists/*

# 빌드된 프론트엔드/백엔드 파일 복사
COPY --from=frontend /app/dist static
COPY --from=backend /app/target/release/docker app

# 포트 노출
EXPOSE 3000

# 애플리케이션 실행
CMD ["./app"]


#-------------------------------------------
# 1> docker build
# docker build -t axum-chat-app .
#
# 2> docker run
# docker run -p 3000:3000 -e DATABASE_URL="postgres://postgres:postgres@host.docker.internal:5432/postgres" --name axum-chat-app-container axum-chat-app

# 3> docker stop
# docker stop axum-chat-app

# 4> docker rm
# docker rm axum-chat-app
