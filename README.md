#!/bin/bash

echo "=== Instalando Docker + Portainer ==="

# Actualizar sistema
sudo apt update -y
sudo apt install -y ca-certificates curl gnupg lsb-release

# Instalar repos oficial de Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
  $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Crear directorio Portainer
sudo mkdir -p /portainer_data

# Ejecutar Portainer (última versión estable)
sudo docker volume create portainer_data

sudo docker run -d \
  -p 8000:8000 \
  -p 9443:9443 \
  -p 9000:9000 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest

echo "=== Portainer instalado correctamente ==="
echo "Acceso web:"
echo "  • HTTPS: https://TU-IP:9443"
echo "  • HTTP (legacy): http://TU-IP:9000"
echo "  • Edge Agent: puerto 8000"
echo "Listo para usar."
