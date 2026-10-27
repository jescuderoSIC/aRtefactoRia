
Pasoa a ejecutar en git para poder subir nuestro repositorio:

git --version
git config --global user.name "jescuderoSIC"
git config --global user.email "jescudero.sic@gmail.com"

Si no tienes creada en tu ordenador una clave tienes que generarla

ssh-keygen -t ed25519 -C "jescudero.sic@gmail.com"

Pero si ya habias hecho el paso anterior ejecutas:

cat ~/.ssh/id_ed25519.pub

y pegas su salida en https://github.com/jescuderoSIC/aRtefactoRia/settings/keys/new


Para testear si estas autenticado:
ssh -T git@github.com


git init
git add .
git commit -m "Primera versión de aRtifactoRia"
git branch -M main
git remote add origin git@github.com:jescuderoSIC/aRtefactoRia.git
git remote set-url origin git@github.com:jescuderoSIC/aRtefactoRia.git
git push -u origin main





Quitamos el soporte para Jekyll

touch .nojekyll
git add .nojekyll
git commit -m "Desactivar Jekyll para GitHub Pages"
git push