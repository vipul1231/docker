Start with kubernetes for any service

1) Create docker file with name: Dockerfile
   * Sample content: 
     FROM openjdk
     RUN useradd -u 1001 -U --create-home -s /bin/bash appuser
     WORKDIR /home/appuser
     USER appuser
     ADD build/libs/springdoc-0.0.1-SNAPSHOT.jar springdoc-0.0.1-SNAPSHOT.jar
     RUN mkdir /home/appuser/config
     RUN chmod -R 0777 /home/appuser/config
     EXPOSE 8085
     ENTRYPOINT ["java","-jar","springdoc-0.0.1-SNAPSHOT.jar"]
2) Now do docker build so that image can reach docker registry: docker build -t <IMAGE-NAME> .
3) After this run docker run --name <ANY-IMAGE-NAME> -p 8080:8080 <IMAGE-NAME>


Kubernetes:

Image has to be available in minikube registry

Command: eval $(minikube docker-env)

ImagePullPolicy: Never in deployment.yaml


1) $ kubectl create deployment demo --image=<IMAGE-NAME> -o=yaml > deployment.yaml
   $ echo --- >> deployment.yaml
   $ kubectl create service clusterip demo --tcp=<EXPOSE-PORT> -o=yaml >> deployment.yaml
2) kubectl apply -f deployment.yaml
3) Incase you want to delete kubectl delete -f deployment.yaml


Port fowrward: kubectl port-forward svc/spring-doc 8080:8080
