OPENCLAW_REPO := https://github.com/openclaw/openclaw.git
OPENCLAW_DIR := openclaw-source
OPENCLAW_IMAGE := openclaw:local

.PHONY: \
	help \
	compose-up compose-down compose-restart compose-logs compose-ps compose-build compose-pull compose-clean \
	openclaw-clone openclaw-pull openclaw-build openclaw-rebuild openclaw-clean openclaw-shell openclaw-images \
	prune

help:
	@echo ""
	@echo "===== Docker Compose ====="
	@echo "make compose-up        - Sobe o laboratório"
	@echo "make compose-down      - Derruba o laboratório"
	@echo "make compose-restart   - Reinicia os containers"
	@echo "make compose-logs      - Mostra os logs"
	@echo "make compose-ps        - Lista os containers"
	@echo "make compose-build     - Build do compose sem cache"
	@echo "make compose-pull      - Atualiza as imagens"
	@echo "make compose-clean     - Remove containers, volumes e cache"
	@echo ""
	@echo "===== OpenClaw ====="
	@echo "make openclaw-clone    - Clona o repositório oficial"
	@echo "make openclaw-pull     - Atualiza o repositório"
	@echo "make openclaw-build    - Build da imagem local"
	@echo "make openclaw-rebuild  - Rebuild sem cache"
	@echo "make openclaw-clean    - Remove a imagem local"
	@echo "make openclaw-shell    - Abre shell na imagem"
	@echo "make openclaw-images   - Lista as imagens"
	@echo ""
	@echo "===== Utilidades ====="
	@echo "make prune             - Limpa cache do Docker"
	@echo ""

#
# Docker Compose
#

compose-up:
	docker compose up -d

compose-down:
	docker compose down

compose-restart:
	docker compose restart

compose-logs:
	docker compose logs -f

compose-ps:
	docker compose ps

compose-build:
	docker compose build --no-cache

compose-pull:
	docker compose pull

compose-clean:
	docker compose down -v --remove-orphans
	docker system prune -af

#
# OpenClaw
#

openclaw-clone:
	@if [ ! -d "$(OPENCLAW_DIR)" ]; then \
		echo "Clonando OpenClaw..."; \
		git clone $(OPENCLAW_REPO) $(OPENCLAW_DIR); \
	else \
		echo "Repositório já existe em $(OPENCLAW_DIR)"; \
	fi

openclaw-pull:
	@if [ -d "$(OPENCLAW_DIR)/.git" ]; then \
		cd $(OPENCLAW_DIR) && git pull; \
	else \
		echo "Repositório não encontrado."; \
		echo "Execute: make openclaw-clone"; \
		exit 1; \
	fi

openclaw-build: openclaw-clone
	cd $(OPENCLAW_DIR) && \
	docker build -t $(OPENCLAW_IMAGE) .

openclaw-rebuild: openclaw-clone
	cd $(OPENCLAW_DIR) && \
	docker build --no-cache -t $(OPENCLAW_IMAGE) .

openclaw-clean:
	docker image rm -f $(OPENCLAW_IMAGE) || true

openclaw-images:
	docker images | grep openclaw || true

openclaw-shell:
	docker run --rm -it $(OPENCLAW_IMAGE) sh

#
# Utilidades
#

prune:
	docker builder prune -af
	docker system prune -af --volumes