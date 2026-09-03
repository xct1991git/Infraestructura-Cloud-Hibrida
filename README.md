Laboratorio Híbrido: Windows Server 2022 + Azure Arc + Microsoft Entra ID + Azure Policy

Proyecto de implementación de una infraestructura de nube híbrida a *coste cero (0 €)*, integrando un entorno local basado en Windows Server 2022 con los servicios de identidad, gestión y gobernanza de Microsoft Azure.

---

1. ARQUITECTURA Y COMPONENTES


ENTORNO ON-PREMISES (Local) :                         
----------------------------------------------
Windows Server 2022 Datacenter: WSLab                          
Dominio AD DS: corp.local                                        
                                                                 
- Active Directory Domain Services (AD DS)                
- Azure Connected Machine Agent (azcmagent)         
- Microsoft Entra Connect (Sincronización PHS)                       


Conectividad saliente segura (HTTPS / TLS 1.3)
-----------------------------------------------
------------------------------------------------------------------------------------
|                               MICROSOFT AZURE                                     |
|                                                                                   |
|    --------------------------     -----------------------     -----------------   |
|   |     Azure Arc-enabled    |   |  Microsoft Entra ID   |   |  Azure Policy  |   |
|   |          Servers         |   |      (Free Tier)      |   |  & Governance  |   |
|   |                          |   |                       |   |                |   |
|   |  - Recurso: WSLab        |   | - Tenant configurado  |   | - Ámbito:      |   |
|   |  - RG: rg-hybrid-lab     |   | - Identidades sync:   |   |   rg-hybrid-lab|   |
|   |  - Estado: Connected     |   |   xcladera, tuser...  |   | - WSLab:       |   |
|   |                          |   | - Sync Status: Activo |   |   Compatible   |   |
|    --------------------------     -----------------------     -----------------   

### Componentes clave del despliegue:
* **Host Local:** Windows Server 2022 (`WSLab`) en dominio Active Directory local `corp.local`.
* **Azure Arc:** Proyección del plano de control de Azure Resource Manager (ARM) sobre el servidor físico/virtual on-premises.
* **Microsoft Entra ID:** Gestión centralizada de identidades en la nube (capa gratuita).
* **Microsoft Entra Connect:** Sincronización de identidades de Active Directory a Entra ID mediante *Password Hash Synchronization (PHS)*.
* **Azure Policy:** Evaluación continua de auditoría y cumplimiento normativo en el servidor híbrido.

---

2. FASES DE IMPLEMENTACIÓN

Fase 1: Onboarding del Servidor Local en Azure Arc

1. Creación del grupo de recursos `rg-hybrid-lab` en Azure.
2. Generación del script de incorporación interactivo en la consola de Azure Arc.
3. Instalación de `AzureConnectedMachineAgent` en `WSLab`.
4. Autenticación contra Azure y validación de conectividad a los endpoints del plano de control (`his.arc.azure.com`, `guestconfiguration.azure.com`).
5. Verificación de estado **Connected** en Azure Portal.

Fase 2: Sincronización de Identidades con Microsoft Entra Connect

1. Despliegue de Active Directory Domain Services (AD DS) en `WSLab` con el dominio `corp.local`.
2. Creación de usuarios y cuentas de servicio locales (`xcladera`, `tuser`).
3. Instalación de Microsoft Entra Connect para sincronizar el bosque local con el tenant `xavicladerahotmail.onmicrosoft.com`.
4. Configuración de sincronización de hash de contraseñas (*Password Hash Synchronization*).

Fase 3: Gobernanza y Auditoría con Azure Policy

1. Asignación de directivas de gobernanza sobre el grupo de recursos `rg-hybrid-lab`:
   - Auditoría de etiquetas de recursos (`Require a tag on resources`).
   - Auditoría de membresía de grupos locales (`Audit Windows machines missing any of specified members in the Administrators group`).
2. Disparo de escaneo de cumplimiento bajo demanda mediante Azure Cloud Shell:
   ```powershell
   Start-AzPolicyComplianceScan
Validación del estado de cumplimiento normativo (Compatible - 100%) reflejado directamente sobre el recurso WSLab.
