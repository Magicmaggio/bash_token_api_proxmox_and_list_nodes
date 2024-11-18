#!/bin/bash

# Charger le fichier .env
export $(grep -v '^#' .env | xargs)

# Paramètres d'entrée
echo "Proxmox Host: $PROXMOX_HOST"
echo "Proxmox Username: $PROXMOX_USERNAME"
echo "Proxmox Password: $PROXMOX_PASSWORD"

# Obtenir un jeton d'accès
echo "Obtention du jeton d'accès..."
AUTH_RESPONSE=$(curl -s -k -X POST "https://$PROXMOX_HOST/api2/json/access/ticket" \
    --data-urlencode "username=$PROXMOX_USERNAME" \
    --data-urlencode "password=$PROXMOX_PASSWORD")

# Afficher la réponse brute pour débogage
echo "Réponse brute de l'authentification :"
echo "$AUTH_RESPONSE" | jq .

# Extraire le jeton et le token CSRF
TICKET=$(echo "$AUTH_RESPONSE" | jq -r '.data.ticket')
CSRF_TOKEN=$(echo "$AUTH_RESPONSE" | jq -r '.data.CSRFPreventionToken')

# Vérifier si l'authentification a réussi
if [[ -z "$TICKET" || "$TICKET" == "null" ]]; then
    echo "Erreur : Impossible d'obtenir un jeton d'accès. Vérifiez les paramètres."
    exit 1
fi

# Afficher les tokens pour vérification
echo "Jeton d'accès (ticket) : $TICKET"
echo "Token CSRFPrevention : $CSRF_TOKEN"

# Utiliser le jeton pour obtenir la liste des nœuds
echo "Récupération de la liste des nœuds..."
NODES_RESPONSE=$(curl -s -k -X GET "https://$PROXMOX_HOST/api2/json/nodes" \
    -H "Cookie: PVEAuthCookie=$TICKET")

# Afficher la réponse brute pour les nœuds
echo "Réponse brute de la requête des nœuds :"
echo "$NODES_RESPONSE" | jq .

# Extraire et afficher la liste des nœuds
NODES=$(echo "$NODES_RESPONSE" | jq -r '.data[] | .node')
if [[ -z "$NODES" ]]; then
    echo "Erreur : Impossible de récupérer la liste des nœuds."
    exit 1
fi

echo "Liste des nœuds disponibles :"
echo "$NODES"
