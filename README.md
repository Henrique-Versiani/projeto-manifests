# Projeto: CI/CD com GitHub Actions - Manifestos Kubernetes

Este repositório implementa o conceito de **GitOps**: ele serve como a "fonte da verdade" para o estado desejado da nossa aplicação no cluster Kubernetes.

**Repositório da Aplicação (FastAPI):** `https://github.com/Henrique-Versiani/projeto-app.git`

## Ferramentas

* **ArgoCD:** Utilizado para a entrega contínua (CD), sincronizando os manifestos deste repositório com o cluster Kubernetes.
* **GitHub Actions:** (Definido no repositório `projeto-app`) É o responsável por atualizar automaticamente os manifestos neste repositório.

## Manifestos

* `deployment.yaml`: Define o "Deployment" da aplicação, especificando qual imagem Docker deve ser usada e quantas réplicas devem ser executadas.
* `service.yaml`: Define o "Service" do Kubernetes, que expõe a aplicação e permite que ela seja acessada dentro do cluster.

## Fluxo de CD (Entrega Contínua)

O processo de entrega contínua é gerenciado pelo ArgoCD da seguinte forma:

1.  **Monitoramento:** O ArgoCD está configurado na interface web para monitorar este repositório Git.
2.  **Atualização Automática (CI):** Quando uma nova versão da `projeto-app` é construída, o GitHub Actions (do outro repositório) faz um `push` automático para este repositório, atualizando a `image:` tag no arquivo `deployment.yaml`.
3.  **Detecção (ArgoCD):** O ArgoCD detecta que o estado definido no Git é diferente do estado atual do cluster (OutOfSync).
4.  **Sincronização (ArgoCD):** O ArgoCD aplica automaticamente as mudanças no cluster Kubernetes (Rancher Desktop) para que ele corresponda ao estado definido no Git, atualizando a aplicação para a nova versão.
