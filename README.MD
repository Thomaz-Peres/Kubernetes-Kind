# To use the service

I used [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) to create a local cluster

***PS*** the scripts below is necessary run with the dot (.), or the Dockerfile folder (./api1) in the final:

Enter in the `api1` directory:

    cd api1

Build the image on docker:

    docker build -t api:1.0 .

After finished do build and want to test:

    docker run --rm -it -p 8001:80 -e ASPNETCORE_HTTP_PORTS=80 api:1.0

In a terminal:

    curl http://localhost:8001/weatherforecast

To create a 'Cluster' run this steps (After install Kind):

    kind create cluster --name api --config=Cluster/DeployCluster.yaml

It is possible to check the Cluster with:

    kubectl cluster-info --context kind-api

After create the cluster on Kind, we need to add the app image in the cluster:

    kind load docker-image api:1.0 --name api

Now applying the Deployment and the Service:

    kubectl apply -f Deployment.yaml

    kubectl apply -f Service.yaml

If try to made a request in the port 80, you will receive a error, so it's necessary run the follow command:

    kubectl port-forward svc/api-service <locahost-port>:<pod-port> (80:80)

    The command it's necessary because we're using ClusterIP (only because I made some testings ).

This last command transfer the local port 80 to the service

After using the command, problaly will work, only need to use:

    curl http://localhost:80/WeatherForecast
