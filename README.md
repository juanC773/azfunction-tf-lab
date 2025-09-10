# Azure Function con Terraform - Paso a Paso

# Link
https://olaolahey.azurewebsites.net/api/olaolahey

## Qué hicimos

Básicamente se tomó el repositorio del profe que ya tenía todo configurado para crear una función en Azure y la desplegamos usando Terraform.

## Paso a paso

### 1. Clonar el repositorio

Primero se hizo fork del repo del profe y luego se clonó:

```bash
git clone https://github.com/juanC773/azfunction-tf-lab.git
cd azfunction-tf-lab
```

### 2. Crear archivo de variables

El repo no viene con el archivo `terraform.tfvars` porque cada persona necesita sus propios valores. Se creó con los datos correspondientes:

## Recursos creados

- **Resource Group**: Contiene todos los recursos
- **Storage Account**: Para el código de la función  
- **Service Plan**: Plan de consumo (gratuito)
- **Function App**: La aplicación que ejecuta las funciones
- **Function**: La función HTTP en Node.js



```hcl
name_function   = "olaolahey"
location        = "West Europe"
subscription_id = "e0e272de-0790-496e-a716-36d36cc58eed"
```

**Importante:** El `name_function` tiene que ser único globalmente porque Azure lo usa para crear el Storage Account.

### 3. Configurar variables en el código

Agregué la variable de suscripción en `variables.tf`:

```hcl
variable "subscription_id" {
  type        = string
  description = "Azure Subscription ID"
}
```

Y la usé en `main.tf` en el provider:

```hcl
provider "azurerm" {
  features {}
  subscription_id = var.subscription_id
}
```

### 4. Autenticarse en Azure

```bash
az login
```

Esto abre el navegador para hacer login con tu cuenta de Azure for Students.

### 5. Inicializar Terraform

```bash
terraform init
```

Esto descarga los providers necesarios (en este caso azurerm) y prepara el proyecto.

### 6. Planificar el despliegue

```bash
terraform plan
```

Te muestra qué recursos va a crear sin ejecutar nada. Deben aparecer 5 recursos:
- Resource Group
- Storage Account  
- Service Plan
- Function App
- La función

### 7. Aplicar los cambios

```bash
terraform apply
```

Te pregunta si quieres continuar, escribes `yes` y esperas unos 5-7 minutos. Al final te da la URL de tu función.

## Resultado

La función queda desplegada en: `https://olaolahey.azurewebsites.net/api/olaolahey`

Responde con un mensaje básico y si le pasas un parámetro `name` te saluda personalizado.



## Capturas del proceso

![alt text](<WhatsApp Image 2025-09-10 at 16.44.42_24783bd6.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.53.18_b907ca2d.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.53.26_2e04ed0d.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.53.10_6a393604.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.45.45_997c871f.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.46.00_14818dd5.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.53.01_34906ba9.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 16.54.40_fe1ac3b1.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 17.00.44_8b050248.jpg>)
![alt text](<WhatsApp Image 2025-09-10 at 17.01.54_bc85b237.jpg>)
**Tecnologías usadas:** Terraform, Azure Functions, Node.js, Azure CLI