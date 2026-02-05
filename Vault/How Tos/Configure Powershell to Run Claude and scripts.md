


Instalación de editor de terminal vim
```powershell
winget install vim.vim
```

Visualizar politicas de uso de script
```powershell
Get-ExecutionPolicy -List
```


Permitir al usuario el uso de scripts y herramientas de CLI (Como Claude o node)

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Visualizar variables de entorno
```powershell
 echo $env:PATH
```

Eliminar carpeta en powershell
```powershell
Remove-Item -Recurse -Force $HOME\.kube\http-cache\
```


Editar fichero profile del terminal para poder hacer uso de Alias y configuración custom
```powershell
notepad $PROFILE
```

```powershell
vim $PROFILE
```