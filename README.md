# Kubernetes - Estudos e Aulas Práticas

Este repositório reúne exemplos e anotações práticas para estudar Kubernetes de forma progressiva, usando ambiente local com `k3d`.

O objetivo é aprender os conceitos principais da orquestração de containers e, ao mesmo tempo, praticar com manifestos reais em YAML.

## O que é Kubernetes

Kubernetes é uma plataforma de orquestração de containers.

Na prática, ele ajuda a:

- executar aplicações em containers
- escalar aplicações com facilidade
- manter os serviços disponíveis
- organizar comunicação entre aplicações
- automatizar deploys e atualizações

Em vez de subir containers manualmente um por um, o Kubernetes gerencia esse ciclo para você.

## Por que usar Kubernetes

Quando uma aplicação cresce, apenas rodar containers com Docker já não resolve tudo. Surge a necessidade de:

- manter múltiplas instâncias da aplicação rodando
- reiniciar containers com falha automaticamente
- distribuir tráfego entre várias réplicas
- atualizar versões sem interromper o serviço
- separar responsabilidades entre aplicações e ambientes

O Kubernetes existe para resolver esse tipo de problema de forma padronizada.

## Conceitos principais

### Cluster

O cluster é o conjunto de máquinas onde o Kubernetes roda.

Ele é composto por:

- plano de controle: responsável por gerenciar o cluster
- nós de trabalho: onde os containers realmente executam

No ambiente de estudo com `k3d`, esse cluster roda localmente usando Docker.

### Node

Node é uma máquina do cluster.

Ela pode ser:

- física
- virtual
- local, no caso de laboratório com `k3d`

É no node que os pods são executados.

### Pod

Pod é a menor unidade de execução no Kubernetes.

Normalmente, um pod contém:

- um container principal
- rede própria
- armazenamento efêmero

Em muitos cenários iniciais, podemos pensar no pod como o "container gerenciado pelo Kubernetes", embora tecnicamente ele seja um nível acima do container.

### Deployment

Deployment é o recurso usado para declarar como uma aplicação deve rodar.

Ele permite:

- criar pods
- manter a quantidade desejada de réplicas
- substituir pods com falha
- atualizar versões da aplicação

É um dos objetos mais usados em aplicações stateless.

### Service

Service é o recurso que cria um ponto estável de acesso para os pods.

Como pods podem ser recriados e mudar de IP, o `Service` resolve isso oferecendo uma forma fixa de comunicação.

Tipos comuns:

- `ClusterIP`: acesso interno dentro do cluster
- `NodePort`: expõe a aplicação em uma porta do node
- `LoadBalancer`: expõe externamente usando balanceador de carga

### Label e Selector

Labels são pares de chave e valor usados para identificar recursos.

Exemplo:

```yaml
labels:
  app: conversao
```

Selectors são filtros usados para encontrar recursos com determinadas labels.

Isso permite que um `Service` encontre os pods corretos, por exemplo.

### Namespace

Namespace é uma forma de organizar recursos dentro do cluster.

Ele ajuda a separar:

- aplicações
- equipes
- ambientes
- contextos diferentes no mesmo cluster

### Manifesto YAML

No Kubernetes, normalmente descrevemos os recursos em arquivos YAML.

Esses arquivos definem o estado desejado da aplicação, por exemplo:

- qual imagem usar
- quantas réplicas manter
- quais portas expor
- quais labels aplicar

Depois disso, usamos o `kubectl` para aplicar o manifesto no cluster.

## Ferramentas usadas neste repositório

### kubectl

É a ferramenta de linha de comando para interagir com o Kubernetes.

Exemplos:

```powershell
kubectl get pods
kubectl get services
kubectl apply -f .\deploy.yml
```

### k3d

`k3d` é uma forma simples de criar clusters Kubernetes locais usando `k3s` dentro de containers Docker.

Ele é ótimo para estudos porque:

- é leve
- sobe rápido
- funciona bem em ambiente local
- facilita testar manifestos sem depender de cloud

Exemplo de criação de cluster:

```powershell
k3d cluster create meucluster
```

## Estrutura do repositório

- [Aula01/README.md](/abs/c:/Users/dario/Documentos/Cursos/kubernetes/Aula01/README.md): primeiro deploy com `Deployment` e `Service`
- `Aula01/deploy.yml`: manifesto da aplicação da Aula 01
- `Aula02`: próxima etapa do conteúdo

## Fluxo de estudo sugerido

1. Entender os conceitos básicos deste README.
2. Criar um cluster local com `k3d`.
3. Entrar na [Aula01/README.md](/abs/c:/Users/dario/Documentos/Cursos/kubernetes/Aula01/README.md).
4. Aplicar o manifesto com `kubectl`.
5. Observar pods, services e acesso à aplicação.

## Resumo

Kubernetes é a camada de orquestração que organiza containers em um ambiente confiável e escalável.

Os conceitos mais importantes para começar são:

- cluster
- node
- pod
- deployment
- service
- labels e selectors
- manifestos YAML

Com essa base, fica muito mais fácil entender as aulas práticas do repositório.
