---
title: Eliminazione in modalità completa
description: Descrive in che modo i tipi di risorsa gestiscono l'eliminazione in modalità completa in modelli di Azure Resource Manager.
ms.topic: conceptual
ms.date: 10/21/2020
ms.openlocfilehash: e0c67bfcda81ad128e0018c4ab37c4b0cbe680f0
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "96184026"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Eliminazione di risorse di Azure per distribuzioni in modalità completa

Questo articolo descrive in che modo i tipi di risorsa gestiscono l'eliminazione quando non si trovano in un modello distribuito in modalità completa.

I tipi di risorsa contrassegnati con **Sì** vengono eliminati quando il tipo non è incluso nel modello distribuito con la modalità completa.

I tipi di risorsa contrassegnati con No non vengono eliminati automaticamente quando non sono **presenti** nel modello; Tuttavia, vengono eliminati se la risorsa padre viene eliminata. Per una descrizione completa del comportamento, vedere [Modalità di distribuzione di Azure Resource Manager](deployment-modes.md).

Se si esegue la distribuzione in [più di un gruppo di risorse in un modello](./deploy-to-resource-group.md), le risorse nel gruppo di risorse specificato nell'operazione di distribuzione sono idonee per essere eliminate. Le risorse nei gruppi di risorse secondarie non vengono eliminate.

Le risorse sono elencate in base allo spazio dei nomi del provider di risorse. Per associare uno spazio dei nomi del provider di risorse al nome del servizio di Azure, vedere [provider di risorse per i servizi di Azure](../management/azure-services-resource-providers.md).

> [!NOTE]
> Usare sempre l' [operazione](template-deploy-what-if.md) di simulazione prima di distribuire un modello in modalità completa. Cosa-se Mostra le risorse che verranno create, eliminate o modificate. Usare il tipo di simulazione per evitare l'eliminazione involontaria delle risorse.
Passare a uno spazio dei nomi del provider di risorse:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft. AgFoodPlatform](#microsoftagfoodplatform)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. automanage](#microsoftautomanage)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft. BareMetalInfrastructure](#microsoftbaremetalinfrastructure)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ChangeAnalysis](#microsoftchangeanalysis)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. codespaces](#microsoftcodespaces)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. ConnectedCache](#microsoftconnectedcache)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft. D365CustomerInsights](#microsoftd365customerinsights)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft. DeviceUpdate](#microsoftdeviceupdate)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft. HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft. Notebooks](#microsoftnotebooks)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft. ScVmm](#microsoftscvmm)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft. Singularity](#microsoftsingularity)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft. StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft. WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | DomainServices | Sì |
> | DomainServices/oucontainer | No |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | supportProviders | No |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | aadsupportcases | No |
> | addsservices | No |
> | agents | No |
> | anonymousapiusers | No |
> | configurazione | No |
> | log | No |
> | reports | No |
> | servicehealthmetrics | No |
> | services | No |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | advisorScore | No |
> | configurazioni | No |
> | generateRecommendations | No |
> | metadata | No |
> | raccomandazioni di film | No |
> | suppressions | No |

## <a name="microsoftagfoodplatform"></a>Microsoft. AgFoodPlatform

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | farmBeats | Sì |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | actionRules | Sì |
> | alerts | No |
> | alertsList | No |
> | alertsMetaData | No |
> | alertsSummary | No |
> | alertsSummaryList | No |
> | smartDetectorAlertRules | Sì |
> | smartGroups | No |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | servers | Sì |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | reportFeedback | No |
> | service | Sì |
> | validateServiceName | No |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | configurationStores | Sì |
> | configurationStores / eventGridFilters | No |
> | configurationStores/valori di valore | No |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Spring | Sì |
> | Primavera/app | No |
> | Spring/Apps/distribuzioni | No |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | attestationProviders | Sì |
> | defaultProviders | No |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | accessReviewScheduleDefinitions | No |
> | accessReviewScheduleSettings | No |
> | classicAdministrators | No |
> | dataalias | No |
> | denyAssignments | No |
> | elevateAccess | No |
> | findOrphanRoleAssignments | No |
> | locks | No |
> | autorizzazioni | No |
> | policyAssignments | No |
> | policyDefinitions | No |
> | policyExemptions | No |
> | policySetDefinitions | No |
> | privateLinkAssociations | No |
> | providerOperations | No |
> | resourceManagementPrivateLinks | Sì |
> | roleAssignments | No |
> | roleAssignmentsUsageMetrics | No |
> | roleDefinitions | No |

## <a name="microsoftautomanage"></a>Microsoft. automanage

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | configurationProfileAssignments | No |
> | configurationProfilePreferences | Sì |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | automationAccounts | Sì |
> | automationAccounts/configurazioni | Sì |
> | automationAccounts/processi | No |
> | automationAccounts/privateEndpointConnectionProxies | No |
> | automationAccounts/privateEndpointConnections | No |
> | automationAccounts/privateLinkResources | No |
> | automationAccounts/runbooks | Sì |
> | automationAccounts/softwareUpdateConfigurations | No |
> | automationAccounts/webhooks | No |

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | privateClouds | Sì |
> | privateClouds/addons | No |
> | privateClouds/autorizzazioni | No |
> | privateClouds/cluster | No |
> | privateClouds / globalReachConnections | No |
> | privateClouds / hcxEnterpriseSites | No |
> | privateClouds / workloadNetworks | No |
> | privateClouds / workloadNetworks / dhcpConfigurations | No |
> | privateClouds/workloadNetworks/gateway | No |
> | privateClouds / workloadNetworks / portMirroringProfiles | No |
> | privateClouds/workloadNetworks/segmenti | No |
> | privateClouds/workloadNetworks/virtualMachines | No |
> | privateClouds / workloadNetworks / vmGroups | No |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | environments | No |
> | ambienti/account | No |
> | ambienti/account/spazi dei nomi | No |
> | ambienti/account/spazi dei nomi/configurazioni | No |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | b2cDirectories | Sì |
> | b2ctenants | No |
> | guestUsages | Sì |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | datacontroller | Sì |
> | postgresInstances | Sì |
> | sqlManagedInstances | Sì |
> | sqlServerInstances | Sì |
> | sqlServerRegistrations | Sì |
> | sqlServerRegistrations/SQLServers | No |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | cloudManifestFiles | No |
> | edgeSubscriptions | Sì |
> | linkedSubscriptions | Sì |
> | registrations | Sì |
> | registrazioni/customerSubscriptions | No |
> | registrations/products | No |

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |

## <a name="microsoftbaremetalinfrastructure"></a>Microsoft. BareMetalInfrastructure

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | bareMetalInstances | Sì |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | batchAccounts | Sì |
> | batchAccounts/certificates | No |
> | batchAccounts/pools | No |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | billingAccounts | No |
> | billingAccounts/contratti | No |
> | billingAccounts / billingPermissions | No |
> | billingAccounts / billingProfiles | No |
> | billingAccounts / billingProfiles / billingPermissions | No |
> | billingAccounts / billingProfiles / billingRoleAssignments | No |
> | billingAccounts / billingProfiles / billingRoleDefinitions | No |
> | billingAccounts / billingProfiles / billingSubscriptions | No |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | No |
> | billingAccounts/billingProfiles/clienti | No |
> | billingAccounts/billingProfiles/istruzioni | No |
> | billingAccounts/billingProfiles/fatture | No |
> | billingAccounts/billingProfiles/fatture/pricesheets | No |
> | billingAccounts/billingProfiles/fatture/transazioni | No |
> | billingAccounts / billingProfiles / invoiceSections | No |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | No |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | No |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | No |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | No |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | No |
> | billingAccounts/billingProfiles/invoiceSections/Products | No |
> | billingAccounts/billingProfiles/invoiceSections/Products/Transfer | No |
> | billingAccounts/billingProfiles/invoiceSections/Products/updateAutoRenew | No |
> | billingAccounts/billingProfiles/invoiceSections/Transactions | No |
> | billingAccounts/billingProfiles/invoiceSections/trasferimenti | No |
> | billingAccounts / BillingProfiles / patchOperations | No |
> | billingAccounts / billingProfiles / paymentMethods | No |
> | billingAccounts/billingProfiles/criteri | No |
> | billingAccounts/billingProfiles/pricesheets | No |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | No |
> | billingAccounts/billingProfiles/Products | No |
> | billingAccounts/billingProfiles/prenotazioni | No |
> | billingAccounts/billingProfiles/Transactions | No |
> | billingAccounts / billingProfiles / validateDetachPaymentMethodEligibility | No |
> | billingAccounts / billingRoleAssignments | No |
> | billingAccounts / billingRoleDefinitions | No |
> | billingAccounts / billingSubscriptions | No |
> | billingAccounts/billingSubscriptions/fatture | No |
> | billingAccounts / createBillingRoleAssignment | No |
> | billingAccounts / createInvoiceSectionOperations | No |
> | billingAccounts/clienti | No |
> | billingAccounts/Customers/billingPermissions | No |
> | billingAccounts/Customers/billingSubscriptions | No |
> | billingAccounts/Customers/initiateTransfer | No |
> | billingAccounts/clienti/criteri | No |
> | billingAccounts/clienti/prodotti | No |
> | billingAccounts/clienti/transazioni | No |
> | billingAccounts/clienti/trasferimenti | No |
> | billingAccounts/reparti | No |
> | billingAccounts/reparti/billingPermissions | No |
> | billingAccounts/reparti/billingRoleAssignments | No |
> | billingAccounts/reparti/billingRoleDefinitions | No |
> | billingAccounts / enrollmentAccounts | No |
> | billingAccounts / enrollmentAccounts / billingPermissions | No |
> | billingAccounts / enrollmentAccounts / billingRoleAssignments | No |
> | billingAccounts / enrollmentAccounts / billingRoleDefinitions | No |
> | billingAccounts/fatture | No |
> | billingAccounts/fatture/transazioni | No |
> | billingAccounts / invoiceSections | No |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | No |
> | billingAccounts / invoiceSections / billingSubscriptions | No |
> | billingAccounts/invoiceSections/billingSubscriptions/Transfer | No |
> | billingAccounts/invoiceSections/eleva | No |
> | billingAccounts / invoiceSections / initiateTransfer | No |
> | billingAccounts / invoiceSections / patchOperations | No |
> | billingAccounts / invoiceSections / productMoveOperations | No |
> | billingAccounts/invoiceSections/Products | No |
> | billingAccounts/invoiceSections/Products/Transfer | No |
> | billingAccounts/invoiceSections/Products/updateAutoRenew | No |
> | billingAccounts/invoiceSections/Transactions | No |
> | billingAccounts/invoiceSections/trasferimenti | No |
> | billingAccounts / lineOfCredit | No |
> | billingAccounts / patchOperations | No |
> | billingAccounts / paymentMethods | No |
> | billingAccounts/prodotti | No |
> | billingAccounts/prenotazioni | No |
> | billingAccounts/transazioni | No |
> | billingPeriods | No |
> | billingPermissions | No |
> | billingProperty | No |
> | billingRoleAssignments | No |
> | billingRoleDefinitions | No |
> | createBillingRoleAssignment | No |
> | departments | No |
> | enrollmentAccounts | No |
> | invoices | No |
> | transfers | No |
> | trasferimenti/acceptTransfer | No |
> | trasferimenti/declineTransfer | No |
> | trasferimenti/operationStatus | No |
> | trasferimenti/validateTransfer | No |
> | validateAddress | No |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | mapApis | Sì |
> | updateCommunicationPreference | No |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | blockchainMembers | Sì |
> | cordaMembers | Sì |
> | watchers | Sì |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | TokenServices | Sì |
> | TokenServices / BlockchainNetworks | No |
> | TokenServices/gruppi | No |
> | TokenServices/gruppi/account | No |
> | TokenServices / TokenTemplates | No |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | blueprintAssignments | No |
> | blueprintAssignments / assignmentOperations | No |
> | blueprintAssignments/operazioni | No |
> | blueprints | No |
> | blueprints/artifacts | No |
> | blueprints/versions | No |
> | blueprints/versions/artifacts | No |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | botServices | Sì |
> | botServices/channels | No |
> | botServices/connessioni | No |
> | languages | No |
> | Modelli | No |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Redis | Sì |
> | Redis/EventGridFilters | No |
> | Redis/privateEndpointConnectionProxies | No |
> | Redis/privateEndpointConnectionProxies/convalida | No |
> | Redis/privateEndpointConnections | No |
> | Redis/privateLinkResources | No |
> | redisEnterprise | Sì |
> | RedisEnterprise / privateEndpointConnectionProxies | No |
> | RedisEnterprise/privateEndpointConnectionProxies/convalida | No |
> | RedisEnterprise / privateEndpointConnections | No |
> | RedisEnterprise / privateLinkResources | No |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | appliedReservations | No |
> | autoQuotaIncrease | No |
> | calculateExchange | No |
> | calculatePrice | No |
> | calculatePurchasePrice | No |
> | catalogs | No |
> | commercialReservationOrders | No |
> | exchange | No |
> | ownReservations | No |
> | placePurchaseOrder | No |
> | reservationOrders | No |
> | reservationOrders / calculateRefund | No |
> | reservationOrders/merge | No |
> | reservationOrders/prenotazioni | No |
> | reservationOrders/prenotazioni/revisioni | No |
> | reservationOrders/return | No |
> | reservationOrders/Split | No |
> | reservationOrders/swap | No |
> | reservations | No |
> | resourceProviders | No |
> | resources | No |
> | validateReservationOrder | No |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | No |
> | CdnWebApplicationFirewallPolicies | Sì |
> | edgenodes | No |
> | Profili | Sì |
> | profiles/endpoints | Sì |
> | profiles/endpoints/customdomains | No |
> | profiles/endpoints/origingroups | No |
> | profiles/endpoints/origins | No |
> | validateProbe | No |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | certificateOrders | Sì |
> | Ordini/certificati | No |
> | validateCertificateRegistrationInformation | No |

## <a name="microsoftchangeanalysis"></a>Microsoft.ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | profile | No |
> | resourceChanges | No |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | capabilities | No |
> | domainNames | Sì |
> | domainNames/funzionalità | No |
> | domainNames/internalLoadBalancers | No |
> | domainNames/serviceCertificates | No |
> | domainNames/slot | No |
> | domainNames/slot/ruoli | No |
> | domainNames/slot/ruoli/metricDefinitions | No |
> | domainNames/slot/ruoli/metriche | No |
> | moveSubscriptionResources | No |
> | operatingSystemFamilies | No |
> | operatingSystems | No |
> | quotas | No |
> | resourceTypes | No |
> | validateSubscriptionMoveAvailability | No |
> | virtualMachines | Sì |
> | virtualMachines/diagnosticSettings | No |
> | virtualMachines/metricDefinitions | No |
> | virtualMachines/metriche | No |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | classicInfrastructureResources | No |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | capabilities | No |
> | expressRouteCrossConnections | No |
> | expressRouteCrossConnections/peering | No |
> | gatewaySupportedDevices | No |
> | networkSecurityGroups | Sì |
> | quotas | No |
> | reservedIps | Sì |
> | virtualNetworks | Sì |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | No |
> | virtualNetworks/virtualNetworkPeerings | No |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | capabilities | No |
> | disks | No |
> | images | No |
> | osImages | No |
> | osPlatformImages | No |
> | publicImages | No |
> | quotas | No |
> | storageAccounts | Sì |
> | storageAccounts/blobServices | No |
> | storageAccounts/fileServices | No |
> | storageAccounts/metricDefinitions | No |
> | storageAccounts/metriche | No |
> | storageAccounts/queueServices | No |
> | storageAccounts/servizi | No |
> | storageAccounts/Services/diagnosticSettings | No |
> | storageAccounts/Services/metricDefinitions | No |
> | storageAccounts/Servizi/metriche | No |
> | storageAccounts/tableServices | No |
> | storageAccounts/VMImage | No |
> | vmImages | No |

## <a name="microsoftcodespaces"></a>Microsoft. codespaces

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | plans | Sì |
> | registeredSubscriptions | No |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | account/privateEndpointConnectionProxies | No |
> | account/privateEndpointConnections | No |
> | account/privateLinkResources | No |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | RateCard | No |
> | UsageAggregates | No |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | availabilitySets | Sì |
> | cloudServices | Sì |
> | cloudServices/networkInterfaces | No |
> | cloudServices/publicIPAddresses | No |
> | cloudServices/roleInstances | No |
> | cloudServices/roleInstances/networkInterfaces | No |
> | cloudServices/ruoli | No |
> | diskAccesses | Sì |
> | diskEncryptionSets | Sì |
> | disks | Sì |
> | galleries | Sì |
> | galleries/applications | No |
> | raccolte/applicazioni/versioni | No |
> | galleries/images | No |
> | galleries/images/versions | No |
> | Gruppi host | Sì |
> | Gruppi host/host | Sì |
> | images | Sì |
> | proximityPlacementGroups | Sì |
> | restorePointCollections | Sì |
> | restorePointCollections / restorePoints | No |
> | sharedVMExtensions | Sì |
> | sharedVMExtensions/versioni | No |
> | sharedVMImages | Sì |
> | sharedVMImages/versioni | No |
> | snapshots | Sì |
> | sshPublicKeys | Sì |
> | virtualMachines | Sì |
> | virtualMachines/estensioni | Sì |
> | virtualMachines/metricDefinitions | No |
> | virtualMachines/runCommands | Sì |
> | virtualMachineScaleSets | Sì |
> | virtualMachineScaleSets/estensioni | No |
> | virtualMachineScaleSets/networkInterfaces | No |
> | virtualMachineScaleSets/publicIPAddresses | No |
> | virtualMachineScaleSets/virtualMachines | No |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | No |

## <a name="microsoftconnectedcache"></a>Microsoft. ConnectedCache

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | CacheNodes | Sì |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | AggregatedCost | No |
> | Saldi | No |
> | Budget | No |
> | Charges | No |
> | CostTags | No |
> | credits | No |
> | eventi | No |
> | Previsioni | No |
> | lots | No |
> | Marketplace | No |
> | Pricesheets | No |
> | products | No |
> | ReservationDetails | No |
> | ReservationRecommendationDetails | No |
> | ReservationRecommendations | No |
> | ReservationSummaries | No |
> | ReservationTransactions | No |
> | Tag | No |
> | tenants | No |
> | Termini | No |
> | UsageDetails | No |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | containerGroups | Sì |
> | serviceAssociationLinks | No |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | registries | Sì |
> | registri/agentPools | Sì |
> | registries/builds | No |
> | registries/builds/cancel | No |
> | registri/compilazioni/getLogLink | No |
> | registries/buildTasks | Sì |
> | registri/buildTasks/passaggi | No |
> | registri/eventGridFilters | No |
> | registri/exportPipelines | No |
> | registri/generateCredentials | No |
> | registri/getBuildSourceUploadUrl | No |
> | registri/GetCredentials | No |
> | registri/importImage | No |
> | registri/importPipelines | No |
> | registri/pipelineRuns | No |
> | registri/privateEndpointConnectionProxies | No |
> | registri/privateEndpointConnectionProxies/convalida | No |
> | registri/privateEndpointConnections | No |
> | registri/privateLinkResources | No |
> | registri/queueBuild | No |
> | registri/regenerateCredential | No |
> | registri/regenerateCredentials | No |
> | registries/replications | Sì |
> | registries/runs | No |
> | registries/runs/cancel | No |
> | registri/scheduleRun | No |
> | registries/scopeMaps | No |
> | registri/taskRuns | No |
> | registries/tasks | Sì |
> | registries/tokens | No |
> | registri/updatePolicies | No |
> | registries/webhooks | Sì |
> | registri/webhook/getCallbackConfig | No |
> | registries/webhooks/ping | No |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | containerServices | Sì |
> | managedClusters | Sì |
> | openShiftManagedClusters | Sì |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Avvisi | No |
> | BillingAccounts | No |
> | Budget | No |
> | CloudConnectors | No |
> | Connettori | Sì |
> | costAllocationRules | No |
> | Departments | No |
> | Dimensioni | No |
> | EnrollmentAccounts | No |
> | Esporta | No |
> | ExternalBillingAccounts | No |
> | ExternalBillingAccounts/avvisi | No |
> | ExternalBillingAccounts/dimensioni | No |
> | ExternalBillingAccounts/previsione | No |
> | ExternalBillingAccounts/query | No |
> | ExternalSubscriptions | No |
> | ExternalSubscriptions/avvisi | No |
> | ExternalSubscriptions/dimensioni | No |
> | ExternalSubscriptions/previsione | No |
> | ExternalSubscriptions/query | No |
> | Previsione | No |
> | Informazioni dettagliate | No |
> | Query | No |
> | register | No |
> | Reportconfigs | No |
> | Report | No |
> | Impostazioni | No |
> | showbackRules | No |
> | Viste | No |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | requests | No |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | associations | No |
> | resourceProviders | Sì |

## <a name="microsoftd365customerinsights"></a>Microsoft. D365CustomerInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | instances | Sì |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | jobs | Sì |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Sì |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | aree di lavoro | Sì |
> | aree di lavoro/dbWorkspaces | No |
> | aree di lavoro/virtualNetworkPeerings | No |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | catalogs | Sì |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | dataFactories | Sì |
> | datafactories/diagnosticSettings | No |
> | datafactories/metricDefinitions | No |
> | dataFactorySchema | No |
> | factories | Sì |
> | factories/integrationRuntimes | No |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | accounts/dataLakeStoreAccounts | No |
> | accounts/storageAccounts | No |
> | account/storageAccounts/contenitori | No |
> | account/transferAnalyticsUnits | No |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | account/eventGridFilters | No |
> | accounts/firewallRules | No |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | services | Sì |
> | services/projects | Sì |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | BackupVaults | Sì |
> | ResourceOperationGateKeepers | Sì |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | accounts/shares | No |
> | accounts/shares/datasets | No |
> | accounts/shares/invitations | No |
> | accounts/shares/providersharesubscriptions | No |
> | Accounts/shares/synchronizationSettings | No |
> | accounts/sharesubscriptions | No |
> | account/sharesubscriptions/consumerSourceDataSets | No |
> | accounts/sharesubscriptions/datasetmappings | No |
> | accounts/sharesubscriptions/triggers | No |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | servers | Sì |
> | servers/advisors | No |
> | servers/keys | No |
> | Server/privateEndpointConnectionProxies | No |
> | Server/privateEndpointConnections | No |
> | Server/privateLinkResources | No |
> | Server/queryTexts | No |
> | Server/recoverableServers | No |
> | Server/avvio | No |
> | Server/arresta | No |
> | Server/topQueryStatistics | No |
> | servers/virtualNetworkRules | No |
> | Server/waitStatistics | No |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | flexibleServers | Sì |
> | servers | Sì |
> | servers/advisors | No |
> | servers/keys | No |
> | Server/privateEndpointConnectionProxies | No |
> | Server/privateEndpointConnections | No |
> | Server/privateLinkResources | No |
> | Server/queryTexts | No |
> | Server/recoverableServers | No |
> | Server/avvio | No |
> | Server/arresta | No |
> | Server/topQueryStatistics | No |
> | Server/aggiornamento | No |
> | servers/virtualNetworkRules | No |
> | Server/waitStatistics | No |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | flexibleServers | Sì |
> | serverGroups | Sì |
> | servers | Sì |
> | servers/advisors | No |
> | servers/keys | No |
> | Server/privateEndpointConnectionProxies | No |
> | Server/privateEndpointConnections | No |
> | Server/privateLinkResources | No |
> | Server/queryTexts | No |
> | Server/recoverableServers | No |
> | Server/topQueryStatistics | No |
> | servers/virtualNetworkRules | No |
> | Server/waitStatistics | No |
> | serversv2 | Sì |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | artifactSources | Sì |
> | rollouts | Sì |
> | serviceTopologies | Sì |
> | serviceTopologies/servizi | Sì |
> | serviceTopologies/Services/serviceUnits | Sì |
> | steps | Sì |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | applicationgroups | Sì |
> | applicationgroups/applications | No |
> | applicationgroups/desktops | No |
> | applicationgroups/startmenuitems | No |
> | hostpools | Sì |
> | hostpools / msixpackages | No |
> | hostpools/sessionhosts | No |
> | hostpools/sessionhosts/usersessions | No |
> | hostpools/usersessions | No |
> | aree di lavoro | Sì |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | ElasticPools | Sì |
> | ElasticPools / IotHubTenants | Sì |
> | ElasticPools/IotHubTenants/securitySettings | No |
> | Hub IoT | Sì |
> | IotHubs/eventGridFilters | No |
> | IotHubs/securitySettings | No |
> | ProvisioningServices | Sì |
> | usages | No |

## <a name="microsoftdeviceupdate"></a>Microsoft. DeviceUpdate

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | account/istanze | Sì |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | pipelines | Sì |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | controllers | Sì |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | labcenters | Sì |
> | labs | Sì |
> | labs/environments | Sì |
> | Lab/serviceRunners | Sì |
> | Lab/virtualMachines | Sì |
> | schedules | Sì |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | digitalTwinsInstances | Sì |
> | digitalTwinsInstances/endpoint | No |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | databaseAccountNames | No |
> | databaseAccounts | Sì |
> | restorableDatabaseAccounts | No |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | domains | Sì |
> | domini/domainOwnershipIdentifiers | No |
> | generateSsoRequest | No |
> | topLevelDomains | No |
> | validateDomainRegistrationInformation | No |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | lcsprojects | No |
> | lcsprojects / clouddeployments | No |
> | lcsprojects/connettori | No |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | services | Sì |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | domains | Sì |
> | domains/topics | No |
> | eventSubscriptions | No |
> | extensionTopics | No |
> | partnerNamespaces | Sì |
> | partnerNamespaces/eventChannels | No |
> | partnerRegistrations | Sì |
> | partnerTopics | Sì |
> | partnerTopics/eventSubscriptions | No |
> | systemTopics | Sì |
> | systemTopics/eventSubscriptions | No |
> | topics | Sì |
> | topicTypes | No |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |
> | spazi dei nomi | Sì |
> | namespaces/authorizationrules | No |
> | namespaces/disasterrecoveryconfigs | No |
> | namespaces/eventhubs | No |
> | namespaces/eventhubs/authorizationrules | No |
> | namespaces/eventhubs/consumergroups | No |
> | namespaces/networkrulesets | No |
> | spazi dei nomi/privateEndpointConnections | No |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | experimentWorkspaces | Sì |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | spazi dei nomi | Sì |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | featureProviders | No |
> | funzionalità | No |
> | provider | No |
> | subscriptionFeatureRegistrations | No |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | enroll | No |
> | galleryitems | No |
> | generateartifactaccessuri | No |
> | myareas | No |
> | aree/aree | No |
> | aree/aree/area | No |
> | aree/aree/area/galleryitems | No |
> | aree/galleryitems di rete | No |
> | aree/galleryitems | No |
> | register | No |
> | resources | No |
> | retrieveresourcesbyid | No |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | autoManagedAccounts | Sì |
> | autoManagedVmConfigurationProfiles | Sì |
> | configurationProfileAssignments | No |
> | guestConfigurationAssignments | No |
> | software | No |
> | softwareUpdateProfile | No |
> | softwareUpdates | No |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | hanaInstances | Sì |
> | sapMonitors | Sì |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | dedicatedHSMs | Sì |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |
> | clusters/applications | No |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | services | Sì |
> | Servizi/iomtconnectors | No |
> | Servizi/iomtconnectors/connessioni | No |
> | Servizi/iomtconnectors/mapping | No |
> | Servizi/privateEndpointConnectionProxies | No |
> | Servizi/privateEndpointConnections | No |
> | Servizi/privateLinkResources | No |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | machines | Sì |
> | computer/assessPatches | No |
> | machines/extensions | Sì |
> | computer/installPatches | No |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | datamanager | Sì |

## <a name="microsofthybridnetwork"></a>Microsoft. HybridNetwork

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | dispositivi | Sì |
> | networkFunctions | Sì |
> | networkFunctionVendors | No |
> | registeredSubscriptions | No |
> | fornitori | No |
> | fornitori/vendorSkus | No |
> | fornitori/vendorSkus/previewSubscriptions | No |
> | virtualNetworkFunctions | Sì |
> | virtualNetworkFunctionVendors | No |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | components | Sì |
> | networkScopes | Sì |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | jobs | Sì |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | appTemplates | No |
> | IoTApps | Sì |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Grafico | Sì |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | deletedVaults | No |
> | hsmPools | Sì |
> | managedHSMs | Sì |
> | insiemi di credenziali | Sì |
> | insiemi di credenziali/accessPolicies | No |
> | insiemi di credenziali/eventGridFilters | No |
> | insiemi di credenziali/chiavi | No |
> | insiemi di credenziali/chiavi/versioni | No |
> | vaults/secrets | No |

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | connectedClusters | Sì |
> | registeredSubscriptions | No |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Estensioni | No |
> | sourceControlConfigurations | No |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |
> | clusters/attacheddatabaseconfigurations | No |
> | clusters/databases | No |
> | clusters/databases/dataconnections | No |
> | clusters/databases/eventhubconnections | No |
> | clusters/databases/principalassignments | No |
> | cluster/DataConnection | No |
> | clusters/principalassignments | No |
> | cluster/sharedidentities | No |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | labaccounts | Sì |
> | users | No |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | hostingEnvironments | Sì |
> | integrationAccounts | Sì |
> | integrationServiceEnvironments | Sì |
> | integrationServiceEnvironments/managedApis | Sì |
> | isolatedEnvironments | Sì |
> | flussi di lavoro | Sì |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | commitmentPlans | Sì |
> | webServices | Sì |
> | Aree di lavoro | Sì |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | aree di lavoro | Sì |
> | aree di lavoro/batchEndpoints | Sì |
> | aree di lavoro/batchEndpoints/distribuzioni | Sì |
> | aree di lavoro/codici | No |
> | aree di lavoro/codici/versioni | No |
> | workspaces/computes | No |
> | aree di lavoro/archivi dati | No |
> | aree di lavoro/eventGridFilters | No |
> | aree di lavoro/processi | No |
> | aree di lavoro/labelingJobs | No |
> | aree di lavoro/linkedServices | No |
> | aree di lavoro/modelli | No |
> | aree di lavoro/modelli/versioni | No |
> | aree di lavoro/onlineEndpoints | Sì |
> | aree di lavoro/onlineEndpoints/distribuzioni | Sì |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | applyUpdates | No |
> | configurationAssignments | No |
> | maintenanceConfigurations | Sì |
> | publicMaintenanceConfigurations | No |
> | updates | No |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Identità | No |
> | userAssignedIdentities | Sì |

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | managedNetworks | Sì |
> | managedNetworks / managedNetworkGroups | Sì |
> | managedNetworks / managedNetworkPeeringPolicies | Sì |
> | notifica | Sì |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | No |
> | registrationAssignments | No |
> | registrationDefinitions | No |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | getEntities | No |
> | managementGroups | No |
> | managementGroups/impostazioni | No |
> | resources | No |
> | startTenantBackfill | No |
> | tenantBackfillStatus | No |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | account/eventGridFilters | No |
> | account/privateAtlases | Sì |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Macc | No |
> | offers | No |
> | offerTypes | No |
> | offerTypes/Publisher | No |
> | offerTypes/Publisher/offerte | No |
> | offerTypes/Publisher/offerte/piani | No |
> | offerTypes/Publisher/offerte/piani/contratti | No |
> | offerTypes/Publisher/offerte/piani/configurazioni | No |
> | offerTypes/Publisher/offerte/piani/configurazioni/importImage | No |
> | privategalleryitems | No |
> | privateStoreClient | No |
> | privateStores | No |
> | privateStores/offerte | No |
> | products | No |
> | publishers | No |
> | publishers/offers | No |
> | publishers/offers/amendments | No |
> | register | No |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | classicDevServices | Sì |
> | updateCommunicationPreference | No |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | agreements | No |
> | offertypes | No |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | mediaservices | Sì |
> | MediaServices/accountFilters | No |
> | mediaservices/assets | No |
> | MediaServices/assets/assetFilters | No |
> | MediaServices/contentKeyPolicies | No |
> | MediaServices/eventGridFilters | No |
> | MediaServices/liveEventOperations | No |
> | mediaservices/liveEvents | Sì |
> | mediaservices/liveEvents/liveOutputs | No |
> | MediaServices/liveOutputOperations | No |
> | MediaServices/mediaGraphs | No |
> | MediaServices/privateEndpointConnectionOperations | No |
> | MediaServices/privateEndpointConnectionProxies | No |
> | MediaServices/privateEndpointConnections | No |
> | MediaServices/streamingEndpointOperations | No |
> | mediaservices/streamingEndpoints | Sì |
> | MediaServices/streamingLocators | No |
> | MediaServices/streamingPolicies | No |
> | mediaservices/transforms | No |
> | mediaservices/transforms/jobs | No |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | appClusters | Sì |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | assessmentProjects | Sì |
> | migrateprojects | Sì |
> | moveCollections | Sì |
> | projects | Sì |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Sì |
> | objectUnderstandingAccounts | Sì |
> | remoteRenderingAccounts | Sì |
> | spatialAnchorsAccounts | Sì |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | netAppAccounts | Sì |
> | netAppAccounts / accountBackups | No |
> | netAppAccounts / capacityPools | Sì |
> | netAppAccounts/capacityPools/volumi | Sì |
> | netAppAccounts/capacityPools/volumi/snapshot | No |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | applicationGateways | Sì |
> | applicationGatewayWebApplicationFirewallPolicies | Sì |
> | applicationSecurityGroups | Sì |
> | azureFirewallFqdnTags | No |
> | azureFirewalls | Sì |
> | bastionHosts | Sì |
> | bgpServiceCommunities | No |
> | connections | Sì |
> | ddosCustomPolicies | Sì |
> | ddosProtectionPlans | Sì |
> | dnsOperationStatuses | No |
> | dnszones | Sì |
> | dnszones/A | No |
> | dnszones/AAAA | No |
> | dnszones/all | No |
> | dnszones/CAA | No |
> | dnszones/CNAME | No |
> | dnszones/MX | No |
> | dnszones/NS | No |
> | dnszones/PTR | No |
> | dnszones/recordsets | No |
> | dnszones/SOA | No |
> | dnszones/SRV | No |
> | dnszones/TXT | No |
> | expressRouteCircuits | Sì |
> | expressRouteCrossConnections | Sì |
> | expressRouteGateways | Sì |
> | expressRoutePorts | Sì |
> | expressRouteServiceProviders | No |
> | firewallPolicies | Sì |
> | frontdoors | Sì |
> | frontdoorWebApplicationFirewallManagedRuleSets | No |
> | frontdoorWebApplicationFirewallPolicies | Sì |
> | getDnsResourceReference | No |
> | internalNotify | No |
> | ipGroups | Sì |
> | loadBalancers | Sì |
> | localNetworkGateways | Sì |
> | natGateways | Sì |
> | networkIntentPolicies | Sì |
> | networkInterfaces | Sì |
> | networkProfiles | Sì |
> | networkSecurityGroups | Sì |
> | networkWatchers | Sì |
> | networkWatchers / connectionMonitors | Sì |
> | networkWatchers/dashboard | Sì |
> | networkWatchers/lenti | Sì |
> | networkWatchers / pingMeshes | Sì |
> | p2sVpnGateways | Sì |
> | privateDnsOperationStatuses | No |
> | privateDnsZones | Sì |
> | privateDnsZones/A | No |
> | privateDnsZones/AAAA | No |
> | privateDnsZones/tutti | No |
> | privateDnsZones/CNAME | No |
> | privateDnsZones/MX | No |
> | privateDnsZones/PTR | No |
> | privateDnsZones/SOA | No |
> | privateDnsZones/SRV | No |
> | privateDnsZones/TXT | No |
> | privateDnsZones/virtualNetworkLinks | Sì |
> | privateEndpoints | Sì |
> | privateLinkServices | Sì |
> | publicIPAddresses | Sì |
> | publicIPPrefixes | Sì |
> | routeFilters | Sì |
> | routeTables | Sì |
> | serviceEndpointPolicies | Sì |
> | trafficManagerGeographicHierarchies | No |
> | trafficmanagerprofiles | Sì |
> | trafficmanagerprofiles/heatMaps | No |
> | trafficManagerUserMetricsKeys | No |
> | virtualHubs | Sì |
> | virtualNetworkGateways | Sì |
> | virtualNetworks | Sì |
> | virtualNetworks/subnet | No |
> | virtualNetworkTaps | Sì |
> | virtualWans | Sì |
> | vpnGateways | Sì |
> | vpnSites | Sì |
> | webApplicationFirewallPolicies | Sì |

## <a name="microsoftnotebooks"></a>Microsoft. Notebooks

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | NotebookProxies | No |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | spazi dei nomi | Sì |
> | namespaces/notificationHubs | Sì |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | osNamespaces | Sì |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | HyperVSites | Sì |
> | ImportSites | Sì |
> | MasterSites | Sì |
> | ServerSites | Sì |
> | VMwareSites | Sì |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |
> | deletedWorkspaces | No |
> | linkTargets | No |
> | storageInsightConfigs | No |
> | aree di lavoro | Sì |
> | aree di lavoro/esportazioni | No |
> | aree di lavoro/origini dati | No |
> | aree di lavoro/linkedServices | No |
> | aree di lavoro/linkedStorageAccounts | No |
> | workspaces/metadata | No |
> | workspaces/query | No |
> | aree di lavoro/scopedPrivateLinkProxies | No |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | managementassociations | No |
> | managementconfigurations | Sì |
> | solutions | Sì |
> | Viste | Sì |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | legacyPeerings | No |
> | peerAsns | No |
> | peerings | Sì |
> | peeringServiceCountries | No |
> | peeringServiceProviders | No |
> | peeringServices | Sì |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | attestazioni | No |
> | policyEvents | No |
> | policyMetadata | No |
> | policyStates | No |
> | policyTrackedResources | No |
> | remediations | No |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | consoles | No |
> | dashboards | Sì |
> | userSettings | No |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | privateLinkServicesForPowerBI | Sì |
> | tenants | Sì |
> | tenant/aree di lavoro | No |
> | workspaceCollections | Sì |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | capacities | Sì |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | deletedAccounts | No |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | providerRegistrations | No |
> | providerRegistrations / defaultRollouts | No |
> | providerRegistrations / resourceTypeRegistrations | No |
> | rollouts | Sì |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Aree di lavoro | Sì |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | backupProtectedItems | No |
> | insiemi di credenziali | Sì |

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | OpenShiftClusters | Sì |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | spazi dei nomi | Sì |
> | namespaces/authorizationrules | No |
> | namespaces/hybridconnections | No |
> | namespaces/hybridconnections/authorizationrules | No |
> | spazi dei nomi/privateEndpointConnections | No |
> | namespaces/wcfrelays | No |
> | namespaces/wcfrelays/authorizationrules | No |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | query | Sì |
> | resourceChangeDetails | No |
> | resourceChanges | No |
> | resources | No |
> | resourcesHistory | No |
> | subscriptionsStatus | No |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | availabilityStatuses | No |
> | childAvailabilityStatuses | No |
> | childResources | No |
> | emergingissues | No |
> | eventi | No |
> | impactedResources | No |
> | metadata | No |
> | Notifiche | No |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | calculateTemplateHash | No |
> | deployments | No |
> | deployments/operations | No |
> | deploymentScripts | Sì |
> | deploymentScripts/log | No |
> | collegamenti | No |
> | notifyResourceJobs | No |
> | provider | No |
> | resourceGroups | No |
> | subscriptions | No |
> | templateSpecs | Sì |
> | templateSpecs/versioni | Sì |
> | tenants | No |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | scala Web | Sì |
> | saasresources | No |

## <a name="microsoftscvmm"></a>Microsoft. ScVmm

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | Cloud | Sì |
> | VirtualMachines | Sì |
> | VirtualMachineTemplates | Sì |
> | VirtualNetworks | Sì |
> | vmmservers | Sì |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | resourceHealthMetadata | No |
> | searchServices | Sì |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | No |
> | advancedThreatProtectionSettings | No |
> | alerts | No |
> | alertsSuppressionRules | No |
> | allowedConnections | No |
> | applicationWhitelistings | No |
> | assessmentMetadata | No |
> | assessments | No |
> | autoDismissAlertsRules | No |
> | automations | Sì |
> | AutoProvisioningSettings | No |
> | Compliances | No |
> | dell'account di integrazione | No |
> | dataCollectionAgents | No |
> | deviceSecurityGroups | No |
> | discoveredSecuritySolutions | No |
> | externalSecuritySolutions | No |
> | InformationProtectionPolicies | No |
> | iotDefenderSettings | No |
> | iotSecuritySolutions | Sì |
> | iotSecuritySolutions / analyticsModels | No |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | No |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | No |
> | iotSecuritySolutions / iotAlerts | No |
> | iotSecuritySolutions / iotAlertTypes | No |
> | iotSecuritySolutions / iotRecommendations | No |
> | iotSecuritySolutions / iotRecommendationTypes | No |
> | iotSensors | No |
> | jitNetworkAccessPolicies | No |
> | jitPolicies | No |
> | criteri | No |
> | pricings | No |
> | regulatoryComplianceStandards | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls | No |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | No |
> | secureScoreControlDefinitions | No |
> | secureScoreControls | No |
> | secureScores | No |
> | secureScores / secureScoreControls | No |
> | securityContacts | No |
> | securitySolutions | No |
> | securitySolutionsReferenceData | No |
> | securityStatuses | No |
> | securityStatusesSummaries | No |
> | serverVulnerabilityAssessments | No |
> | Scheda Impostazioni | No |
> | sqlVulnerabilityAssessments | No |
> | sottovalutazioni | No |
> | attività | No |
> | topologies | No |
> | workspaceSettings | No |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | aggregations | No |
> | alertRules | No |
> | alertRuleTemplates | No |
> | automationRules | No |
> | bookmarks | No |
> | cases | No |
> | DataConnector | No |
> | dataConnectorsCheckRequirements | No |
> | entities | No |
> | entityQueries | No |
> | incidents | No |
> | officeConsents | No |
> | Scheda Impostazioni | No |
> | threatIntelligence | No |
> | watchlists | No |

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | consoleServices | No |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | spazi dei nomi | Sì |
> | namespaces/authorizationrules | No |
> | namespaces/disasterrecoveryconfigs | No |
> | namespaces/eventgridfilters | No |
> | namespaces/networkrulesets | No |
> | spazi dei nomi/privateEndpointConnections | No |
> | namespaces/queues | No |
> | namespaces/queues/authorizationrules | No |
> | namespaces/topics | No |
> | namespaces/topics/authorizationrules | No |
> | namespaces/topics/subscriptions | No |
> | namespaces/topics/subscriptions/rules | No |
> | premiumMessagingRegions | No |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | scala Web | Sì |
> | clusters | Sì |
> | clusters/applications | No |
> | containerGroups | Sì |
> | containerGroupSets | Sì |
> | edgeclusters | Sì |
> | edgeclusters/applicazioni | No |
> | managedclusters | Sì |
> | managedclusters/NodeTypes | No |
> | networks | Sì |
> | secretstores | Sì |
> | secretstores/certificati | No |
> | secretstores/segreti | No |
> | volumes | Sì |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | scala Web | Sì |
> | containerGroups | Sì |
> | gateways | Sì |
> | networks | Sì |
> | chiavi private | Sì |
> | volumes | Sì |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | providerRegistrations | No |
> | providerRegistrations / resourceTypeRegistrations | No |
> | rollouts | Sì |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | SignalR | Sì |
> | SignalR/eventGridFilters | No |

## <a name="microsoftsingularity"></a>Microsoft. Singularity

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | account/accountQuotaPolicies | No |
> | account/groupPolicies | No |
> | account/processi | No |
> | account/storageContainers | No |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | hybridUseBenefits | No |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | applicationDefinitions | Sì |
> | scala Web | Sì |
> | jitRequests | Sì |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | managedInstances | Sì |
> | managedInstances/database | Sì |
> | managedInstances/databases/backupShortTermRetentionPolicies | No |
> | managedInstances/database/schemi/tabelle/colonne/sensitivityLabels | No |
> | managedInstances/databases/vulnerabilityAssessments | No |
> | managedInstances/databases/vulnerabilityAssessments/Rules/Baselines | No |
> | managedInstances/encryptionProtector | No |
> | managedInstances/chiavi | No |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | No |
> | managedInstances/vulnerabilityAssessments | No |
> | servers | Sì |
> | servers/administrators | No |
> | Server/communicationLinks | No |
> | servers/databases | Sì |
> | Server/encryptionProtector | No |
> | servers/firewallRules | No |
> | servers/keys | No |
> | Server/restorableDroppedDatabases | No |
> | servers/serviceobjectives | No |
> | Server/tdeCertificates | No |
> | virtualClusters | No |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Sì |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | No |
> | SqlVirtualMachines | Sì |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | deletedAccounts | No |
> | storageAccounts | Sì |
> | storageAccounts/blobServices | No |
> | storageAccounts/fileServices | No |
> | storageAccounts/queueServices | No |
> | storageAccounts/servizi | No |
> | storageAccounts/Services/metricDefinitions | No |
> | storageAccounts/tableServices | No |
> | usages | No |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | caches | Sì |
> | cache/storageTargets | No |
> | usageModels | No |

## <a name="microsoftstoragereplication"></a>Microsoft. StorageReplication

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | replicationGroups | No |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | storageSyncServices | Sì |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No |
> | storageSyncServices / syncGroups / serverEndpoints | No |
> | storageSyncServices/flussi di lavoro | No |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | storageSyncServices | Sì |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No |
> | storageSyncServices / syncGroups / serverEndpoints | No |
> | storageSyncServices/flussi di lavoro | No |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | storageSyncServices | Sì |
> | storageSyncServices/registeredServers | No |
> | storageSyncServices/syncGroups | No |
> | storageSyncServices / syncGroups / cloudEndpoints | No |
> | storageSyncServices / syncGroups / serverEndpoints | No |
> | storageSyncServices/flussi di lavoro | No |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | managers | Sì |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | clusters | Sì |
> | cluster/privateEndpoints | No |
> | streamingjobs | Sì |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | acceptChangeTenant | No |
> | alias | No |
> | cancel | No |
> | changeTenantRequest | No |
> | changeTenantStatus | No |
> | CreateSubscription | No |
> | abilitare | No |
> | ridenominazione | No |
> | SubscriptionDefinitions | No |
> | SubscriptionOperations | No |
> | subscriptions | No |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | privateLinkHubs | Sì |
> | aree di lavoro | Sì |
> | aree di lavoro/bigDataPools | Sì |
> | aree di lavoro/operationStatuses | No |
> | aree di lavoro/SQLDatabase | Sì |
> | aree di lavoro/sqlpool | Sì |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | environments | Sì |
> | environments/accessPolicies | No |
> | environments/eventsources | Sì |
> | environments/referenceDataSets | Sì |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | stores | Sì |
> | Archivi/accessPolicies | No |
> | stores/services | No |
> | stores/services/tokens | No |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | imageTemplates | Sì |
> | imageTemplates / runOutputs | No |

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | ArcZones | Sì |
> | ResourcePools | Sì |
> | VCenter | Sì |
> | VirtualMachines | Sì |
> | VirtualMachineTemplates | Sì |
> | VirtualNetworks | Sì |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Sì |
> | dedicatedCloudServices | Sì |
> | virtualMachines | Sì |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | dispositivi | Sì |
> | registeredSubscriptions | No |
> | fornitori | No |
> | fornitori/SKU | No |
> | fornitori/vnfs | No |
> | virtualNetworkFunctionSkus | No |
> | vnfs | Sì |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | account | Sì |
> | plans | Sì |
> | registeredSubscriptions | No |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | apiManagementAccounts | No |
> | Account/ACL | No |
> | Account/API | No |
> | Account/API/ACL | No |
> | Account/API/ACL | No |
> | Account/API/connessioni | No |
> | Account/API/Connections/ACL | No |
> | Account/API/localizedDefinitions | No |
> | Account/ACL | No |
> | Account/connessioni | No |
> | billingMeters | No |
> | certificates | Sì |
> | connectionGateways | Sì |
> | connections | Sì |
> | customApis | Sì |
> | deletedSites | No |
> | hostingEnvironments | Sì |
> | hostingEnvironments/eventGridFilters | No |
> | hostingEnvironments/multiRolePools | No |
> | hostingEnvironments/dei pool | No |
> | kubeEnvironments | Sì |
> | publishingUsers | No |
> | raccomandazioni di film | No |
> | resourceHealthMetadata | No |
> | runtimes | No |
> | serverFarms | Sì |
> | serverFarms/eventGridFilters | No |
> | serverFarms/firstPartyApps | No |
> | serverFarms/firstPartyApps/keyVaultSettings | No |
> | siti | Sì |
> | siti/configurazione  | No |
> | siti/eventGridFilters | No |
> | siti/hostNameBindings | No |
> | siti/file networkconfig | No |
> | sites/premieraddons | Sì |
> | sites/slots | Sì |
> | siti/slot/eventGridFilters | No |
> | siti/slot/hostNameBindings | No |
> | siti/slot/file networkconfig | No |
> | sourceControls | No |
> | staticSites | Sì |
> | validate | No |
> | verifyHostingEnvironmentVnet | No |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | diagnosticSettings | No |
> | diagnosticSettingsCategories | No |

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | multipleActivationKeys | Sì |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | DeviceServices | Sì |

## <a name="microsoftworkloadbuilder"></a>Microsoft. WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | carichi di lavoro | Sì |
> | carichi di lavoro/istanze | No |
> | carichi di lavoro/versioni | No |
> | carichi di lavoro/versioni/artefatti | No |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo di risorsa | Eliminazione in modalità completa |
> | ------------- | ----------- |
> | components | No |
> | componentsSummary | No |
> | monitorInstances | No |
> | monitorInstancesSummary | No |
> | monitors | No |
> | notificationSettings | No |

## <a name="next-steps"></a>Passaggi successivi

Per ottenere gli stessi dati come file con valori delimitati da virgole, scaricare [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).