FROM node:22-alpine

RUN apk add --no-cache \
    bash \
    curl \
    git \
    openssh-client

RUN addgroup -S openclaw && \
    adduser -S openclaw -G openclaw

USER openclaw
WORKDIR /home/openclaw

CMD ["tail", "-f", "/dev/null"]