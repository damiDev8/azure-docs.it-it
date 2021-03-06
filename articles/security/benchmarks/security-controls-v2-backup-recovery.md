---
title: Benchmark di sicurezza di Azure V2-backup e ripristino
description: Backup e ripristino del benchmark di sicurezza di Azure V2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 089cf521a7c5428833be340001c88b870c568a8f
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368886"
---
# <a name="security-control-v2-backup-and-recovery"></a>Controllo di sicurezza V2: backup e ripristino

Il servizio di backup e ripristino copre i controlli per garantire che i backup dei dati e della configurazione nei diversi livelli di servizio vengano eseguiti, convalidati e protetti.

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: garantire backup automatici regolari

| ID Azure | Controlli CIS v 7.1 ID/i | ID del NIST SP 800-53 R4 |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Assicurarsi di eseguire il backup dei sistemi e dei dati per mantenere la continuità aziendale dopo un evento imprevisto. Questo deve essere definito da tutti gli obiettivi per l'obiettivo del punto di ripristino (RPO) e l'obiettivo del tempo di ripristino (RTO).

Abilitare backup di Azure e configurare l'origine di backup, ad esempio macchine virtuali di Azure, SQL Server, database HANA o condivisioni file, nonché la frequenza e il periodo di memorizzazione desiderati.  

Per un livello di protezione più elevato, è possibile abilitare l'opzione di archiviazione con ridondanza geografica per replicare i dati di backup in un'area secondaria e ripristinare tramite il ripristino tra aree.

- [Continuità aziendale e ripristino di emergenza su scala aziendale](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

- [Come abilitare backup di Azure](../../backup/index.yml)

- [Come abilitare il ripristino tra aree geografiche](../../backup/backup-azure-arm-restore-vms.md#cross-region-restore)

**Responsabilità**: Customer

**Stakeholder** per la sicurezza dei clienti ([altre informazioni](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Criteri e standard](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Architettura di sicurezza](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Sicurezza dell'infrastruttura e degli endpoint](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Preparazione agli eventi imprevisti](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: crittografare i dati di backup

| ID Azure | Controlli CIS v 7.1 ID/i | ID del NIST SP 800-53 R4 |
|--|--|--|--|
| BR-2 | 10,2 | CP-9 |

Assicurarsi che i backup siano protetti da attacchi. Questa operazione dovrebbe includere la crittografia dei backup per evitare la perdita di riservatezza.   

Per i backup locali con backup di Azure, la crittografia inattiva viene fornita usando la passphrase fornita. Per i normali backup dei servizi di Azure, i dati di backup vengono crittografati automaticamente usando le chiavi gestite dalla piattaforma Azure. È possibile scegliere di crittografare i backup usando la chiave gestita dal cliente. In questo caso, verificare che la chiave gestita dal cliente nell'insieme di credenziali delle chiavi sia presente anche nell'ambito di backup. 

Usare il controllo degli accessi in base al ruolo di Azure in backup di Azure, Azure Key Vault o altre risorse per proteggere i backup e le chiavi gestite dal cliente. Inoltre, è possibile abilitare le funzionalità di sicurezza avanzate per richiedere l'autenticazione a più fattori prima che i backup possano essere modificati o eliminati.

- [Panoramica delle funzionalità di sicurezza in Backup di Azure](../../backup/security-overview.md)

- [Crittografia dei dati di backup tramite chiavi gestite dal cliente](../../backup/encryption-at-rest-with-cmk.md) 

- [Come eseguire il backup di chiavi di Key Vault in Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

- [Funzionalità di sicurezza per proteggere i backup ibridi dagli attacchi](../../backup/backup-azure-security-feature.md#prevent-attacks)

**Responsabilità**: Customer

**Stakeholder** per la sicurezza dei clienti ([altre informazioni](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Architettura di sicurezza](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Sicurezza dell'infrastruttura e degli endpoint](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Preparazione agli eventi imprevisti](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Convalidare tutti i backup che includono chiavi gestite dal cliente

| ID Azure | Controlli CIS v 7.1 ID/i | ID del NIST SP 800-53 R4 |
|--|--|--|--|
| BR-3 | 10.3 | CP-4, CP-9 |

Eseguire periodicamente il ripristino dei dati del backup. Assicurarsi che sia possibile ripristinare le chiavi gestite dal cliente di cui è stato eseguito il backup.

- [Come ripristinare i file dal backup della macchina virtuale di Azure](../../backup/backup-azure-restore-files-from-vm.md)

- [Come ripristinare chiavi di Key Vault in Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Responsabilità**: Customer

**Stakeholder** per la sicurezza dei clienti ([altre informazioni](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Preparazione agli eventi imprevisti](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Gestione della conformità della sicurezza](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Attenuare il rischio di chiavi perse

| ID Azure | Controlli CIS v 7.1 ID/i | ID del NIST SP 800-53 R4 |
|--|--|--|--|
| BR-4 | 10.4 | CP-9 |

Assicurarsi di disporre di misure per prevenire e ripristinare la perdita di chiavi. Abilitare l'eliminazione temporanea e la protezione dalla rimozione definitiva in Azure Key Vault per proteggere le chiavi da eliminazioni accidentali o dannose.  

- [Come abilitare l'eliminazione temporanea e la protezione dalla rimozione definitiva in Key Vault](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Responsabilità**: Customer

**Stakeholder** per la sicurezza dei clienti ([altre informazioni](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Architettura di sicurezza](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Preparazione agli eventi imprevisti](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Sicurezza dei dati](/azure/cloud-adoption-framework/organize/cloud-security-data-security)