Comandos útiles de linux

```shell
# Genera una lista de links de los archivos contenidos un nivel inferior en la misma carpeta
find . -maxdepth 1 -not -path '*/.*' -printf "- [%f](%f)\n" | sort

# Genera una lista de links de todos los archivos contenidos en las carpeta inferiores y lo guarda en README.md
find . -not -path '*/.*' -type f | sort | while read -r f; do echo "- [${f#./}](${f// /%20})"; done >

```
