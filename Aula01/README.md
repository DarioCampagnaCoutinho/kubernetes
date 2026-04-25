# Aula 01 - Primeiro deploy no Kubernetes com k3d

Este diretório contém um exemplo simples de deploy no Kubernetes usando um `Deployment` e um `Service`.

O arquivo [deploy.yml](C:/Users/dario/Documentos/Cursos/kubernetes/Aula01/deploy.yml) publica a aplicação `kubedevio/conversao-temperatura:v1` em um cluster local criado com `k3d`.

## Objetivo da aula

Nesta aula, a ideia é entender a base de uma aplicação rodando no Kubernetes:

- `Deployment`: define como os pods da aplicação devem ser criados.
- `Service`: expõe a aplicação para que ela possa ser acessada.
- `NodePort`: libera uma porta do cluster para acesso externo.

## O que existe no `deploy.yml`

O manifesto está dividido em dois recursos:

### 1. Deployment

O `Deployment` chamado `conversao`:

- usa a imagem `kubedevio/conversao-temperatura:v1`
- cria pods com a label `app: conversao`
- expõe a porta `8080` dentro do container

Isso significa que a aplicação escuta internamente na porta `8080`.

### 2. Service

O `Service` também se chama `conversao` e:

- seleciona os pods com a label `app: conversao`
- encaminha a porta `80` do serviço para a porta `8080` do container
- usa `type: NodePort`
- publica a porta `30000`

Na prática:

- porta do container: `8080`
- porta do serviço dentro do cluster: `80`
- porta exposta no nó: `30000`

## Pré-requisitos

Antes de executar a aula, você precisa ter instalado:

- `docker`
- `kubectl`
- `k3d`

## Criando o cluster com k3d

Se você ainda não tiver um cluster local, pode criar um com:

```powershell
k3d cluster create meucluster -p "30000:30000@server:0"
```

Esse comando:

- cria um cluster chamado `meucluster`
- faz o mapeamento da porta `30000` do host para a porta `30000` do node do cluster

Para verificar se o cluster foi criado:

```powershell
k3d cluster list
kubectl cluster-info
kubectl get nodes
```

## Aplicando o deploy

Com o cluster ativo, execute:

```powershell
kubectl apply -f .\deploy.yml
```

Depois confira os recursos criados:

```powershell
kubectl get deployments
kubectl get pods
kubectl get services
```

Se quiser acompanhar mais detalhes do pod:

```powershell
kubectl describe pod -l app=conversao
```

## Testando a aplicação

Como o serviço foi publicado via `NodePort` na porta `30000`, você pode acessar localmente por:

```text
http://localhost:30000
```

Se quiser testar via terminal:

```powershell
curl http://localhost:30000
```

## Comandos úteis da aula

Ver logs da aplicação:

```powershell
kubectl logs -l app=conversao
```

Reaplicar alterações no manifesto:

```powershell
kubectl apply -f .\deploy.yml
```

Remover os recursos da aula:

```powershell
kubectl delete -f .\deploy.yml
```

Apagar o cluster `k3d` quando terminar:

```powershell
k3d cluster delete meucluster
```

## Resumo do aprendizado

Na Aula 01, você pratica os conceitos mais importantes do começo de Kubernetes:

- como subir uma aplicação com `Deployment`
- como identificar pods usando `labels`
- como expor a aplicação com `Service`
- como acessar a aplicação localmente usando `NodePort`
- como usar o `k3d` para estudar Kubernetes de forma leve no ambiente local

Esse é o primeiro passo para entender como o Kubernetes organiza, executa e expõe aplicações em containers.
