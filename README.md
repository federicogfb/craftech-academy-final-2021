# Desafio final del curso de DevOps

Para darle un cierre a las clases de Craftech, los invitamos a intentar completar estas pequeñas tareas que resumen lo visto en el curso!

### Resumen

Se cuenta con el diseño de una aplicación para prestamos por internet. El backend fue construido con [Django](https://www.djangoproject.com/) utilizando [Python v3.7](https://www.python.org/), la cual se encuentra [acá](backend/) y el frontend fue construido con [React.js](https://es.reactjs.org/), la cual se encuentra [acá](frontend/).

Se necesita:

1. Armar un diagrama de los componentes que se involucran (Base de datos, deployments, etc) como si estuviera deployado en AWS.

1. Dockerizar las aplicaciones Frontend y Backend

1. Armar un docker-compose que nos permita levantar la aplicación entera y sus dependencias

1. Armar los manifiestos de Kubernetes para deployar las aplicaciones y sus dependencias en un cluster (Utilizar minikube como ref)

1. Diseñar un pipeline de CI/CD de la aplicación frontend y backend que nos permita deployar en 2 entornos (dev y prod)# ci-dockerhub-cd-minikube


# Resolucion

Primero se empezo con la Dockerizacion del front y el back. En el back la app anda sin problemas, si bien hubo que resolverse algunas dificultades al momento de crear la imagen correctamente (las dependencias pygrapviz y psycopg2) esto pudo solventarse. En cambio, en el frontend muchas de las dependencias se encuentran deprecadas e implicaria hacer cambios en el codigo y esto se encuentra fuera de mi area, asi que preferi no tocar nada; aun asi, la app pudo ser Dockerizada pero se encuentra apenas armada pero funcional. 

El docker-compose lo arme rapido ya que es mucho mas sencillo que armar un Dockerfile. 

Para la creacion de los manifiestos de Kubernetes utilice Kompose. Primero se los guardaba en una carpeta llamada “deployment” pero debido a errores en el workflow de Github Actions, opte por crear carpetas de “dev” y “prod” y copiar los manifiestos dentro de ambas carpetas.

Para el pipeline del CI/CD primero se opto por intentar hacerlo en Jenkins, en una instancia contenerizada. Debido a limitaciones del hardware, esta opcion se tuvo que abandonar.
El segundo intento fue en Gitlab. Utilizando un runner dentro de un container, la idea era la misma que la anterior: utilizando un container podria tener una network compartida en Docker para que sea facil la comunicacion entre mi instancia de Minikube y la del gitlab-runner. Esta opcion se abandono debido a que el pipeline utilizaba DinD y el runner ya estaba alojado en un container.
La ultima opcion y la que resulto fue utilizando Github Actions. Primero se intento de varias maneras conectar el runner de Github a mi instancia local de Minikube, como esto no resulto, opte por crear una instancia de Minikube dentro del propio pipeline para “simular” la mia local. Funciono y estaba por dejarlo asi hasta que investigando se me ocurrio instalar un runner de Github Actions en mi maquina. Utilizaria este runner para correr solamente el deployment a mi instancia local de Minikube, facilitando asi la comunicacion entre el runner y tambien este tendria acceso a todas mis herramientas (kubectl por ejemplo).

En conclusion: utilizando Github Actions, un runner local y uno proporcionado por Github, el CI de crear la imagen y subirla al Dockerhub; y luego el CD de deployar los manifiestos de Kubernetes en mi instancia de Minikube es posible. 
