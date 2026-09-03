# Laboratorio Híbrido: Windows Server 2022 + Azure Arc + Microsoft Entra ID + Azure Policy

Proyecto de implementación de una infraestructura de nube híbrida a **coste cero (0 €)**, integrando un entorno local basado en Windows Server 2022 con los servicios de identidad, gestión y gobernanza de Microsoft Azure.

---

## 1. Arquitectura y Componentes

| Capa | Entorno / Recurso | Servicios y Configuración Clave |
|---|---|---|
| **Local (On-Premises)** | **Host:** Windows Server 2022 (`WSLab`)<br>**Dominio:** `corp.local` | • **AD DS:** Directorio activo local y usuarios (`xcladera`, `tuser`).<br>• **Azure Arc Agent:** Agente `azcmagent` conectado mediante HTTPS/TLS 1.3.<br>• **Microsoft Entra Connect:** Sincronización hash de contraseñas (PHS). |
| **Nube (Microsoft Azure)** | **Azure Arc-enabled Servers** | • Recurso proyectado: `WSLab`<br>• Grupo de recursos: `rg-hybrid-lab`<br>• Estado: **Connected** |
| | **Microsoft Entra ID (Free)** | • Tenant configurado.<br>• Identidades locales sincronizadas con éxito. |
| | **Azure Policy & Governance** | • Ámbito: `rg-hybrid-lab`<br>• Evaluación de directivas: **100% Compatible** |

### Componentes clave del despliegue:
* **Host Local:** Windows Server 2022 (`WSLab`) en dominio Active Directory local `corp.local`.
* **Azure Arc:** Proyección del plano de control de Azure Resource Manager (ARM) sobre el servidor físico/virtual on-premises.
* **Microsoft Entra ID:** Gestión centralizada de identidades en la nube (capa gratuita).
* **Microsoft Entra Connect:** Sincronización de identidades de Active Directory a Entra ID mediante *Password Hash Synchronization (PHS)*.
* **Azure Policy:** Evaluación continua de auditoría y cumplimiento normativo en el servidor híbrido.

---

## 🚀 2. Fases de Implementación

### Fase 1: Onboarding del Servidor Local en Azure Arc
1. Creación del grupo de recursos `rg-hybrid-lab` en Azure.
2. Generación del script de incorporación interactivo en la consola de Azure Arc.
3. Instalación de `AzureConnectedMachineAgent` en `WSLab`.
4. Autenticación contra Azure y validación de conectividad a los endpoints del plano de control (`his.arc.azure.com`, `guestconfiguration.azure.com`).
5. Verificación de estado **Connected** en Azure Portal.

### Fase 2: Sincronización de Identidades con Microsoft Entra Connect
1. Despliegue de Active Directory Domain Services (AD DS) en `WSLab` con el dominio `corp.local`.
2. Creación de usuarios y cuentas de servicio locales (`xcladera`, `tuser`).
3. Instalación de Microsoft Entra Connect para sincronizar el bosque local con el tenant `xavicladerahotmail.onmicrosoft.com`.
4. Configuración de sincronización de hash de contraseñas (*Password Hash Synchronization*).

### Fase 3: Gobernanza y Auditoría con Azure Policy
1. Asignación de directivas de gobernanza sobre el grupo de recursos `rg-hybrid-lab`:
   - Auditoría de etiquetas de recursos (`Require a tag on resources`).
   - Auditoría de membresía de grupos locales (`Audit Windows machines missing any of specified members in the Administrators group`).
2.  Lanzamos el escaneo de cumplimiento mediante Azure Cloud Shell para forzar la sincronización:
   ```powershell
   Start-AzPolicyComplianceScan
