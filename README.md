
# Hadoop Docker Setup - WordCount Example

Este documento proporciona una guía para desplegar un clúster de Hadoop utilizando Docker y Docker Compose, y ejecutar el ejemplo de **WordCount** con MapReduce.

## Requisitos Previos

- **Cuenta en Azure** para crear una máquina virtual.
- **Docker** y **Docker Compose** instalados en tu sistema.
- Un directorio de trabajo donde estén todos los archivos necesarios, incluyendo `docker-compose.yml` y el archivo `hadoop-mapreduce-examples-3.2.1.jar`.

## Configuración de la Máquina Virtual en Azure

### 1. Crear una Máquina Virtual en Azure con Ubuntu 20.04 LTS

1. Accede al [Portal de Azure](https://portal.azure.com/) e inicia sesión.
2. Haz clic en "Crear un recurso" y selecciona "Máquina virtual".
3. Configura los detalles básicos de la máquina virtual:
   - **Grupo de recursos**: Crea uno nuevo o usa uno existente.
   - **Nombre de la VM**: Especifica un nombre, por ejemplo, `HadoopVM`.
   - **Región**: Selecciona la región de tu preferencia.
   - **Imagen**: Escoge "Ubuntu Server 20.04 LTS".
   - **Tamaño**: Selecciona un tamaño adecuado; `Standard_B2s` es suficiente para esta práctica.
4. Configura la autenticación con SSH:
   - **Nombre de usuario**: Por ejemplo, `azureuser`.
   - **Tipo de autenticación**: Selecciona "Clave pública SSH".
   - **Clave pública SSH**: Pega tu clave pública.
5. Revisa y crea la VM. 

### 2. Conectarse a la Máquina Virtual desde tu Terminal Local

Una vez creada la VM, conéctate a ella usando SSH:

```bash
ssh azureuser@<dirección_IP_pública_de_tu_VM>
```

Reemplaza `<dirección_IP_pública_de_tu_VM>` con la IP asignada a tu VM.

### 3. Instalar Docker y Docker Compose en la Máquina Virtual

1. **Actualizar paquetes e instalar Docker**:

   ```bash
   sudo apt update
   sudo apt install -y docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

2. **Agregar tu usuario al grupo Docker**:

   ```bash
   sudo usermod -aG docker $USER
   ```

   Cierra sesión y vuelve a iniciar sesión para aplicar los cambios.

3. **Instalar Docker Compose**:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   docker-compose --version
   ```

Este paso asegura que Docker y Docker Compose están configurados en la VM.

## Paso a Paso para Ejecutar el Ejemplo

### 1. Levantar el Clúster de Hadoop

Ejecuta el comando `docker-compose up` para levantar los servicios del clúster de Hadoop. Utiliza el flag `-d` para ejecutarlos en segundo plano:

```bash
docker-compose up -d
```

### 2. Verificar que los Servicios Están Corriendo

Asegúrate de que todos los servicios necesarios estén activos:

```bash
docker ps
```

Deberías ver los contenedores `namenode`, `datanode1`, `datanode2`, `datanode3`, `resourcemanager`, `nodemanager`, y `historyserver` en la lista.

### 3. Configuración de Permisos en el Directorio de Trabajo

Si tienes problemas de permisos en el directorio de trabajo, asegúrate de que el usuario tenga permisos totales:

1. Cambia la propiedad del directorio:
   ```bash
   sudo chown -R $USER:$USER /ruta/a/tu/directorio
   ```

2. Otorga permisos de lectura, escritura y ejecución al usuario:
   ```bash
   chmod -R u+rwx /ruta/a/tu/directorio
   ```

Esto garantiza que puedes ejecutar los comandos sin necesidad de `sudo`.
