# pruebas jenkins local

password para registro: 947e415a87224ec28c6b80439e669c8a


para instalar npm

wget

(Instalar google por dockerfile)
# Check available versions here: https://www.ubuntuupdates.org/package/google_chrome/stable/main/base/google-chrome-stable
ARG CHROME_VERSION="95.0.4638.54-1"
RUN wget --no-verbose -O /tmp/chrome.deb https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_VERSION}_amd64.deb \
  && apt install -y /tmp/chrome.deb \
  && rm /tmp/chrome.deb


(Para instalar google a pelo Con versión específica 95.0.4638.54-1)
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
wget --no-verbose -O /tmp/chrome.deb https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_95.0.4638.54-1_amd64.deb
apt install -y /tmp/chrome.deb




Para guardar imagen de frontend/backend a nexus hay que configurar nexus previamente:

 - Hay que entrar dentro del contenedor con un: docker exec {containe id de nexus} cat /nexus-data/admin.password con este comando sacamos la password y no podemos ir a la dirección donde esta levantada nexus localhost:8081 y en el login ponemos el username (admin) y el password anteriormente conseguido y te pedirá que cambies la contraseña, en mi caso puse admin ya que es en local.

 - Una vez dentro de nexus en configuración (la tuerca) vamos a repositorios y creamos un repositorio de docker hay tres tipos de repositorio de docker, tenemos que elegir el de hosted, dentro hay que darle un nombre y usar otro puerto distinto en el http (Por eso en el docker-compose hay dos puertos abiertos en mi caso el 8082), habilitamos el docker V1 API, la policy en disable redeploy y check allow redeployonly on 'latest' tag, con eso creamos repositorio.



 docker tag frontend-test:$(cat version) localhost:8082/frontend-test:$(cat version)
 docker push localhost:8082/frontend-test:$(cat version)

 - Para hacer el push hay que autenticarse que a pelo sería (en mi caso): docker login -u admin -p admin localhost:8082 y ya podriamos hacer el push.



 Una vez se haga en docker-compose configurar jenkins y nexus para que al realizar el build en pipeline funcione todo.

 Para saber la pass inicial de jenkins docker exec -u root prueba-jenkins-frontend-local_jenkins_1 cat /var/jenkins_home/secrets/initialAdminPassword

 vincular jenkins con gitlab creando un multibranch
