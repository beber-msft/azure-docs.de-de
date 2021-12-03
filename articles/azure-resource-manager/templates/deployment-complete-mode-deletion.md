---
title: Löschung des vollständigen Modus
description: Zeigt, wie die Ressourcentypen die Löschung des vollständigen Modus in Azure Resource Manager-Vorlagen verarbeiten.
ms.topic: conceptual
ms.date: 10/20/2021
ms.openlocfilehash: ea3ea4de33a1680961f078e8e3cc8815da45956e
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130229897"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Löschen von Azure-Ressourcen für Bereitstellungen im vollständigen Modus

Dieser Artikel beschreibt, wie Ressourcentypen das Löschen handhaben, wenn sie sich nicht in einer Vorlage befinden, die im vollständigen Modus bereitgestellt wird.

Die mit **Ja** markierten Ressourcentypen werden gelöscht, wenn sich der Typ nicht in der Vorlage befindet, die im vollständigen Modus bereitgestellt wird.

Die mit **Nein** markierten Ressourcentypen werden nicht automatisch gelöscht, wenn sie sich nicht in der Vorlage befinden. Sie werden jedoch gelöscht, wenn die übergeordnete Ressource gelöscht wird. Eine vollständige Beschreibung des Verhaltens finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).

Wenn Sie in [mehr als einer Ressourcengruppe in einer Vorlage](./deploy-to-resource-group.md) bereitstellen, können Ressourcen, die sich in der Ressourcengruppe befinden, die im Bereitstellungsvorgang angegeben wurde, gelöscht werden. Ressourcen in den sekundären Ressourcengruppen werden nicht gelöscht.

Die Ressourcen werden nach dem Ressourcenanbieter-Namespace aufgelistet. Informationen zur Zuordnung des Namespace von Ressourcenanbietern zu den entsprechenden Azure-Diensten finden Sie unter [Ressourcenanbieter für Azure-Dienste](../management/azure-services-resource-providers.md).

> [!NOTE]
> Verwenden Sie immer den [Was-wäre-wenn-Vorgang](./deploy-what-if.md), bevor Sie eine Vorlage im vollständigen Modus bereitstellen. Anhand von Was-wäre-wenn-Vorgänge können Sie sehen, welche Ressourcen erstellt, gelöscht oder geändert werden. Verwenden Sie Was-wäre-wenn-Vorgänge, um ein unbeabsichtigtes Löschen von Ressourcen zu vermeiden.

Navigieren Sie direkt zu einem Ressourcenanbieter-Namespace:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AgFoodPlatform](#microsoftagfoodplatform)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.AnyBuild](#microsoftanybuild)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppAssessment](#microsoftappassessment)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automanage](#microsoftautomanage)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureArcData](#microsoftazurearcdata)
> - [Microsoft.AzureCIS](#microsoftazurecis)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzurePercept](#microsoftazurepercept)
> - [Microsoft.AzureSphere](#microsoftazuresphere)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.BackupSolutions](#microsoftbackupsolutions)
> - [Microsoft.BareMetalInfrastructure](#microsoftbaremetalinfrastructure)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BillingBenefits](#microsoftbillingbenefits)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cascade](#microsoftcascade)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ChangeAnalysis](#microsoftchangeanalysis)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClusterStor](#microsoftclusterstor)
> - [Microsoft.CodeSigning](#microsoftcodesigning)
> - [Microsoft.Codespaces](#microsoftcodespaces)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.ConfidentialLedger](#microsoftconfidentialledger)
> - [Microsoft.ConnectedCache](#microsoftconnectedcache)
> - [Microsoft.ConnectedVehicle](#microsoftconnectedvehicle)
> - [Microsoft.ConnectedVMwarevSphere](#microsoftconnectedvmwarevsphere)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.D365CustomerInsights](#microsoftd365customerinsights)
> - [Microsoft.Dashboard](#microsoftdashboard)
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
> - [Microsoft.DelegatedNetwork](#microsoftdelegatednetwork)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DeviceUpdate](#microsoftdeviceupdate)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.Diagnostics](#microsoftdiagnostics)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EdgeOrder](#microsoftedgeorder)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Fidalgo](#microsoftfidalgo)
> - [Microsoft.FluidRelay](#microsoftfluidrelay)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.Graph](#microsoftgraph)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthBot](#microsofthealthbot)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HpcWorkbench](#microsofthpcworkbench)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridConnectivity](#microsofthybridconnectivity)
> - [Microsoft.HybridContainerService](#microsofthybridcontainerservice)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSecurity](#microsoftiotsecurity)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceNotifications](#microsoftmarketplacenotifications)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.MobileNetwork](#microsoftmobilenetwork)
> - [Microsoft.Monitor](#microsoftmonitor)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.NetworkFunction](#microsoftnetworkfunction)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.Notebooks](#microsoftnotebooks)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PlayFab](#microsoftplayfab)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.PowerPlatform](#microsoftpowerplatform)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Purview](#microsoftpurview)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.Quota](#microsoftquota)
> - [Microsoft.RecommendationsService](#microsoftrecommendationsservice)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceConnector](#microsoftresourceconnector)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scom](#microsoftscom)
> - [Microsoft.ScVmm](#microsoftscvmm)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.ServiceLinker](#microsoftservicelinker)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Singularity](#microsoftsingularity)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TestBase](#microsofttestbase)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VideoIndexer](#microsoftvideoindexer)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DomainServices | Ja |
> | DomainServices/oucontainer | Nein |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | supportProviders | Nein |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | aadsupportcases | Nein |
> | addsservices | Nein |
> | agents | Nein |
> | anonymousapiusers | Nein |
> | Konfiguration | Nein |
> | logs | Nein |
> | reports | Nein |
> | servicehealthmetrics | Nein |
> | services | Nein |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | advisorScore | Nein |
> | Konfigurationen | Nein |
> | generateRecommendations | Nein |
> | metadata | Nein |
> | empfehlungen | Nein |
> | suppressions | Nein |

## <a name="microsoftagfoodplatform"></a>Microsoft.AgFoodPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | farmBeats | Ja |
> | farmBeats/eventGridFilters | Nein |
> | farmBeats/extensions | Nein |
> | farmBeatsExtensionDefinitions | Nein |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | actionRules | Ja |
> | alerts | Nein |
> | alertsList | Nein |
> | alertsMetaData | Nein |
> | alertsSummary | Nein |
> | alertsSummaryList | Nein |
> | migrateFromSmartDetection | Nein |
> | resourceHealthAlertRules | Ja |
> | smartDetectorAlertRules | Ja |
> | smartGroups | Nein |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja |

## <a name="microsoftanybuild"></a>Microsoft.AnyBuild

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Nein |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deletedServices | Nein |
> | getDomainOwnershipIdentifier | Nein |
> | reportFeedback | Nein |
> | Dienst | Ja |
> | service/eventGridFilters | Nein |
> | validateServiceName | Nein |

## <a name="microsoftappassessment"></a>Microsoft.AppAssessment

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | migrateProjects | Nein |
> | migrateProjects/assessments | Nein |
> | migrateProjects/assessments/assessedApplications | Nein |
> | migrateProjects/assessments/assessedApplications/machines | Nein |
> | migrateProjects/assessments/assessedMachines | Nein |
> | migrateProjects/assessments/assessedMachines/applications | Nein |
> | migrateProjects/assessments/machinesToAssess | Nein |
> | migrateProjects/sites | Nein |
> | migrateProjects/sites/applianceConfigurations | Nein |
> | migrateProjects/sites/machines | Nein |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | configurationStores | Ja |
> | configurationStores/eventGridFilters | Nein |
> | configurationStores/keyValues | Nein |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Spring | Ja |
> | Spring/apps | Nein |
> | Spring/apps/deployments | Nein |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | attestationProviders | Ja |
> | defaultProviders | Nein |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accessReviewScheduleDefinitions | Nein |
> | accessReviewScheduleSettings | Nein |
> | batchResourceCheckAccess | Nein |
> | classicAdministrators | Nein |
> | dataAliases | Nein |
> | dataPolicyManifests | Nein |
> | denyAssignments | Nein |
> | diagnosticSettings | Nein |
> | diagnosticSettingsCategories | Nein |
> | elevateAccess | Nein |
> | eligibleChildResources | Nein |
> | findOrphanRoleAssignments | Nein |
> | locks | Nein |
> | Berechtigungen | Nein |
> | policyAssignments | Nein |
> | policyDefinitions | Nein |
> | policyExemptions | Nein |
> | policySetDefinitions | Nein |
> | privateLinkAssociations | Nein |
> | providerOperations | Nein |
> | resourceManagementPrivateLinks | Ja |
> | roleAssignmentApprovals | Nein |
> | roleAssignments | Nein |
> | roleAssignmentScheduleInstances | Nein |
> | roleAssignmentScheduleRequests | Nein |
> | roleAssignmentSchedules | Nein |
> | roleAssignmentsUsageMetrics | Nein |
> | roleDefinitions | Nein |
> | roleEligibilityScheduleInstances | Nein |
> | roleEligibilityScheduleRequests | Nein |
> | roleEligibilitySchedules | Nein |
> | roleManagementPolicies | Nein |
> | roleManagementPolicyAssignments | Nein |

## <a name="microsoftautomanage"></a>Microsoft.Automanage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | bestPractices | Nein |
> | bestPractices/versions | Nein |
> | configurationProfileAssignmentIntents | Nein |
> | configurationProfileAssignments | Nein |
> | configurationProfilePreferences | Ja |
> | configurationProfiles | Ja |
> | configurationProfiles/versions | Ja |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | automationAccounts | Ja |
> | automationAccounts/configurations | Ja |
> | automationAccounts/hybridRunbookWorkerGroups | Nein |
> | automationAccounts/hybridRunbookWorkerGroups/hybridRunbookWorkers | Nein |
> | automationAccounts/jobs | Nein |
> | automationAccounts/privateEndpointConnectionProxies | Nein |
> | automationAccounts/privateEndpointConnections | Nein |
> | automationAccounts/privateLinkResources | Nein |
> | automationAccounts/runbooks | Ja |
> | automationAccounts/softwareUpdateConfigurations | Nein |
> | automationAccounts/webhooks | Nein |

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | privateClouds | Ja |
> | privateClouds/addons | Nein |
> | privateClouds/authorizations | Nein |
> | privateClouds/cloudLinks | Nein |
> | privateClouds/clusters | Nein |
> | privateClouds/clusters/datastores | Nein |
> | privateClouds/clusters/placementPolicies | Nein |
> | privateClouds/clusters/virtualMachines | Nein |
> | privateClouds/globalReachConnections | Nein |
> | privateClouds/hcxEnterpriseSites | Nein |
> | privateClouds/scriptExecutions | Nein |
> | privateClouds/scriptPackages | Nein |
> | privateClouds/scriptPackages/scriptCmdlets | Nein |
> | privateClouds/workloadNetworks | Nein |
> | privateClouds/workloadNetworks/dhcpConfigurations | Nein |
> | privateClouds/workloadNetworks/dnsServices | Nein |
> | privateClouds/workloadNetworks/dnsZones | Nein |
> | privateClouds/workloadNetworks/gateways | Nein |
> | privateClouds/workloadNetworks/portMirroringProfiles | Nein |
> | privateClouds/workloadNetworks/publicIPs | Nein |
> | privateClouds/workloadNetworks/segments | Nein |
> | privateClouds/workloadNetworks/virtualMachines | Nein |
> | privateClouds/workloadNetworks/vmGroups | Nein |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | environments | Nein |
> | environments/accounts | Nein |
> | environments/accounts/namespaces | Nein |
> | environments/accounts/namespaces/configurations | Nein |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | b2cDirectories | Ja |
> | b2ctenants | Nein |
> | guestUsages | Ja |

## <a name="microsoftazurearcdata"></a>Microsoft.AzureArcData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DataControllers | Nein |
> | DataWarehouseInstances | Nein |
> | PostgresInstances | Nein |
> | SqlManagedInstances | Nein |
> | SqlServerInstances | Nein |

## <a name="microsoftazurecis"></a>Microsoft.AzureCIS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | autopilotEnvironments | Nein |
> | dstsServiceAccounts | Nein |
> | dstsServiceClientIdentities | Nein |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | sqlServerRegistrations | Ja |
> | sqlServerRegistrations/sqlServers | Nein |

## <a name="microsoftazurepercept"></a>Microsoft.AzurePercept

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Nein |
> | accounts/devices | Nein |

## <a name="microsoftazuresphere"></a>Microsoft.AzureSphere

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | catalogs | Nein |
> | catalogs/certificates | Nein |
> | catalogs/deployments | Nein |
> | catalogs/devices | Nein |
> | catalogs/images | Nein |
> | catalogs/products | Nein |
> | catalogs/products/devicegroups | Nein |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | cloudManifestFiles | Nein |
> | linkedSubscriptions | Ja |
> | registrations | Ja |
> | registrations/customerSubscriptions | Nein |
> | registrations/products | Nein |

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Nein |
> | clusters/arcsettings | Nein |
> | clusters/arcsettings/extensions | Nein |
> | galleryImages | Nein |
> | networkInterfaces | Nein |
> | virtualHardDisks | Nein |
> | virtualmachines | Nein |
> | virtualmachines/extensions | Nein |
> | virtualmachines/hybrididentitymetadata | Nein |
> | virtualNetworks | Nein |

## <a name="microsoftbackupsolutions"></a>Microsoft.BackupSolutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | VMwareApplications | Ja |

## <a name="microsoftbaremetalinfrastructure"></a>Microsoft.BareMetalInfrastructure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | bareMetalInstances | Ja |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | batchAccounts | Ja |
> | batchAccounts/certificates | Nein |
> | batchAccounts/pools | Nein |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | billingAccounts | Nein |
> | billingAccounts/agreements | Nein |
> | billingAccounts/appliedReservationOrders | Nein |
> | billingAccounts/billingPermissions | Nein |
> | billingAccounts/billingProfiles | Nein |
> | billingAccounts/billingProfiles/billingPermissions | Nein |
> | billingAccounts/billingProfiles/billingRoleAssignments | Nein |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Nein |
> | billingAccounts/billingProfiles/billingSubscriptions | Nein |
> | billingAccounts/billingProfiles/createBillingRoleAssignment | Nein |
> | billingAccounts/billingProfiles/customers | Nein |
> | billingAccounts/billingProfiles/instructions | Nein |
> | billingAccounts/billingProfiles/invoices | Nein |
> | billingAccounts/billingProfiles/invoices/pricesheet | Nein |
> | billingAccounts/billingProfiles/invoices/transactions | Nein |
> | billingAccounts/billingProfiles/invoiceSections | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingPermissions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleAssignments | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleDefinitions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingSubscriptions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/createBillingRoleAssignment | Nein |
> | billingAccounts/billingProfiles/invoiceSections/initiateTransfer | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/transfer | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/updateAutoRenew | Nein |
> | billingAccounts/billingProfiles/invoiceSections/transactions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/transfers | Nein |
> | billingAccounts/billingProfiles/invoiceSections/validateDeleteInvoiceSectionEligibility | Nein |
> | billingAccounts/BillingProfiles/patchOperations | Nein |
> | billingAccounts/billingProfiles/paymentMethodLinks | Nein |
> | billingAccounts/billingProfiles/paymentMethods | Nein |
> | billingAccounts/billingProfiles/policies | Nein |
> | billingAccounts/billingProfiles/pricesheet | Nein |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Nein |
> | billingAccounts/billingProfiles/products | Nein |
> | billingAccounts/billingProfiles/reservations | Nein |
> | billingAccounts/billingProfiles/transactions | Nein |
> | billingAccounts/billingProfiles/validateDeleteBillingProfileEligibility | Nein |
> | billingAccounts/billingProfiles/validateDetachPaymentMethodEligibility | Nein |
> | billingAccounts/billingRoleAssignments | Nein |
> | billingAccounts/billingRoleDefinitions | Nein |
> | billingAccounts/billingSubscriptions | Nein |
> | billingAccounts/billingSubscriptions/elevateRole | Nein |
> | billingAccounts/billingSubscriptions/invoices | Nein |
> | billingAccounts/createBillingRoleAssignment | Nein |
> | billingAccounts/createInvoiceSectionOperations | Nein |
> | billingAccounts/customers | Nein |
> | billingAccounts/customers/billingPermissions | Nein |
> | billingAccounts/customers/billingSubscriptions | Nein |
> | billingAccounts/customers/initiateTransfer | Nein |
> | billingAccounts/customers/policies | Nein |
> | billingAccounts/customers/products | Nein |
> | billingAccounts/customers/transactions | Nein |
> | billingAccounts/customers/transfers | Nein |
> | billingAccounts/customers/transferSupportedAccounts | Nein |
> | billingAccounts/departments | Nein |
> | billingAccounts/Departments/billingPermissions | Nein |
> | billingAccounts/departments/billingRoleAssignments | Nein |
> | billingAccounts/departments/billingRoleDefinitions | Nein |
> | billingAccounts/departments/billingSubscriptions | Nein |
> | billingAccounts/departments/enrollmentAccounts | Nein |
> | billingAccounts/enrollmentAccounts | Nein |
> | billingAccounts/enrollmentAccounts/billingPermissions | Nein |
> | billingAccounts/enrollmentAccounts/billingRoleAssignments | Nein |
> | billingAccounts/enrollmentAccounts/billingRoleDefinitions | Nein |
> | billingAccounts/enrollmentAccounts/billingSubscriptions | Nein |
> | billingAccounts/invoices | Nein |
> | billingAccounts/invoices/transactions | Nein |
> | billingAccounts/invoices/transactionSummary | Nein |
> | billingAccounts/invoices | Nein |
> | billingAccounts/invoiceSections/billingSubscriptionMoveOperations | Nein |
> | billingAccounts / invoiceSections/billingSubscriptions | Nein |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Nein |
> | billingAccounts/invoiceSections/elevate | Nein |
> | billingAccounts/invoiceSections/initiateTransfer | Nein |
> | billingAccounts/invoiceSections/patchOperations | Nein |
> | billingAccounts/invoiceSections/productMoveOperations | Nein |
> | billingAccounts/invoiceSections/products | Nein |
> | billingAccounts/invoiceSections/products/transfer | Nein |
> | billingAccounts/invoiceSections/products/updateAutoRenew | Nein |
> | billingAccounts/invoiceSections/transactions | Nein |
> | billingAccounts/invoiceSections/transfers | Nein |
> | billingAccounts/lineOfCredit | Nein |
> | billingAccounts/patchOperations | Nein |
> | billingAccounts/payableOverage | Nein |
> | billingAccounts/paymentMethods | Nein |
> | billingAccounts/payNow | Nein |
> | billingAccounts/policies | Nein |
> | billingAccounts/products | Nein |
> | billingAccounts/promotionalCredits | Nein |
> | billingAccounts/reservations | Nein |
> | billingAccounts/savingsPlanOrders | Nein |
> | billingAccounts/savingsPlanOrders/savingsPlans | Nein |
> | billingAccounts/savingsPlans | Nein |
> | billingAccounts/transactions | Nein |
> | billingPeriods | Nein |
> | billingPermissions | Nein |
> | billingProperty | Nein |
> | billingRoleAssignments | Nein |
> | billingRoleDefinitions | Nein |
> | createBillingRoleAssignment | Nein |
> | departments | Nein |
> | enrollmentAccounts | Nein |
> | invoices | Nein |
> | paymentMethods | Nein |
> | promotionalCredits | Nein |
> | Erweiterungen | Nein |
> | transfers | Nein |
> | transfers/acceptTransfer | Nein |
> | transfers/declineTransfer | Nein |
> | transfers/operationStatus | Nein |
> | transfers/validateTransfer | Nein |
> | validateAddress | Nein |

## <a name="microsoftbillingbenefits"></a>Microsoft.BillingBenefits

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | savingsPlanOrderAliases | Nein |
> | validate | Nein |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | blockchainMembers | Ja |
> | cordaMembers | Ja |
> | watchers | Ja |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | TokenServices | Ja |
> | TokenServices/BlockchainNetworks | Nein |
> | TokenServices/Groups | Nein |
> | TokenServices/Groups/Accounts | Nein |
> | TokenServices/TokenTemplates | Nein |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | blueprintAssignments | Nein |
> | blueprintAssignments/assignmentOperations | Nein |
> | blueprintAssignments/operations | Nein |
> | blueprints | Nein |
> | blueprints/artifacts | Nein |
> | blueprints/versions | Nein |
> | blueprints/versions/artifacts | Nein |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | botServices | Ja |
> | botServices/channels | Nein |
> | botServices/connections | Nein |
> | botServices/privateEndpointConnectionProxies | Nein |
> | botServices/privateEndpointConnections | Nein |
> | botServices/privateLinkResources | Nein |
> | hostSettings | Nein |
> | languages | Nein |
> | Vorlagen | Nein |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Redis | Ja |
> | Redis/EventGridFilters | Nein |
> | Redis/privateEndpointConnectionProxies | Nein |
> | Redis/privateEndpointConnectionProxies/validate | Nein |
> | Redis/privateEndpointConnections | Nein |
> | Redis/privateLinkResources | Nein |
> | redisEnterprise | Ja |
> | redisEnterprise/databases | Nein |
> | RedisEnterprise/privateEndpointConnectionProxies | Nein |
> | RedisEnterprise/privateEndpointConnectionProxies/validate | Nein |
> | RedisEnterprise/privateEndpointConnections | Nein |
> | RedisEnterprise/privateLinkResources | Nein |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | appliedReservations | Nein |
> | autoQuotaIncrease | Nein |
> | calculateExchange | Nein |
> | calculatePrice | Nein |
> | calculatePurchasePrice | Nein |
> | catalogs | Nein |
> | commercialReservationOrders | Nein |
> | Börse | Nein |
> | ownReservations | Nein |
> | placePurchaseOrder | Nein |
> | reservationOrders | Nein |
> | reservationOrders/calculateRefund | Nein |
> | reservationOrders/merge | Nein |
> | reservationOrders/reservations | Nein |
> | reservationOrders/reservations/revisions | Nein |
> | reservationOrders/return | Nein |
> | reservationOrders/split | Nein |
> | reservationOrders/swap | Nein |
> | reservations | Nein |
> | resourceProviders | Nein |
> | ressourcen | Nein |
> | validateReservationOrder | Nein |

## <a name="microsoftcascade"></a>Microsoft.Cascade

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | sites | Nein |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Nein |
> | CdnWebApplicationFirewallPolicies | Ja |
> | edgenodes | Nein |
> | profiles | Ja |
> | profiles/afdendpoints | Ja |
> | profiles/afdendpoints/routes | Nein |
> | profiles/customdomains | Nein |
> | profiles/endpoints | Ja |
> | profiles/endpoints/customdomains | Nein |
> | profiles/endpoints/origingroups | Nein |
> | profiles/endpoints/origins | Nein |
> | profiles/origingroups | Nein |
> | profiles/origingroups/origins | Nein |
> | profiles/rulesets | Nein |
> | profiles/rulesets/rules | Nein |
> | profiles/secrets | Nein |
> | profiles/securitypolicies | Nein |
> | validateProbe | Nein |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | certificateOrders | Ja |
> | certificateOrders/certificates | Nein |
> | validateCertificateRegistrationInformation | Nein |

## <a name="microsoftchangeanalysis"></a>Microsoft.ChangeAnalysis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Änderungen | Nein |
> | changeSnapshots | Nein |
> | computeChanges | Nein |
> | profile | Nein |
> | resourceChanges | Nein |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein |
> | domainNames | Ja |
> | domainNames/capabilities | Nein |
> | domainNames/internalLoadBalancers | Nein |
> | domainNames/serviceCertificates | Nein |
> | domainNames/slots | Nein |
> | domainNames/slots/roles | Nein |
> | domainNames/slots/roles/metricDefinitions | Nein |
> | domainNames/slots/roles/metrics | Nein |
> | moveSubscriptionResources | Nein |
> | operatingSystemFamilies | Nein |
> | operatingSystems | Nein |
> | quotas | Nein |
> | resourceTypes | Nein |
> | validateSubscriptionMoveAvailability | Nein |
> | virtualMachines | Ja |
> | virtualMachines/diagnosticSettings | Nein |
> | virtualMachines/metricDefinitions | Nein |
> | virtualMachines/metrics | Nein |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | classicInfrastructureResources | Nein |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein |
> | expressRouteCrossConnections | Nein |
> | expressRouteCrossConnections/peerings | Nein |
> | gatewaySupportedDevices | Nein |
> | networkSecurityGroups | Ja |
> | quotas | Nein |
> | reservedIps | Ja |
> | virtualNetworks | Ja |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein |
> | virtualNetworks/virtualNetworkPeerings | Nein |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein |
> | disks | Nein |
> | images | Nein |
> | osImages | Nein |
> | osPlatformImages | Nein |
> | publicImages | Nein |
> | quotas | Nein |
> | storageAccounts | Ja |
> | storageAccounts/blobServices | Nein |
> | storageAccounts/fileServices | Nein |
> | storageAccounts/metricDefinitions | Nein |
> | storageAccounts/metrics | Nein |
> | storageAccounts/queueServices | Nein |
> | storageAccounts/services | Nein |
> | storageAccounts/services/diagnosticSettings | Nein |
> | storageAccounts/services/metricDefinitions | Nein |
> | storageAccounts/services/metrics | Nein |
> | storageAccounts/tableServices | Nein |
> | storageAccounts/vmImages | Nein |
> | vmImages | Nein |

## <a name="microsoftclusterstor"></a>Microsoft.ClusterStor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | nodes | Ja |

## <a name="microsoftcodesigning"></a>Microsoft.CodeSigning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | codeSigningAccounts | Nein |
> | codeSigningAccounts/certificateProfiles | Nein |

## <a name="microsoftcodespaces"></a>Microsoft.Codespaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | plans | Ja |
> | registeredSubscriptions | Nein |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/privateEndpointConnectionProxies | Nein |
> | accounts/privateEndpointConnections | Nein |
> | accounts/privateLinkResources | Nein |
> | deletedAccounts | Nein |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | availabilitySets | Ja |
> | cloudServices | Ja |
> | cloudServices/networkInterfaces | Nein |
> | cloudServices/publicIPAddresses | Nein |
> | cloudServices/roleInstances | Nein |
> | cloudServices/roleInstances/networkInterfaces | Nein |
> | cloudServices/roles | Nein |
> | diskAccesses | Ja |
> | diskEncryptionSets | Ja |
> | disks | Ja |
> | galleries | Ja |
> | galleries/applications | Nein |
> | galleries/applications/versions | Nein |
> | galleries/images | Ja |
> | galleries/images/versions | Ja |
> | hostGroups | Ja |
> | hostGroups/hosts | Ja |
> | images | Ja |
> | proximityPlacementGroups | Ja |
> | restorePointCollections | Ja |
> | restorePointCollections/restorePoints | Nein |
> | sharedVMExtensions | Ja |
> | sharedVMExtensions/versions | Nein |
> | sharedVMImages | Ja |
> | sharedVMImages/versions | Nein |
> | snapshots | Ja |
> | sshPublicKeys | Ja |
> | virtualMachines | Ja |
> | virtualMachines/extensions | Ja |
> | virtualMachines/metricDefinitions | Nein |
> | virtualMachines/runCommands | Ja |
> | virtualMachineScaleSets | Ja |
> | virtualMachineScaleSets/extensions | Nein |
> | virtualMachineScaleSets/networkInterfaces | Nein |
> | virtualMachineScaleSets/publicIPAddresses | Nein |
> | virtualMachineScaleSets/virtualMachines | Nein |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | RateCard | Nein |
> | UsageAggregates | Nein |

## <a name="microsoftconfidentialledger"></a>Microsoft.ConfidentialLedger

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Ledgers | Nein |

## <a name="microsoftconnectedcache"></a>Microsoft.ConnectedCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | CacheNodes | Nein |

## <a name="microsoftconnectedvehicle"></a>Microsoft.ConnectedVehicle

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | platformAccounts | Nein |
> | registeredSubscriptions | Nein |

## <a name="microsoftconnectedvmwarevsphere"></a>Microsoft.ConnectedVMwarevSphere

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Cluster | Nein |
> | Datenspeicher | Nein |
> | Hosts | Nein |
> | ResourcePools | Nein |
> | VCenters | Nein |
> | VCenters/InventoryItems | Nein |
> | VirtualMachines | Nein |
> | VirtualMachines/Extensions | Ja |
> | VirtualMachines/GuestAgents | Nein |
> | VirtualMachines/HybridIdentityMetadata | Nein |
> | VirtualMachineTemplates | Nein |
> | VirtualNetworks | Nein |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | AggregatedCost | Nein |
> | Bilanzen | Nein |
> | Budgets | Nein |
> | Charges | Nein |
> | CostTags | Nein |
> | credits | Nein |
> | events | Nein |
> | Vorhersagen | Nein |
> | lots | Nein |
> | Marketplaces | Nein |
> | Pricesheets | Nein |
> | products | Nein |
> | ReservationDetails | Nein |
> | ReservationRecommendationDetails | Nein |
> | ReservationRecommendations | Nein |
> | ReservationSummaries | Nein |
> | ReservationTransactions | Nein |
> | `Tags` | Nein |
> | tenants | Nein |
> | Begriffe | Nein |
> | UsageDetails | Nein |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | containerGroups | Ja |
> | serviceAssociationLinks | Nein |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | registries | Ja |
> | registries/agentPools | Ja |
> | registries/builds | Nein |
> | registries/builds/cancel | Nein |
> | registries/builds/getLogLink | Nein |
> | registries/buildTasks | Ja |
> | registries/buildTasks/steps | Nein |
> | registries/connectedRegistries | Nein |
> | registries/connectedRegistries/deactivate | Nein |
> | registries/eventGridFilters | Nein |
> | registries/exportPipelines | Nein |
> | registries/generateCredentials | Nein |
> | registries/getBuildSourceUploadUrl | Nein |
> | registries/GetCredentials | Nein |
> | registries/importImage | Nein |
> | registries/importPipelines | Nein |
> | registries/pipelineRuns | Nein |
> | registries/privateEndpointConnectionProxies | Nein |
> | registries/privateEndpointConnectionProxies/validate | Nein |
> | registries/privateEndpointConnections | Nein |
> | registries/privateLinkResources | Nein |
> | registries/queueBuild | Nein |
> | registries/regenerateCredential | Nein |
> | registries/regenerateCredentials | Nein |
> | registries/replications | Ja |
> | registries/runs | Nein |
> | registries/runs/cancel | Nein |
> | registries/scheduleRun | Nein |
> | registries/scopeMaps | Nein |
> | registries/taskRuns | Nein |
> | registries/tasks | Ja |
> | registries/tokens | Nein |
> | registries/updatePolicies | Nein |
> | registries/webhooks | Ja |
> | registries/webhooks/getCallbackConfig | Nein |
> | registries/webhooks/ping | Nein |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | containerServices | Ja |
> | managedClusters | Ja |
> | ManagedClusters/eventGridFilters | Nein |
> | openShiftManagedClusters | Ja |
> | snapshots | Ja |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Alerts | Nein |
> | BenefitUtilizationSummaries | Nein |
> | BillingAccounts | Nein |
> | Budgets | Nein |
> | calculatePrice | Nein |
> | CloudConnectors | Nein |
> | Connectors | Ja |
> | costAllocationRules | Nein |
> | Departments | Nein |
> | Dimensionen | Nein |
> | EnrollmentAccounts | Nein |
> | Exports | Nein |
> | ExternalBillingAccounts | Nein |
> | ExternalBillingAccounts/Alerts | Nein |
> | ExternalBillingAccounts/Dimensions | Nein |
> | ExternalBillingAccounts/Forecast | Nein |
> | ExternalBillingAccounts/Query | Nein |
> | ExternalSubscriptions | Nein |
> | ExternalSubscriptions/Alerts | Nein |
> | ExternalSubscriptions/Dimensions | Nein |
> | ExternalSubscriptions/Forecast | Nein |
> | ExternalSubscriptions/Query | Nein |
> | fetchPrices | Nein |
> | Forecast | Nein |
> | GenerateDetailedCostReport | Nein |
> | GenerateReservationDetailsReport | Nein |
> | Einblicke | Nein |
> | Abfrage | Nein |
> | Registrieren | Nein |
> | Reportconfigs | Nein |
> | Berichte | Nein |
> | ScheduledActions | Nein |
> | Einstellungen | Nein |
> | showbackRules | Nein |
> | Sichten | Nein |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DisableLockbox | Nein |
> | EnableLockbox | Nein |
> | requests | Nein |
> | TenantOptedIn | Nein |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | associations | Nein |
> | resourceProviders | Ja |

## <a name="microsoftd365customerinsights"></a>Microsoft.D365CustomerInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | instances | Ja |

## <a name="microsoftdashboard"></a>Microsoft.Dashboard

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | grafana | Nein |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | jobs | Ja |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Ja |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | workspaces | Ja |
> | workspaces/dbWorkspaces | Nein |
> | workspaces/virtualNetworkPeerings | Nein |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | catalogs | Ja |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dataFactories | Ja |
> | dataFactories/diagnosticSettings | Nein |
> | dataFactories/metricDefinitions | Nein |
> | dataFactorySchema | Nein |
> | factories | Ja |
> | factories/integrationRuntimes | Nein |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/dataLakeStoreAccounts | Nein |
> | accounts/storageAccounts | Nein |
> | accounts/storageAccounts/containers | Nein |
> | accounts/transferAnalyticsUnits | Nein |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/eventGridFilters | Nein |
> | accounts/firewallRules | Nein |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DatabaseMigrations | Nein |
> | services | Ja |
> | services/projects | Ja |
> | SqlMigrationServices | Ja |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | BackupVaults | Ja |
> | ResourceGuards | Ja |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/shares | Nein |
> | accounts/shares/datasets | Nein |
> | accounts/shares/invitations | Nein |
> | accounts/shares/providersharesubscriptions | Nein |
> | accounts/shares/synchronizationSettings | Nein |
> | accounts/sharesubscriptions | Nein |
> | accounts/sharesubscriptions/consumerSourceDataSets | Nein |
> | accounts/sharesubscriptions/datasetmappings | Nein |
> | accounts/sharesubscriptions/triggers | Nein |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja |
> | servers/advisors | Nein |
> | servers/keys | Nein |
> | servers/privateEndpointConnectionProxies | Nein |
> | servers/privateEndpointConnections | Nein |
> | servers/privateLinkResources | Nein |
> | servers/queryTexts | Nein |
> | servers/recoverableServers | Nein |
> | servers/resetQueryPerformanceInsightData | Nein |
> | servers/start | Nein |
> | servers/stop | Nein |
> | servers/topQueryStatistics | Nein |
> | servers/virtualNetworkRules | Nein |
> | servers/waitStatistics | Nein |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | flexibleServers | Ja |
> | getPrivateDnsZoneSuffix | Nein |
> | servers | Ja |
> | servers/advisors | Nein |
> | servers/keys | Nein |
> | servers/privateEndpointConnectionProxies | Nein |
> | servers/privateEndpointConnections | Nein |
> | servers/privateLinkResources | Nein |
> | servers/queryTexts | Nein |
> | servers/recoverableServers | Nein |
> | servers/resetQueryPerformanceInsightData | Nein |
> | servers/start | Nein |
> | servers/stop | Nein |
> | servers/topQueryStatistics | Nein |
> | servers/upgrade | Nein |
> | servers/virtualNetworkRules | Nein |
> | servers/waitStatistics | Nein |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | flexibleServers | Ja |
> | getPrivateDnsZoneSuffix | Nein |
> | serverGroups | Ja |
> | serverGroupsv2 | Ja |
> | servers | Ja |
> | servers/advisors | Nein |
> | servers/keys | Nein |
> | servers/privateEndpointConnectionProxies | Nein |
> | servers/privateEndpointConnections | Nein |
> | servers/privateLinkResources | Nein |
> | servers/queryTexts | Nein |
> | servers/recoverableServers | Nein |
> | servers/resetQueryPerformanceInsightData | Nein |
> | servers/topQueryStatistics | Nein |
> | servers/virtualNetworkRules | Nein |
> | servers/waitStatistics | Nein |
> | serversv2 | Ja |

## <a name="microsoftdelegatednetwork"></a>Microsoft.DelegatedNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Controller | Ja |
> | delegatedSubnets | Ja |
> | orchestrators | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | artifactSources | Ja |
> | rollouts | Ja |
> | serviceTopologies | Ja |
> | serviceTopologies/services | Ja |
> | serviceTopologies/services/serviceUnits | Ja |
> | steps | Ja |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationgroups | Ja |
> | applicationgroups/applications | Nein |
> | applicationgroups/desktops | Nein |
> | applicationgroups/startmenuitems | Nein |
> | hostpools | Ja |
> | hostpools/msixpackages | Nein |
> | hostpools/sessionhosts | Nein |
> | hostpools/sessionhosts/usersessions | Nein |
> | hostpools/usersessions | Nein |
> | scalingplans | Ja |
> | workspaces | Ja |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | ElasticPools | Ja |
> | ElasticPools/IotHubTenants | Ja |
> | ElasticPools/IotHubTenants/securitySettings | Nein |
> | IotHubs | Ja |
> | IotHubs/eventGridFilters | Nein |
> | IotHubs/failover | Nein |
> | IotHubs/securitySettings | Nein |
> | ProvisioningServices | Ja |
> | usages | Nein |

## <a name="microsoftdeviceupdate"></a>Microsoft.DeviceUpdate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Nein |
> | accounts/instances | Nein |
> | registeredSubscriptions | Nein |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | pipelines | Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Controller | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labcenters | Ja |
> | labs | Ja |
> | labs/environments | Ja |
> | labs/serviceRunners | Ja |
> | labs/virtualMachines | Ja |
> | schedules | Ja |

## <a name="microsoftdiagnostics"></a>Microsoft.Diagnostics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | AzureKB | Nein |
> | InsightDiagnostics | Nein |
> | solutions | Nein |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | digitalTwinsInstances | Ja |
> | digitalTwinsInstances/endpoints | Nein |
> | digitalTwinsInstances/timeSeriesDatabaseConnections | Nein |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | cassandraClusters | Ja |
> | databaseAccountNames | Nein |
> | databaseAccounts | Ja |
> | restorableDatabaseAccounts | Nein |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | domains | Ja |
> | domains/domainOwnershipIdentifiers | Nein |
> | generateSsoRequest | Nein |
> | topLevelDomains | Nein |
> | validateDomainRegistrationInformation | Nein |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | lcsprojects | Nein |
> | lcsprojects/clouddeployments | Nein |
> | lcsprojects/connectors | Nein |

## <a name="microsoftedgeorder"></a>Microsoft.EdgeOrder

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Adressen | Ja |
> | orderItems | Ja |
> | Aufträge | Nein |
> | productFamiliesMetadata | Nein |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | services | Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | domains | Ja |
> | domains/topics | Nein |
> | eventSubscriptions | Nein |
> | extensionTopics | Nein |
> | partnerNamespaces | Ja |
> | partnerNamespaces/eventChannels | Nein |
> | partnerRegistrations | Ja |
> | partnerTopics | Ja |
> | partnerTopics/eventSubscriptions | Nein |
> | systemTopics | Ja |
> | systemTopics/eventSubscriptions | Nein |
> | topics | Ja |
> | topicTypes | Nein |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | Namespaces | Ja |
> | namespaces/authorizationrules | Nein |
> | namespaces/disasterrecoveryconfigs | Nein |
> | namespaces/eventhubs | Nein |
> | namespaces/eventhubs/authorizationrules | Nein |
> | namespaces/eventhubs/consumergroups | Nein |
> | namespaces/networkrulesets | Nein |
> | namespaces/privateEndpointConnections | Nein |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | experimentWorkspaces | Ja |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Namespaces | Ja |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | featureConfigurations | Nein |
> | featureProviderNamespaces | Nein |
> | featureProviders | Nein |
> | Features | Nein |
> | providers | Nein |
> | subscriptionFeatureRegistrations | Nein |

## <a name="microsoftfidalgo"></a>Microsoft.Fidalgo

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | devcenters | Nein |
> | devcenters/catalogs | Nein |
> | devcenters/catalogs/items | Nein |
> | devcenters/environmentTypes | Nein |
> | devcenters/mappings | Nein |
> | machinedefinitions | Nein |
> | networksettings | Nein |
> | projects | Nein |
> | projects/catalogItems | Nein |
> | projects/environments | Nein |
> | projects/environments/deployments | Nein |
> | projects/environmentTypes | Nein |
> | projects/pools | Nein |

## <a name="microsoftfluidrelay"></a>Microsoft.FluidRelay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | fluidRelayServers | Nein |
> | fluidRelayServers/fluidRelayContainers | Nein |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | enroll | Nein |
> | galleryitems | Nein |
> | generateartifactaccessuri | Nein |
> | myareas | Nein |
> | myareas/areas | Nein |
> | myareas/areas/areas | Nein |
> | myareas/areas/areas/galleryitems | Nein |
> | myareas/areas/galleryitems | Nein |
> | myareas/galleryitems | Nein |
> | Registrieren | Nein |
> | ressourcen | Nein |
> | retrieveresourcesbyid | Nein |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |

## <a name="microsoftgraph"></a>Microsoft.Graph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | AzureAdApplication | Nein |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | autoManagedAccounts | Ja |
> | autoManagedVmConfigurationProfiles | Ja |
> | configurationProfileAssignments | Nein |
> | guestConfigurationAssignments | Nein |
> | software | Nein |
> | softwareUpdateProfile | Nein |
> | softwareUpdates | Nein |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hanaInstances | Ja |
> | sapMonitors | Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dedicatedHSMs | Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusterPools | Ja |
> | clusterPools/clusters | Ja |
> | clusterPools/clusters/instanceViews | Nein |
> | clusterPools/clusters/serviceConfigs | Nein |
> | clusterPools/clusters/sessionClusters | Ja |
> | clusterPools/clusters/sessionClusters/instanceViews | Nein |
> | clusterPools/clusters/sessionClusters/serviceConfigs | Nein |
> | clusters | Ja |
> | clusters/applications | Nein |

## <a name="microsofthealthbot"></a>Microsoft.HealthBot

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | healthBots | Nein |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | services | Ja |
> | services/iomtconnectors | Nein |
> | services/iomtconnectors/connections | Nein |
> | services/iomtconnectors/mappings | Nein |
> | services/privateEndpointConnectionProxies | Nein |
> | services/privateEndpointConnections | Nein |
> | services/privateLinkResources | Nein |
> | workspaces | Ja |
> | workspaces/dicomservices | Ja |
> | workspaces/eventGridFilters | Nein |
> | workspaces/fhirservices | Ja |
> | workspaces/iotconnectors | Ja |
> | workspaces/iotconnectors/destinations | Nein |
> | workspaces/iotconnectors/fhirdestinations | Nein |
> | workspaces/privateEndpointConnectionProxies | Nein |
> | workspaces/privateEndpointConnections | Nein |
> | workspaces/privateLinkResources | Nein |

## <a name="microsofthpcworkbench"></a>Microsoft.HpcWorkbench

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | instances | Nein |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | machines | Ja |
> | machines/assessPatches | Nein |
> | machines/extensions | Ja |
> | machines/installPatches | Nein |
> | machines/privateLinkScopes | Nein |
> | privateLinkScopes | Ja |
> | privateLinkScopes/privateEndpointConnectionProxies | Nein |
> | privateLinkScopes/privateEndpointConnections | Nein |

## <a name="microsofthybridconnectivity"></a>Microsoft.HybridConnectivity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | -Endpunkte | Nein |

## <a name="microsofthybridcontainerservice"></a>Microsoft.HybridContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | provisionedClusters | Nein |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dataManagers | Ja |

## <a name="microsofthybridnetwork"></a>Microsoft.HybridNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | devices | Nein |
> | networkFunctions | Nein |
> | networkFunctionVendors | Nein |
> | registeredSubscriptions | Nein |
> | vendors | Nein |
> | vendors/vendorSkus | Nein |
> | vendors/vendorskus/previewSubscriptions | Nein |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | components | Ja |
> | networkScopes | Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | jobs | Ja |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein |
> | diagnosticSettingsCategories | Nein |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | appTemplates | Nein |
> | IoTApps | Ja |

## <a name="microsoftiotsecurity"></a>Microsoft.IoTSecurity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | alertTypes | Nein |
> | defenderSettings | Nein |
> | onPremiseSensors | Nein |
> | recommendationTypes | Nein |
> | sensors | Nein |
> | sites | Nein |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Graph | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deletedManagedHSMs | Nein |
> | deletedVaults | Nein |
> | hsmPools | Ja |
> | managedHSMs | Ja |
> | vaults | Ja |
> | vaults/accessPolicies | Nein |
> | vaults/eventGridFilters | Nein |
> | vaults/keys | Nein |
> | vaults/keys/versions | Nein |
> | vaults/secrets | Nein |

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | connectedClusters | Nein |
> | registeredSubscriptions | Nein |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Erweiterungen | Nein |
> | fluxConfigurations | Nein |
> | sourceControlConfigurations | Nein |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | clusters/attacheddatabaseconfigurations | Nein |
> | clusters/databases | Nein |
> | clusters/databases/dataconnections | Nein |
> | clusters/databases/eventhubconnections | Nein |
> | clusters/databases/principalassignments | Nein |
> | clusters/databases/scripts | Nein |
> | clusters/dataconnections | Nein |
> | clusters/principalassignments | Nein |
> | clusters/sharedidentities | Nein |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labaccounts | Ja |
> | labplans | Ja |
> | labs | Ja |
> | users | Nein |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hostingEnvironments | Ja |
> | integrationAccounts | Ja |
> | integrationServiceEnvironments | Ja |
> | integrationServiceEnvironments/managedApis | Ja |
> | isolatedEnvironments | Ja |
> | workflows | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | commitmentPlans | Ja |
> | webServices | Ja |
> | Arbeitsbereiche | Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | aisysteminventories | Ja |
> | virtualclusters | Ja |
> | workspaces | Ja |
> | workspaces/batchEndpoints | Ja |
> | workspaces/batchEndpoints/deployments | Ja |
> | workspaces/batchEndpoints/deployments/jobs | Nein |
> | workspaces/batchEndpoints/jobs | Nein |
> | workspaces/codes | Nein |
> | workspaces/codes/versions | Nein |
> | workspaces/components | Nein |
> | workspaces/components/versions | Nein |
> | workspaces/computes | Nein |
> | workspaces/data | Nein |
> | workspaces/datasets | Nein |
> | workspaces/datastores | Nein |
> | workspaces/environments | Nein |
> | workspaces/eventGridFilters | Nein |
> | workspaces/jobs | Nein |
> | workspaces/labelingJobs | Nein |
> | workspaces/linkedServices | Nein |
> | workspaces/models | Nein |
> | workspaces/models/versions | Nein |
> | workspaces/onlineEndpoints | Ja |
> | workspaces/onlineEndpoints/deployments | Ja |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applyUpdates | Nein |
> | configurationAssignments | Nein |
> | maintenanceConfigurations | Ja |
> | publicMaintenanceConfigurations | Nein |
> | updates | Nein |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Identities | Nein |
> | userAssignedIdentities | Ja |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | Nein |
> | registrationAssignments | Nein |
> | registrationDefinitions | Nein |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | getEntities | Nein |
> | managementGroups | Nein |
> | managementGroups/settings | Nein |
> | ressourcen | Nein |
> | startTenantBackfill | Nein |
> | tenantBackfillStatus | Nein |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/creators | Ja |
> | accounts/eventGridFilters | Nein |
> | accounts/privateAtlases | Ja |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | macc | Nein |
> | offers | Nein |
> | offerTypes | Nein |
> | offerTypes/publishers | Nein |
> | offerTypes/publishers/offers | Nein |
> | offerTypes/publishers/offers/plans | Nein |
> | offerTypes/publishers/offers/plans/agreements | Nein |
> | offerTypes/publishers/offers/plans/configs | Nein |
> | offerTypes/publishers/offers/plans/configs/importImage | Nein |
> | privategalleryitems | Nein |
> | privateStoreClient | Nein |
> | privateStores | Nein |
> | privateStores/AdminRequestApprovals | Nein |
> | privateStores/billingAccounts | Nein |
> | privateStores/bulkCollectionsAction | Nein |
> | privateStores/collections | Nein |
> | privateStores/collections/offers | Nein |
> | privateStores/collections/transferOffers | Nein |
> | privateStores/collectionsToSubscriptionsMapping | Nein |
> | privateStores/offers | Nein |
> | privateStores/offers/acknowledgeNotification | Nein |
> | privateStores/queryApprovedPlans | Nein |
> | privateStores/queryNotificationsState | Nein |
> | privateStores/queryOffers | Nein |
> | privateStores/RequestApprovals | Nein |
> | privateStores/requestApprovals/query | Nein |
> | privateStores/requestApprovals/withdrawPlan | Nein |
> | products | Nein |
> | publishers | Nein |
> | publishers/offers | Nein |
> | publishers/offers/amendments | Nein |
> | Registrieren | Nein |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | classicDevServices | Ja |
> | updateCommunicationPreference | Nein |

## <a name="microsoftmarketplacenotifications"></a>Microsoft.MarketplaceNotifications

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | reviewsnotifications | Nein |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | agreements | Nein |
> | offertypes | Nein |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | mediaservices | Ja |
> | mediaservices/accountFilters | Nein |
> | mediaservices/assets | Nein |
> | mediaservices/assets/assetFilters | Nein |
> | mediaservices/contentKeyPolicies | Nein |
> | mediaservices/eventGridFilters | Nein |
> | mediaservices/graphInstances | Nein |
> | mediaservices/graphTopologies | Nein |
> | mediaservices/liveEventOperations | Nein |
> | mediaservices/liveEvents | Ja |
> | mediaservices/liveEvents/liveOutputs | Nein |
> | mediaservices/liveOutputOperations | Nein |
> | mediaservices/mediaGraphs | Nein |
> | mediaservices/privateEndpointConnectionOperations | Nein |
> | mediaservices/privateEndpointConnectionProxies | Nein |
> | mediaservices/privateEndpointConnections | Nein |
> | mediaservices/streamingEndpointOperations | Nein |
> | mediaservices/streamingEndpoints | Ja |
> | mediaservices/streamingLocators | Nein |
> | mediaservices/streamingPolicies | Nein |
> | mediaservices/transforms | Nein |
> | mediaservices/transforms/jobs | Nein |
> | videoAnalyzers | Ja |
> | videoAnalyzers/accessPolicies | Nein |
> | videoAnalyzers/edgeModules | Nein |
> | videoAnalyzers/livePipelines | Nein |
> | videoAnalyzers/pipelineJobs | Nein |
> | videoAnalyzers/pipelineTopologies | Nein |
> | videoAnalyzers/videos | Nein |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | assessmentProjects | Ja |
> | migrateprojects | Ja |
> | moveCollections | Ja |
> | projects | Ja |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Ja |
> | objectAnchorsAccounts | Ja |
> | objectUnderstandingAccounts | Ja |
> | remoteRenderingAccounts | Ja |
> | spatialAnchorsAccounts | Ja |

## <a name="microsoftmobilenetwork"></a>Microsoft.MobileNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | mobileNetworks | Nein |
> | mobileNetworks/dataNetworks | Nein |
> | mobileNetworks/services | Nein |
> | mobileNetworks/simPolicies | Nein |
> | mobileNetworks/sites | Nein |
> | mobileNetworks/slices | Nein |
> | networks | Nein |
> | networks/sites | Nein |
> | packetCoreControlPlanes | Nein |
> | packetCoreControlPlanes/packetCoreDataPlanes | Nein |
> | packetCoreControlPlanes/packetCoreDataPlanes/attachedDataNetworks | Nein |
> | packetCores | Nein |
> | sims | Nein |
> | sims/simProfiles | Nein |

## <a name="microsoftmonitor"></a>Microsoft.Monitor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | netAppAccounts | Ja |
> | netAppAccounts/accountBackups | Nein |
> | netappaccounts/capacitypools | Ja |
> | netappaccounts/capacitypools/volumes | Ja |
> | netappaccounts/capacitypools/volumes/snapshots | Nein |
> | netAppAccounts/capacityPools/volumes/subvolumes | Nein |
> | netAppAccounts/snapshotPolicies | Ja |
> | netAppAccounts/volumeGroups | Nein |

## <a name="microsoftnetworkfunction"></a>Microsoft.NetworkFunction

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | azureTrafficCollectors | Ja |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationGateways | Ja |
> | applicationGatewayWebApplicationFirewallPolicies | Ja |
> | applicationSecurityGroups | Ja |
> | azureFirewallFqdnTags | Nein |
> | azureFirewalls | Ja |
> | bastionHosts | Ja |
> | bgpServiceCommunities | Nein |
> | connections | Ja |
> | ddosCustomPolicies | Ja |
> | ddosProtectionPlans | Ja |
> | dnsOperationStatuses | Nein |
> | dnszones | Ja |
> | dnszones/A | Nein |
> | dnszones/AAAA | Nein |
> | dnszones/all | Nein |
> | dnszones/CAA | Nein |
> | dnszones/CNAME | Nein |
> | dnszones/MX | Nein |
> | dnszones/NS | Nein |
> | dnszones/PTR | Nein |
> | dnszones/recordsets | Nein |
> | dnszones/SOA | Nein |
> | dnszones/SRV | Nein |
> | dnszones/TXT | Nein |
> | expressRouteCircuits | Ja |
> | expressRouteCrossConnections | Ja |
> | expressRouteGateways | Ja |
> | expressRoutePorts | Ja |
> | expressRouteServiceProviders | Nein |
> | firewallPolicies | Ja |
> | frontdoors | Ja |
> | frontdoorWebApplicationFirewallManagedRuleSets | Nein |
> | frontdoorWebApplicationFirewallPolicies | Ja |
> | getDnsResourceReference | Nein |
> | internalNotify | Nein |
> | ipGroups | Ja |
> | loadBalancers | Ja |
> | localNetworkGateways | Ja |
> | natGateways | Ja |
> | networkIntentPolicies | Ja |
> | networkInterfaces | Ja |
> | networkProfiles | Ja |
> | networkSecurityGroups | Ja |
> | networkWatchers | Ja |
> | networkwatchers/connectionmonitors | Ja |
> | networkWatchers/flowLogs | Ja |
> | networkwatchers/lenses | Ja |
> | networkwatchers/pingmeshes | Ja |
> | p2sVpnGateways | Ja |
> | privateDnsOperationStatuses | Nein |
> | privateDnsZones | Ja |
> | privateDnsZones/A | Nein |
> | privateDnsZones/AAAA | Nein |
> | privateDnsZones/all | Nein |
> | privateDnsZones/CNAME | Nein |
> | privateDnsZones/MX | Nein |
> | privateDnsZones/PTR | Nein |
> | privateDnsZones/SOA | Nein |
> | privateDnsZones/SRV | Nein |
> | privateDnsZones/TXT | Nein |
> | privateDnsZones/virtualNetworkLinks | Ja |
> | privateEndpoints | Ja |
> | privateLinkServices | Ja |
> | publicIPAddresses | Ja |
> | publicIPPrefixes | Ja |
> | routeFilters | Ja |
> | routeTables | Ja |
> | serviceEndpointPolicies | Ja |
> | trafficManagerGeographicHierarchies | Nein |
> | trafficmanagerprofiles | Ja |
> | trafficmanagerprofiles/heatMaps | Nein |
> | trafficManagerUserMetricsKeys | Nein |
> | virtualHubs | Ja |
> | virtualNetworkGateways | Ja |
> | virtualNetworks | Ja |
> | virtualNetworks/subnets | Nein |
> | virtualNetworkTaps | Ja |
> | virtualWans | Ja |
> | vpnGateways | Ja |
> | vpnSites | Ja |
> | webApplicationFirewallPolicies | Ja |

## <a name="microsoftnotebooks"></a>Microsoft.Notebooks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | NotebookProxies | Nein |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Namespaces | Ja |
> | namespaces/notificationHubs | Ja |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | osNamespaces | Nein |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | HyperVSites | Ja |
> | ImportSites | Ja |
> | MasterSites | Ja |
> | ServerSites | Ja |
> | VMwareSites | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | deletedWorkspaces | Nein |
> | linkTargets | Nein |
> | querypacks | Ja |
> | storageInsightConfigs | Nein |
> | workspaces | Ja |
> | workspaces/dataExports | Nein |
> | workspaces/dataSources | Nein |
> | workspaces/linkedServices | Nein |
> | workspaces/linkedStorageAccounts | Nein |
> | workspaces/metadata | Nein |
> | workspaces/query | Nein |
> | workspaces/scopedPrivateLinkProxies | Nein |
> | workspaces/storageInsightConfigs | Nein |
> | workspaces/tables | Nein |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managementassociations | Nein |
> | managementconfigurations | Ja |
> | solutions | Ja |
> | views | Ja |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | cdnPeeringPrefixes | Nein |
> | legacyPeerings | Nein |
> | lookingGlass | Nein |
> | peerAsns | Nein |
> | peerings | Ja |
> | peeringServiceCountries | Nein |
> | peeringServiceProviders | Nein |
> | peeringServices | Ja |

## <a name="microsoftplayfab"></a>Microsoft.PlayFab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | PlayerAccountPools | Nein |
> | Titel | Nein |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | attestations | Nein |
> | eventGridFilters | Nein |
> | policyEvents | Nein |
> | policyMetadata | Nein |
> | policyStates | Nein |
> | policyTrackedResources | Nein |
> | remediations | Nein |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | consoles | Nein |
> | dashboards | Ja |
> | tenantconfigurations | Nein |
> | userSettings | Nein |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | privateLinkServicesForPowerBI | Ja |
> | tenants | Ja |
> | tenants/workspaces | Nein |
> | workspaceCollections | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | autoScaleVCores | Ja |
> | capacities | Ja |
> | servers | Ja |

## <a name="microsoftpowerplatform"></a>Microsoft.PowerPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | enterprisePolicies | Ja |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | deletedAccounts | Nein |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | providerRegistrations | Nein |
> | providerRegistrations/customRollouts | Nein |
> | providerRegistrations/defaultRollouts | Nein |
> | providerRegistrations/resourceActions | Nein |
> | providerRegistrations/resourceTypeRegistrations | Nein |

## <a name="microsoftpurview"></a>Microsoft.Purview

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | deletedAccounts | Nein |
> | getDefaultAccount | Nein |
> | removeDefaultAccount | Nein |
> | setDefaultAccount | Nein |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Arbeitsbereiche | Nein |

## <a name="microsoftquota"></a>Microsoft.Quota

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | quotaRequests | Nein |
> | quotas | Nein |
> | usages | Nein |

## <a name="microsoftrecommendationsservice"></a>Microsoft.RecommendationsService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Nein |
> | accounts/modeling | Nein |
> | accounts/serviceEndpoints | Nein |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | backupProtectedItems | Nein |
> | vaults | Ja |

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | OpenShiftClusters | Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Namespaces | Ja |
> | namespaces/authorizationrules | Nein |
> | namespaces/hybridconnections | Nein |
> | namespaces/hybridconnections/authorizationrules | Nein |
> | namespaces/privateEndpointConnections | Nein |
> | namespaces/wcfrelays | Nein |
> | namespaces/wcfrelays/authorizationrules | Nein |

## <a name="microsoftresourceconnector"></a>Microsoft.ResourceConnector

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | appliances | Ja |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Abfragen | Ja |
> | resourceChangeDetails | Nein |
> | resourceChanges | Nein |
> | ressourcen | Nein |
> | resourcesHistory | Nein |
> | subscriptionsStatus | Nein |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | availabilityStatuses | Nein |
> | childAvailabilityStatuses | Nein |
> | childResources | Nein |
> | emergingissues | Nein |
> | events | Nein |
> | impactedResources | Nein |
> | metadata | Nein |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deployments | Nein |
> | deployments/operations | Nein |
> | deploymentScripts | Ja |
> | deploymentScripts/logs | Nein |
> | deploymentStacks | Nein |
> | deploymentStacks/snapshots | Nein |
> | Verknüpfungen | Nein |
> | providers | Nein |
> | resourceGroups | Nein |
> | subscriptions | Nein |
> | templateSpecs | Ja |
> | templateSpecs/versions | Ja |
> | tenants | Nein |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja |
> | ressourcen | Ja |
> | saasresources | Nein |

## <a name="microsoftscom"></a>Microsoft.Scom

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managedInstances | Nein |

## <a name="microsoftscvmm"></a>Microsoft.ScVmm

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clouds | Nein |
> | VirtualMachines | Nein |
> | VirtualMachineTemplates | Nein |
> | VirtualNetworks | Nein |
> | vmmservers | Nein |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | resourceHealthMetadata | Nein |
> | searchServices | Ja |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | Nein |
> | advancedThreatProtectionSettings | Nein |
> | alerts | Nein |
> | alertsSuppressionRules | Nein |
> | allowedConnections | Nein |
> | antiMalwareSettings | Nein |
> | applicationWhitelistings | Nein |
> | assessmentMetadata | Nein |
> | assessments | Nein |
> | Zuweisungen | Ja |
> | autoDismissAlertsRules | Nein |
> | automations | Ja |
> | AutoProvisioningSettings | Nein |
> | Compliances | Nein |
> | connectedContainerRegistries | Nein |
> | connectors | Nein |
> | customAssessmentAutomations | Ja |
> | customEntityStoreAssignments | Ja |
> | dataCollectionAgents | Nein |
> | deviceSecurityGroups | Nein |
> | discoveredSecuritySolutions | Nein |
> | externalSecuritySolutions | Nein |
> | InformationProtectionPolicies | Nein |
> | ingestionSettings | Nein |
> | insights | Nein |
> | iotSecuritySolutions | Ja |
> | iotSecuritySolutions/analyticsModels | Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedAlerts | Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedRecommendations | Nein |
> | iotSecuritySolutions/iotAlerts | Nein |
> | iotSecuritySolutions/iotAlertTypes | Nein |
> | iotSecuritySolutions/iotRecommendations | Nein |
> | iotSecuritySolutions/iotRecommendationTypes | Nein |
> | jitNetworkAccessPolicies | Nein |
> | jitPolicies | Nein |
> | Richtlinien | Nein |
> | pricings | Nein |
> | regulatoryComplianceStandards | Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls | Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls/regulatoryComplianceAssessments | Nein |
> | secureScoreControlDefinitions | Nein |
> | secureScoreControls | Nein |
> | secureScores | Nein |
> | secureScores/secureScoreControls | Nein |
> | securityConnectors | Ja |
> | securityContacts | Nein |
> | securitySolutions | Nein |
> | securitySolutionsReferenceData | Nein |
> | securityStatuses | Nein |
> | securityStatusesSummaries | Nein |
> | serverVulnerabilityAssessments | Nein |
> | settings | Nein |
> | sqlVulnerabilityAssessments | Nein |
> | standards | Ja |
> | subAssessments | Nein |
> | Tasks | Nein |
> | topologies | Nein |
> | workspaceSettings | Nein |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein |
> | diagnosticSettingsCategories | Nein |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | aggregations | Nein |
> | alertRules | Nein |
> | alertRuleTemplates | Nein |
> | automationRules | Nein |
> | bookmarks | Nein |
> | cases | Nein |
> | dataConnectors | Nein |
> | dataConnectorsCheckRequirements | Nein |
> | enrichment | Nein |
> | entities | Nein |
> | entityQueries | Nein |
> | entityQueryTemplates | Nein |
> | incidents | Nein |
> | metadata | Nein |
> | officeConsents | Nein |
> | onboardingStates | Nein |
> | settings | Nein |
> | sourceControls | Nein |
> | threatIntelligence | Nein |
> | watchlists | Nein |

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | consoleServices | Nein |
> | serialPorts | Nein |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Namespaces | Ja |
> | namespaces/authorizationrules | Nein |
> | namespaces/disasterrecoveryconfigs | Nein |
> | namespaces/eventgridfilters | Nein |
> | namespaces/networkrulesets | Nein |
> | namespaces/privateEndpointConnections | Nein |
> | namespaces/queues | Nein |
> | namespaces/queues/authorizationrules | Nein |
> | namespaces/topics | Nein |
> | namespaces/topics/authorizationrules | Nein |
> | namespaces/topics/subscriptions | Nein |
> | namespaces/topics/subscriptions/rules | Nein |
> | premiumMessagingRegions | Nein |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja |
> | clusters | Ja |
> | clusters/applications | Nein |
> | containerGroups | Ja |
> | containerGroupSets | Ja |
> | edgeclusters | Ja |
> | edgeclusters/applications | Nein |
> | managedclusters | Ja |
> | managedclusters/applications | Nein |
> | managedclusters/applications/services | Nein |
> | managedclusters/applicationTypes | Nein |
> | managedclusters/applicationTypes/versions | Nein |
> | managedclusters/nodetypes | Nein |
> | networks | Ja |
> | secretstores | Ja |
> | secretstores/certificates | Nein |
> | secretstores/secrets | Nein |
> | volumes | Ja |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja |
> | containerGroups | Ja |
> | gateways | Ja |
> | networks | Ja |
> | secrets | Ja |
> | volumes | Ja |

## <a name="microsoftservicelinker"></a>Microsoft.ServiceLinker

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | linkers | Nein |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | providerRegistrations | Nein |
> | providerRegistrations/resourceTypeRegistrations | Nein |
> | rollouts | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SignalR | Ja |
> | SignalR/eventGridFilters | Nein |
> | WebPubSub | Ja |

## <a name="microsoftsingularity"></a>Microsoft.Singularity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/accountQuotaPolicies | Nein |
> | accounts/groupPolicies | Nein |
> | accounts/jobs | Nein |
> | accounts/models | Nein |
> | accounts/storageContainers | Nein |
> | images | Nein |
> | quotas | Nein |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hybridUseBenefits | Nein |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationDefinitions | Ja |
> | applications | Ja |
> | jitRequests | Ja |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managedInstances | Ja |
> | managedinstances/databases | Ja |
> | managedInstances/databases/backupShortTermRetentionPolicies | Nein |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nein |
> | managedInstances/databases/vulnerabilityAssessments | Nein |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nein |
> | managedInstances/encryptionProtector | Nein |
> | managedInstances/keys | Nein |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nein |
> | managedInstances/vulnerabilityAssessments | Nein |
> | servers | Ja |
> | servers/administrators | Nein |
> | servers/communicationLinks | Nein |
> | servers/databases | Ja |
> | servers/encryptionProtector | Nein |
> | servers/firewallRules | Nein |
> | servers/keys | Nein |
> | servers/restorableDroppedDatabases | Nein |
> | servers/serviceobjectives | Nein |
> | servers/tdeCertificates | Nein |
> | virtualClusters | Nein |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Ja |
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Nein |
> | SqlVirtualMachines | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deletedAccounts | Nein |
> | storageAccounts | Ja |
> | storageAccounts/blobServices | Nein |
> | storageAccounts/encryptionScopes | Nein |
> | storageAccounts/fileServices | Nein |
> | storageAccounts/queueServices | Nein |
> | storageAccounts/services | Nein |
> | storageAccounts/services/metricDefinitions | Nein |
> | storageAccounts/tableServices | Nein |
> | usages | Nein |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | amlFilesystems | Ja |
> | caches | Ja |
> | caches/storageTargets | Nein |
> | usageModels | Nein |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | replicationGroups | Nein |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | storageSyncServices | Ja |
> | storageSyncServices/registeredServers | Nein |
> | storageSyncServices/syncGroups | Nein |
> | storageSyncServices/syncGroups/cloudEndpoints | Nein |
> | storageSyncServices/syncGroups/serverEndpoints | Nein |
> | storageSyncServices/workflows | Nein |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managers | Ja |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | clusters/privateEndpoints | Nein |
> | streamingjobs | Ja |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | acceptChangeTenant | Nein |
> | acceptOwnership | Nein |
> | acceptOwnershipStatus | Nein |
> | aliases | Nein |
> | cancel | Nein |
> | changeTenantRequest | Nein |
> | changeTenantStatus | Nein |
> | CreateSubscription | Nein |
> | enable | Nein |
> | Richtlinien | Nein |
> | rename | Nein |
> | SubscriptionDefinitions | Nein |
> | SubscriptionOperations | Nein |
> | subscriptions | Nein |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | kustoOperations | Nein |
> | privateLinkHubs | Ja |
> | workspaces | Ja |
> | workspaces/bigDataPools | Ja |
> | workspaces/kustoPools | Ja |
> | workspaces/kustoPools/attacheddatabaseconfigurations | Nein |
> | workspaces/kustoPools/databases | Nein |
> | workspaces/kustoPools/databases/dataconnections | Nein |
> | workspaces/operationStatuses | Nein |
> | workspaces/sqlDatabases | Ja |
> | workspaces/sqlPools | Ja |

## <a name="microsofttestbase"></a>Microsoft.TestBase

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | testBaseAccounts | Nein |
> | testBaseAccounts/customerEvents | Nein |
> | testBaseAccounts/emailEvents | Nein |
> | testBaseAccounts/flightingRings | Nein |
> | testBaseAccounts/packages | Nein |
> | testBaseAccounts/packages/favoriteProcesses | Nein |
> | testBaseAccounts/packages/osUpdates | Nein |
> | testBaseAccounts/testSummaries | Nein |
> | testBaseAccounts/testTypes | Nein |
> | testBaseAccounts/usages | Nein |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | environments | Ja |
> | environments/accessPolicies | Nein |
> | environments/eventsources | Ja |
> | environments/privateEndpointConnectionProxies | Nein |
> | environments/privateEndpointConnections | Nein |
> | environments/privateLinkResources | Nein |
> | environments/referenceDataSets | Ja |

## <a name="microsoftvideoindexer"></a>Microsoft.VideoIndexer

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Nein |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | imageTemplates | Ja |
> | imageTemplates/runOutputs | Nein |

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | arczones | Nein |
> | resourcepools | Nein |
> | vcenters | Nein |
> | VCenters/InventoryItems | Nein |
> | virtualmachines | Nein |
> | virtualmachinetemplates | Nein |
> | virtualnetworks | Nein |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Ja |
> | dedicatedCloudServices | Ja |
> | virtualMachines | Ja |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | plans | Ja |
> | registeredSubscriptions | Nein |
## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | apiManagementAccounts | Nein |
> | apiManagementAccounts/apiAcls | Nein |
> | apiManagementAccounts/apis | Nein |
> | apiManagementAccounts/apis/apiAcls | Nein |
> | apiManagementAccounts/apis/connectionAcls | Nein |
> | apiManagementAccounts/apis/connections | Nein |
> | apiManagementAccounts/apis/connections/connectionAcls | Nein |
> | apiManagementAccounts/apis/localizedDefinitions | Nein |
> | apiManagementAccounts/connectionAcls | Nein |
> | apiManagementAccounts/connections | Nein |
> | billingMeters | Nein |
> | certificates | Ja |
> | connectionGateways | Ja |
> | connections | Ja |
> | customApis | Ja |
> | deletedSites | Nein |
> | functionAppStacks | Nein |
> | generateGithubAccessTokenForAppserviceCLI | Nein |
> | hostingEnvironments | Ja |
> | hostingEnvironments/eventGridFilters | Nein |
> | hostingEnvironments/multiRolePools | Nein |
> | hostingEnvironments/workerPools | Nein |
> | kubeEnvironments | Ja |
> | publishingUsers | Nein |
> | empfehlungen | Nein |
> | resourceHealthMetadata | Nein |
> | runtimes | Nein |
> | serverFarms | Ja |
> | serverFarms/eventGridFilters | Nein |
> | serverFarms/firstPartyApps | Nein |
> | serverFarms/firstPartyApps/keyVaultSettings | Nein |
> | sites | Ja |
> | sites/config  | Nein |
> | sites/eventGridFilters | Nein |
> | sites/hostNameBindings | Nein |
> | sites/networkConfig | Nein |
> | sites/premieraddons | Ja |
> | sites/slots | Ja |
> | sites/slots/eventGridFilters | Nein |
> | sites/slots/hostNameBindings | Nein |
> | sites/slots/networkConfig | Nein |
> | sourceControls | Nein |
> | staticSites | Ja |
> | validate | Nein |
> | verifyHostingEnvironmentVnet | Nein |
> | webAppStacks | Nein |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein |
> | diagnosticSettingsCategories | Nein |

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | multipleActivationKeys | Ja |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DeviceServices | Ja |

## <a name="microsoftworkloadbuilder"></a>Microsoft.WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | migrationAgents | Nein |
> | workloads | Nein |
> | workloads/instances | Nein |
> | workloads/versions | Nein |
> | workloads/versions/artifacts | Nein |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | monitors | Nein |

## <a name="next-steps"></a>Nächste Schritte

Um die gleichen Daten als Datei mit durch Trennzeichen getrennten Werten abzurufen, laden Sie [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv) herunter.