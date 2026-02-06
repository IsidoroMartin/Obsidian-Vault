Table of Contents

1. [Corporate Email Requirement](#corporate-email-requirement)
2. [2. Mandatory Commit Signing](#2%C2%A0mandatory-commit-signing)
3. [Requisitos](#requisitos)
4. [1. Generación de la Clave SSH](#1-generaci%C3%B3n-de-la-clave-ssh)
5. [2. Configuración del Servicio OpenSSH Agent](#2-configuraci%C3%B3n-del-servicio-openssh-agent)
	1. [Opción A: Vía PowerShell (Rápido)](#opci%C3%B3n-a-v%C3%ADa-powershell-r%C3%A1pido)
	2. [Opción B: Vía GUI (Visual)](#opci%C3%B3n-b-v%C3%ADa-gui-visual)
6. [3. Configuración de KeePassXC](#3-configuraci%C3%B3n-de-keepassxc)
7. [4. Vinculación con GitHub](#4-vinculaci%C3%B3n-con-github)
8. [Clave de validación de cifrado de los commits (pública)](#clave-de-validaci%C3%B3n-de-cifrado-de-los-commits-p%C3%BAblica)
9. [Clave de autenticación SSH (pública)](#clave-de-autenticaci%C3%B3n-ssh-p%C3%BAblica)
10. [5. Configure Git for signing commits](#5-configure-git-for-signing-commits)
11. [6 Verify configuration](#6-verify-configuration)
12. [7. Configuración en IntelliJ IDEA](#7-configuraci%C3%B3n-en-intellij-idea)
13. [8. Troubleshooting](#8-troubleshooting)
	1. [Registrar claves SSH en Github](#registrar-claves-ssh-en-github)
	2. [Forzar el uso del cliente SSH nativo de Windows](#forzar-el-uso-del-cliente-ssh-nativo-de-windows)
	3. [Comandos de utilidad](#comandos-de-utilidad)
	4. [Troubleshooting KeePassXC](#troubleshooting-keepassxc)

# New Org Policy
## Corporate Email Requirement

All commits must be authored using a verified corporate email address (non-personal accounts). This ensures:

- Proper attribution of code changes to organization members
- Compliance with corporate security policies
- Clear audit trails for all code contributions

## 2. Mandatory Commit Signing

All commits must be cryptographically signed using SSH keys. This provides:

- Verification that commits come from authenticated users
- Protection against commit spoofing and impersonation
- Enhanced security posture for the entire codebase

https://github.com/masorange/new-org-wide-restrictions
# 🔑 Git SSH, KeePassXC y Automatización de clonado de repositorios desde la Terminal

Esta guía detalla la configuración de un entorno de desarrollo profesional en Windows. El objetivo es eliminar el uso de contraseñas/tokens y automatizar el flujo de trabajo mediante **SSH nativo** y **búsqueda difusa**.

---
## Requisitos

Se requieren privilegios de administrador para instalar la herramienta, y manejar el servicio de Open SSH
## 1. Generación de la Clave SSH

Utilizaremos el algoritmo **Ed25519**, que es el estándar actual por seguridad y rendimiento.


> [!warning] Importante, se debe usar **el mismo email** que el que tenemos configurado como principal en la cuenta de Github

1. Abre PowerShell y ejecuta:
    
    PowerShell
    ```powershell    
	ssh-keygen -t ed25519 -C "nombre.apellido.apellido@example.com" -f $env:USERPROFILE\.ssh\commit-signing-mo
    ```
    
2. Presiona `Enter` para aceptar la ruta (o cámbiala si lo prefieres).
    
3. Por seguridad, elige una contraseña (mientras la bóveda está abierta la primera vez que quieras usarla saldrá un popup y te pedira que la rellenes)

```powershell
# Global configuration (applies to all repositories)
git config --global user.email "nombre.apellido.apellido@example.com"
git config --global user.name "Your Name"

# Verify
git config --global user.email
```



---

## 2. Configuración del Servicio OpenSSH Agent

El agente es el proceso que mantiene las llaves en memoria. Debe estar activo para que Git pueda usarlas.

### Opción A: Vía PowerShell (Rápido)

PowerShell

```
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service ssh-agent
```

### Opción B: Vía GUI (Visual)

1. En la barra de windows busca "Servicios"
2. Abrir con click derecho como administrador
3. Busca **"Agente de autenticación OpenSSH"**.
4. Cambia el **Tipo de inicio** a **Automático** e **inicia el servicio**.

---
## 3. Configuración de KeePassXC

KeePassXC gestionará tu clave privada y la inyectará en el agente de Windows automáticamente.

1. Ve a **Herramientas > Configuración > Agente SSH**.
    
2. Marca **"Habilitar Agente SSH"** y selecciona **"Usar el Agente OpenSSH de Windows"**.
    
3. En la entrada de GitHub de tu base de datos:
    
    - Ve a la pestaña de **Avanzado** > Añadir archivo
    - Carga el fichero de la clave privada (`commit-signing-mo`)
    - Ve a la pestaña **Agente SSH** > **Adjunto**.
    - Elige tu llave privada en el desplegable (`commit-signing-mo`) en "Archivo de clave externa".
    - Marca **"Añadir clave al agente al abrir la base de datos"**.
        

---

## 4. Vinculación con GitHub

## Clave de validación de cifrado de los commits (pública)

> [!warning] En el desplegable Seleccionar **Key type: Signing Key**

1. Ejecuta en PowerShell: `cat $env:USERPROFILE\.ssh\commit-signing-mo.pub | clip`
    
2. Ve a **GitHub > Settings > SSH and GPG keys > New SSH Key**
	1. Title: commit-signing-mo
	2. Key type: Signing Key
	3. Key: Pega la clave pública


## Clave de autenticación SSH (pública)

Opcional, en caso de querer bajar y hacer push de los repositorios usando SSH, si se configura descarga el repo por HTTPS no es necesario.

> [!warning] En el desplegable Seleccionar **Authentication Key**

1. Ejecuta en PowerShell: `cat $env:USERPROFILE\.ssh\commit-signing-mo.pub | clip`
    
2. Ve a **GitHub > Settings > SSH and GPG keys > New SSH Key**
	1. Title: commit-signing-mo
	2. Key type: Authentication Key
	3. Key: Pega la clave pública
3. Validar el SSO de las org de masorange

---
## 5. Configure Git for signing commits
  
  **PowerShell Instructions**

```powershell
# Forzar a Git a usar OpenSSH nativo
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"

# Set SSH as the signing format
git config --global gpg.format ssh

# Specify your signing key
git config --global user.signingkey "$env:USERPROFILE\.ssh\commit-signing-mo.pub"

# Enable automatic commit signing
git config --global commit.gpgsign true

# Verify configuration
git config --global --list | Select-String "(gpg|signing)"
```

---
## 6 Verify configuration

**PowerShell:**

```powershell
# Clone the test repository (if not already done)
git clone https://github.com/masorange/new-org-wide-restrictions.git
Set-Location new-org-wide-restrictions

# Create a unique test branch
$email = git config user.email
$branchName = "test/verify-setup-$($email -replace '@','-at-' -replace '\.','-')"
git checkout -b $branchName

# Create your own test file
"Setup verified - $(Get-Date)" | Out-File -FilePath "test-commits\$(git config user.email).txt" -Encoding utf8
git add test-commits\
git commit -m "test: verify commit signing setup on Windows"

# Push to your test branch
git push -u origin HEAD

# Create PR (if you have GitHub CLI installed)
gh pr create --title "test: verify commit signing setup (Windows)" `
  --body "Testing commit signing compliance with osc-commit-signing ruleset from Windows"
```

---
## 7. Configuración en IntelliJ IDEA

Para que el IDE reconozca tu sesión SSH de la terminal:

1. Ve a **File > Settings > Version Control > Git**.
    
2. En **SSH executable**, selecciona **Native**.
    
3. Haz clic en `Test` para verificar la conexión.
## 8. Troubleshooting

### Registrar claves SSH en Github

Para clonar y pushear los repositorios es necesaria la clave SSH de tipo **Authentication Key**

Para aplicar la firma los commits es necesario la clave SSH de tipo **Signing Key**

### Forzar el uso del cliente SSH nativo de Windows
Si Git no reconoce el agente, asegúrate de que esté usando el ejecutable de OpenSSH del sistema en lugar de su versión interna:
```powershell
git config --global core.sshCommand "C:\Windows\System32\OpenSSH\ssh.exe"
```

### Comandos de utilidad

```powershell
# Validar que la clave esta correctamente cargada en el agente SSH
ssh-add -l

# Validar que el servicio de OpenSSH esta levantado
Get-Service ssh-agent
```
### Troubleshooting KeePassXC

Las claves SSH solamente están disponibles mientras la bóveda esta abierta, por lo que es necesario que se haya abierto la bóveda antes de abrir el IDE para que el IDE cargue correctamente las claves SSH. Si la bóveda se cierra el IDE deja de poder acceder a las claves SSH, por lo que es necesario volver a desbloquear la bóveda
