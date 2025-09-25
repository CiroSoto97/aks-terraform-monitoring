# Proyecto Final - AKS con Terraform 🚀

Este proyecto despliega un **clúster de Kubernetes en Azure (AKS)** utilizando **Terraform**, y configura monitoreo con **Prometheus** y **Grafana**.

---

## 📌 Requisitos

Antes de comenzar asegúrate de tener instalado:

- [Terraform](https://developer.hashicorp.com/terraform/downloads)  
- [Azure CLI](https://learn.microsoft.com/es-es/cli/azure/install-azure-cli)  
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)  
- Una **suscripción activa de Azure**  
- Permisos suficientes en la suscripción para crear **Resource Groups, AKS y asignar roles**  

---

## 🚀 Pasos de uso

### 1. Clonar el repositorio
```bash
git clone https://github.com/CiroSoto97/aks-terraform-monitoring.git
cd aks-terraform-monitoring
```

### 2. Autenticarse en Azure
Inicia sesión en Azure:
```bash
az login
```

Verifica tu suscripción activa:
```bash
az account show
```

Si es necesario, selecciona la suscripción correcta:
```bash
az account set --subscription <SubscriptionId>
```

### 3. Inicializar Terraform
Inicializa el proyecto para descargar los providers:
```bash
terraform init
```

### 4. Crear el plan de ejecución
Genera un plan para validar qué se va a crear:
```bash
terraform plan -out plan.tfplan
```

### 5. Aplicar el plan
Ejecuta la creación de la infraestructura:
```bash
terraform apply plan.tfplan
```

Esto creará:
- Un **Resource Group**
- Un **AKS Cluster**
- Los recursos de red necesarios

### 6. Configurar `kubectl`
Conecta `kubectl` a tu clúster recién creado:
```bash
az aks get-credentials --resource-group <tu_resource_group> --name <tu_nombre_aks>
kubectl get nodes
```

Si ves los nodos, el clúster está listo ✅.

---

## 📊 Monitoreo con Prometheus y Grafana

### 1. Instalar Helm (si no lo tienes)
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### 2. Instalar Prometheus
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus
```

Verifica que los pods estén corriendo:
```bash
kubectl get pods
```

### 3. Instalar Grafana
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana
```

Obtén la contraseña de admin de Grafana:
```bash
kubectl get secret grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Reenvía el puerto para acceder a Grafana:
```bash
kubectl port-forward svc/grafana 3000:80
```

Luego abre en tu navegador 👉 [http://localhost:3000](http://localhost:3000)

---

## 📈 Dashboards en Grafana

Dentro de Grafana:
1. Inicia sesión con usuario `admin` y la contraseña obtenida.  
2. Agrega **Prometheus** como fuente de datos.  
3. Importa dashboards preconstruidos desde [Grafana Dashboards](https://grafana.com/grafana/dashboards/).  

Ejemplos de dashboards recomendados:
- **Kubernetes cluster monitoring (Prometheus)**
- **Node Exporter Full**
- **API Server metrics**

---

## 📂 Estructura del Repositorio

```
aks-terraform-monitoring/
│── main.tf                # Configuración principal de Terraform
│── variables.tf           # Variables usadas en el proyecto
│── outputs.tf             # Salidas importantes (IP, credenciales, etc.)
│── aks.tf                 # Definición del AKS
│── README.md              # Documentación del proyecto
```


## 📝 Notas finales

- Si obtienes errores `403 Forbidden`, revisa que tu usuario tenga permisos **Contributor** en la suscripción y Resource Group.  
- Recuerda ejecutar:
  ```bash
  terraform destroy
  ```
  para eliminar todos los recursos creados y evitar costos adicionales en Azure.  

---

💡 **Autor:** Ciro Soto  
📅 Proyecto Final – AKS con Terraform y Monitoreo
