# Proyecto Final - DevOps Monitoring con AKS

## 1. Pasos para crear Kubernetes (AKS) con Terraform
1. Instalar Terraform y Azure CLI.
2. Loguearse en Azure:
   ```bash
   az login
   ```

3. Clonar este repositorio.
   ```bash
   git clone https://github.com/CiroSoto97/aks-terraform-monitoring.git
   cd aks-terraform-monitoring
   ```

4. Editar el archivo `variables.tf` y poner tu **subscription_id**.

5. Ejecutar:
   ```bash
   terraform init
   terraform plan
   terraform apply -auto-approve
   ```

6. Conectarse al clúster:
   ```bash
   az aks get-credentials -g rg-devops-monitoring -n aks-devops-monitoring
   kubectl get nodes
   ```
