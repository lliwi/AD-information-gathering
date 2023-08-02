# AD_information-gathering
AD information gathering scripts and command

## POWERVIEW
```
. .\powerview.ps1
```

### Enumeracion SAM
#### Grupos Locales
```
Get-NetLocalGroup
Get-NetGroup -UserName 'XXXX' | select name
Get-NetLocalGroupMember -GroupName 'XXXX' | Select-Object MemberName, IsGroup, IsDomain
```
#### Grupos remotos
```
Get-NetLocalGroup -ComputerName XXX --> Solo con permisos de admin
```
#### Usuarios logados y sessiones en el sistema
```
Get-NetLoggedon
Get-NetSession
```

### Enumeración NTDS
#### Informacion del dominio
```
Get-NetDomain
Get-DomainPolicy
(Get-DomainPolicy)."SystemAccess"
Get-NetDomainController
```
#### Usuarios del dominio
```
Get-NetUser
Get-NetUser | Select name, lastlogoff, lastlogon
```
#### Equipos del dominio
```
Get-NetComputer
Get-NetComputer | Select DNSHostName, SamAccountName, Name, operatingsystemversion
Get-NetComputer -Ping | Select name, operatingsystem
```
#### Grupos del dominio
```
Get-DomainGroup
Get-DomainGroup | Select grouptype, name, description
Get-NetGroupMember -Identity "XXXX"
Get-NetGroupMember -Identity "XXXX" | Select Membername
```

##### Carpetas compartidas
```
Find-DomainShare
```

##### Unidades administrativas
```
Get-NetOU
```

#### Politicas de dominio
```
Get-NetGPO
Get-NetGPO | Select displayname
Get-NetGPO -ComputerIdentity XXXX
```

#### ACL's
```
Get-ObjectAcl
Get-ObjectAcl -SamAccountName XXXX
```

#### Informacion del bosque
```
Get-NetForestDomain
```

#### Permisos locales
```
Find-LocalAdminAccess -Verbose
Invoke-EnumerateLocalAdmin -Verbose
Invoke-UserHunter -GroupName "RDPUsers"
```

## AD

### Enumeracion SAM
#### Grupos Locales
```
Get-LocalGroup | Select Name, objectclass, Principalsource, sid
Get-LocalGroupMember -Group 'XXXX'
Invoke-Command -ScriptBlock { Get-LocalGroupMember -Group 'XXXX' } -ComputerName XXX --> Solo con permisos de admin
```

### Enumeracion NTDS
#### Informacion del dominio
```
Get-ADDomain
Get-ADDomainController
```
#### Usuarios del dominio
```
Get-ADUser -Filer *
Get-AdUser -Filer * | Select Name, ObjectClass, ObjectGuid
```
#### Equipos del dominio
```
Get-ADComputer -Filter *
```

## RPCCLIENT
#### Acceso a terminal rpccclient
```
rpcclient -U "corp\USER%PASSWORD" AD.XX.XX
```

## IMPACKET
#### Dump SAM
```
impacket-samdump corp/USER:PASSWORD@AD.XX.XX
```

#### Escuchar sessiones de red a un equipo
```
impacket-netview corp/USER:PASSWORD@AD.XX.XX --> Solo con permisos de admin
```
