---

title: "Gestionar configuraciones de Git para múltiples servicios (GIT)"
date: 2025-01-07
tags: [git, github]
---

Este blog nació de mi necesidad de organizar las configuraciones de los servicios de control de versiones que utilizo:
- **GitLab** para el trabajo,
- **GitHub** para proyectos personales que quiero compartir,
- **Gogs** (inicialmente como prueba, pero terminó siendo mi espacio para guardar configuraciones y backups privados junto a un familiar).

Separar cada servicio es clave para evitar confusiones y errores al realizar modificaciones.

---

#### **1. Un solo servicio**
Si solo usas un servicio, configura tus credenciales globales:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu.email@ejemplo.com"
```

Estos valores se aplicarán a todos los repositorios.

---

#### **2. Múltiples servicios**
##### **Opción 1: Configuración por repositorio**
Para cada repositorio, establece credenciales específicas:

```bash
cd /ruta/a/tu/repositorio
git config user.name "Tu Nombre para Gogs"
git config user.email "tu.email@gogs.com"
```

**Configura el remoto correctamente**:
```bash
git remote add origin https://gogs.ejemplo.com/usuario/repositorio.git
# o para GitHub/GitLab:
git remote add origin https://github.com/usuario/repositorio.git
```

---

##### **Opción 2: Uso de SSH (recomendado)**
1. **Genera claves SSH** (si no las tienes):
   ```bash
   ssh-keygen -t rsa -b 4096 -C "tu.email@ejemplo.com"
   ```

2. **Agrega las claves al agente SSH**:
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_rsa_gogs
   ssh-add ~/.ssh/id_rsa_github
   ssh-add ~/.ssh/id_rsa_gitlab
   ```

3. **Configura `~/.ssh/config`**:
   ```bash
   # Gogs
   Host gogs
       HostName gogs.ejemplo.com
       User git
       IdentityFile ~/.ssh/id_rsa_gogs

   # GitHub
   Host github
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa_github

   # GitLab
   Host gitlab
       HostName gitlab.com
       User git
       IdentityFile ~/.ssh/id_rsa_gitlab
   ```

4. **Usa URLs SSH para los remotos**:
   ```bash
   git remote add origin gogs:usuario/repositorio.git
   git remote add origin github:usuario/repositorio.git
   git remote add origin gitlab:usuario/repositorio.git
   ```

---

#### **Bonus: Credenciales temporales**
Si necesitas colaborar en un repositorio ajeno sin modificar tu configuración global, usa la URL con credenciales:
```bash
git remote set-url origin https://username:token@github.com/usuario/repositorio.git
```
*(Reemplaza `token` por un [Personal Access Token](https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic) con permisos de `repo`)*


**Nota**: Verifica siempre el directorio y la configuración antes de ejecutar comandos.
