Aplicação composta por 3 serviços: <i>backend (Java)</i>, <i>frontend (Angular)</i> e <i>database (MySQL)</i>.

O código-fonte responsável pela geração das imagens está disponível no meu projeto [docker-sample-project](https://github.com/ManassesAlmeida/docker-sample-project). A aplicação foi implantada e testada no Amazon EKS (Amazon Elastic Kubernetes Service).

# Executando localmente com Kubectl e Minikube
Passos para executar a aplicação:

1. Clonar o projeto do repositório.
    - Escolha um diretório e execute o comando: 
    <br><code>git clone https://github.com/ManassesAlmeida/kubernetes-sample-project.git</code>
2. Criar os ingresses, aplicando na ordem abaixo.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute os comandos: 
    <br><code>kubectl apply -f .\ingress\ingress-nginx-controller-deploy.yaml</code> (Após executar este comando, aguarde cerca de 1 minuto para que a criação dos serviços finalize)
    <br><code>kubectl apply -f .\ingress\ingress.yaml</code>
3. Criar o namespace.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\namespaces\ </code>
4. Criar os deployments.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\deployments\ </code>
5. Criar os services.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\services\ </code>
6. Faça o redirecionamento de porta (necessário no Windows) do serviço <i>backend</i> e do serviço <i>frontend</i>.
    - Para redirecionar a porta do serviço backend execute o comando: <code>kubectl port-forward service/backend 31000:8080 -n product</code>
    - Para redirecionar a porta do serviço frontend execute o comando: <code>kubectl port-forward service/frontend 31001:80 -n product</code>

Link para visualizar o frontend em Angular: http://localhost:31001/ <br>
Link para visualizar o backend em Java: http://localhost:31000/api/products <br>

Para finalizar a execução e efetuar a exclusão dos pods/ingresses/deployments/services, execute os comandos: 
- <code>kubectl delete all --all -n ingress-nginx</code>
- <code>kubectl delete all --all -n product</code>
