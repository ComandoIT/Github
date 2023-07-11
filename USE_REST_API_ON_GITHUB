#!/bin/bash
# Definir variables
REPO_OWNER="nombre_usuario"
REPO_NAME="nombre_repositorio"
TOKEN="tu_token"
BRANCH="nombre_rama"

# Realizar una solicitud POST para crear un árbol de cambios
response=$(curl -X POST \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  -d '{
    "base_tree": "master",
    "tree": [
      {
        "path": "archivo.txt",
        "mode": "100644",
        "content": "Contenido del archivo"
      }
    ]
  }' \
  "https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/git/trees")

# Extraer el SHA del árbol de cambios
tree_sha=$(echo "$response" | jq -r '.sha')

# Realizar una solicitud POST para crear un nuevo commit
response=$(curl -X POST \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  -d '{
    "message": "Mensaje del commit",
    "tree": "'"$tree_sha"'",
    "parents": ["master"]
  }' \
  "https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/git/commits")

# Extraer el SHA del nuevo commit
commit_sha=$(echo "$response" | jq -r '.sha')

# Realizar una solicitud PATCH para actualizar la referencia de la rama
curl -X PATCH \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  -d '{
    "sha": "'"$commit_sha"'"
  }' \
  "https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/git/refs/heads/$BRANCH"
