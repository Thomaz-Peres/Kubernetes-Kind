# Para usar o serviço

I used [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) to create a localo cluster

***obs*** os scripts abaixo é necessário rodar com o ponto final no final:

Suba a imagem do docker:

     docker build -t ingress:1.0 .

Depois que terminou de subir e quiser testar

    docker run -p 8001:80 ingress:1.0 .


Em um terminal:

    curl http://localhost:8001/WeatherForecast


Para criar o Cluster rode esses passos abaixo (depois de instalar o Kind):

    kind create cluster --name ingress --config=Cluster/DeployCluster.yaml

Pode verificar se o cluster está ok com:

    kubectl cluster-info --context kind-ingress

Depois de criar o Cluster no kind, precisamos adicionar a imagem da aplicação neste cluster com:

    kind load docker-image ingress:1.0 --name ingress

Agora instalando o Deployment e Service:

    kubectl apply -f Deployment.yaml

    kubectl apply -f Service.yaml

Se tentar fazer uma requisição para a porta 80, deve disparar um erro, então é necessário rodar o segundo comando:

    kubectl port-forward svc/ingress-service <locahost-port>:<pod-port> (80:80)

    o comando é necessário pois estamos utilizando ClusterIP.

Este ultimo comando transfere a porta local 80 para o serviço 80

Depois de realizar o comando de transferencia de porta e para o serviço, ja deve funcionar a requisição, basta utilizar:

    curl http://localhost:80/WeatherForecast