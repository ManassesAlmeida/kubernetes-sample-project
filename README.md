Aplicação composta por 3 serviços: <i>backend (Java)</i>, <i>frontend (Angular)</i> e <i>database (MySQL)</i>.

O código-fonte responsável pela geração das imagens está disponível no meu projeto [docker-sample-project](https://github.com/ManassesAlmeida/docker-sample-project). A aplicação foi implantada e testada no Amazon EKS (Amazon Elastic Kubernetes Service).

# Executando localmente com Kubectl e Minikube
Passos para executar a aplicação:

1. Clonar o projeto do repositório.
    - Escolha um diretório e execute o comando: 
    <br><code>git clone https://github.com/ManassesAlmeida/kubernetes-sample-project.git</code>
2. Criar os ingresses, aplicando na ordem abaixo.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute os comandos: 
    <br><code>kubectl apply -f .\ingress\ingress-nginx-mandatory.yaml</code>
    <br><code>kubectl apply -f .\ingress\ingress-nginx-cloud-generic.yaml</code>
    <br><code>kubectl apply -f .\ingress\ingress.yaml</code>
3. Criar o namespace.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\namespaces\ </code>
4. Criar os deployments.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\deployments\ </code>
5. Criar os services.
    - No diretório <i>'kubernetes-sample-project'</i> (diretório raiz) execute o comando: <code>kubectl apply -f .\services\ </code>
6. Faça o redirecionamento de porta (necessário no Windows) do serviço <i>backend</i> e do serviço <i>frontend</i>.
    - Para redirecionar a porta do serviço backend:
        - Execute o comando: <code>minikube service backend --url -n product </code> e copie o número da porta impresso no console. Ex.: Se foi impresso 'http://192.168.49.2:30555', copie '30555'.
        - Execute o comando a seguir, substituindo XXXXX pelo número da porta copiado acima: <code>kubectl port-forward service/backend XXXXX:8080 -n product</code>. Exemplo: <code>kubectl port-forward service/backend 30555:8080 -n product</code>
        - Acesse o backend substituindo XXXXX pelo número da porta previamente copiado: 'http://localhost:XXXXX/api/products'. Ex.: 'http://localhost:30555/api/products'
    - Para redirecionar a porta do serviço frontend:
        - Execute o comando: <code>minikube service frontend --url -n product </code> e copie o número da porta impresso no console. Ex.: Se foi impresso 'http://192.168.49.2:31294', copie '31294'.
        - Execute o comando a seguir, substituindo YYYYY pelo número da porta copiado acima: <code>kubectl port-forward service/backend YYYYY:80 -n product</code>. Exemplo: <code> kubectl port-forward service/frontend 31294:80 -n product</code>
        - Acesse o frontend substituindo YYYYY pelo número da porta previamente copiado: 'http://localhost:YYYY/'. Ex.: 'http://localhost:31294/'

Para finalizar a execução e efetuar a exclusão dos pods/ingresses/deployments/services, execute os comandos: 
- <code>kubectl delete all --all -n ingress-nginx</code>
- <code>kubectl delete all --all -n product</code>
