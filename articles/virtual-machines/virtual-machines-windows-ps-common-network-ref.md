---
title: "Häufige PowerShell-Netzwerkbefehle für VMs | Microsoft Docs"
description: "Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe für die Erstellung eines virtuellen Netzwerks und der zugeordneten Ressourcen für VMs."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 45a45b616b4de005da66562c69eef83f2f48cc79
ms.openlocfilehash: ce08e9dcd0585e00bdd42b8aa34ac2c597a53a6e


---
# <a name="common-network-azure-powershell-commands-for-vms"></a>Häufige Azure PowerShell-Netzwerkbefehle für VMs
Zur Erstellung eines virtuellen Computers müssen Sie ein [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) erstellen oder über ein vorhandenes virtuelles Netzwerk verfügen, dem die VM hinzugefügt werden kann. Normalerweise müssen Sie beim Erstellen einer VM auch die in diesem Artikel beschriebenen Ressourcen erstellen.

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
| Aufgabe | Befehl |
| --- | --- |
| Erstellen von Subnetzkonfigurationen |$subnet1 = [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -Name "subnet_name" -AddressPrefix XX.X.X.X/XX<BR>$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name "subnet_name" -AddressPrefix XX.X.X.X/XX<BR><BR>Ein typisches Netzwerk kann über ein Subnetz für einen [Load Balancer mit Internetzugriff](../load-balancer/load-balancer-internet-overview.md) und ein separates Subnetz für einen [internen Load Balancer](../load-balancer/load-balancer-internal-overview.md) verfügen. |
| Erstellen eines virtuellen Netzwerks |$vnet = [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -Name "virtual_network_name" -ResourceGroupName "resource_group_name" -Location "location_name" -AddressPrefix XX.X.X.X/XX -Subnet $subnet1, $subnet2 |
| Überprüfen auf einen eindeutigen Domänennamen |[Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) -DomainQualifiedName "domain_name" -Location "location_name"<BR><BR>Sie können einen DNS-Domänennamen für eine [öffentliche IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md) angeben. Dadurch erstellen Sie für domainname.location.cloudapp.azure.com eine Zuordnung zur öffentlichen IP-Adresse auf den von Azure verwalteten DNS-Servern. Der Name darf nur Buchstaben, Zahlen und Bindestriche enthalten. Das erste und letzte Zeichen muss jeweils ein Buchstabe oder eine Zahl sein, und der Domänenname muss für den Azure-Standort eindeutig sein. Wenn **TRUE** zurückgegeben wird, ist der vorgeschlagene Name global eindeutig. |
| Erstellen einer öffentlichen IP-Adresse |$pip = [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -Name "ip_address_name" -ResourceGroupName "resource_group_name" -DomainNameLabel "domain_name" -Location "location_name" -AllocationMethod Dynamic<BR><BR>Für die öffentliche IP-Adresse wird der zuvor getestete Domänenname verwendet. Sie wird bei der Front-End-Konfiguration des Load Balancers genutzt. |
| Erstellen einer Front-End-IP-Konfiguration |$frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -Name "frontend_ip_name" -PublicIpAddress $pip<BR><BR>Die Front-End-Konfiguration enthält die öffentliche IP-Adresse, die Sie zuvor für eingehenden Netzwerk-Datenverkehr erstellt haben. |
| Erstellen eines Back-End-Adresspools |$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -Name "backend_pool_name"<BR><BR>Stellt interne Adressen für das Back-End des Load Balancers bereit, auf die über eine Netzwerkschnittstelle zugegriffen wird. |
| Erstellen eines Tests |$healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -Name "probe_name" -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2<BR><BR>Enthält Integritätstests, die zum Überprüfen der Verfügbarkeit von Instanzen virtueller Computer im Back-End-Adresspool verwendet werden. |
| Erstellen einer Lastenausgleichsregel |$lbRule = [New-AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port im Back-End-Adresspool zugewiesen wird. |
| Erstellen einer eingehenden NAT-Regel |$inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -Name "rule_name" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389<BR><BR>Enthält Regeln, mit denen ein öffentlicher Port auf dem Load Balancer einem Port für einen bestimmten virtuellen Computer im Back-End-Adresspool zugeordnet wird. |
| Einrichten eines Load Balancers |$loadBalancer = [New-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) -ResourceGroupName "resource_group_name" -Name "load_balancer_name" -Location "location_name" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule -LoadBalancingRule $lbRule -BackendAddressPool $beAddressPool -Probe $healthProbe |
| Erstellen einer Netzwerkschnittstelle |$nic1= [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) -ResourceGroupName "resource_group_name" -Name "network_interface_name" -Location "location_name" -PrivateIpAddress XX.X.X.X -Subnet subnet2 -LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] -LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Erstellen Sie eine Netzwerkschnittstelle, indem Sie die öffentliche IP-Adresse und das zuvor erstellte virtuelle Netzwerksubnetz verwenden. |

## <a name="get-information-about-network-resources"></a>Abrufen von Informationen zu Netzwerkressourcen
| Aufgabe | Befehl |
| --- | --- |
| Auflisten von virtuellen Netzwerken |[Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) -ResourceGroupName "resource_group_name"<BR><BR>Listet alle virtuellen Netzwerke in der Ressourcengruppe auf. |
| Abrufen von Informationen zu einem virtuellen Netzwerk |Get-AzureRmVirtualNetwork -Name "virtual_network_name" -ResourceGroupName "resource_group_name" |
| Auflisten von Subnetzen in einem virtuellen Netzwerk |Get-AzureRmVirtualNetwork -Name "virtual_network_name" -ResourceGroupName "resource_group_name" &#124; Select Subnets |
| Abrufen von Informationen zu einem Subnetz |[Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -Name "subnet_name" -VirtualNetwork $vnet<BR><BR>Ruft Informationen zu dem Subnetz im angegebenen virtuellen Netzwerk ab. Der $vnet-Wert gibt das von „Get-AzureRmVirtualNetwork“ zurückgegebene Objekt an. |
| Auflisten von IP-Adressen |[Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) -ResourceGroupName "resource_group_name"<BR><BR>Listet die öffentlichen IP-Adressen in der Ressourcengruppe auf. |
| Auflisten von Load Balancern |[Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) -ResourceGroupName "resource_group_name"<BR><BR>Listet alle Load Balancer in der Ressourcengruppe auf. |
| Auflisten von Netzwerkschnittstellen |[Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) -ResourceGroupName "resource_group_name"<BR><BR>Listet alle Netzwerkschnittstellen in der Ressourcengruppe auf. |
| Abrufen von Informationen zu einer Netzwerkschnittstelle |Get-AzureRmNetworkInterface -Name "network_interface_name" -ResourceGroupName "resource_group_name"<BR><BR>Ruft Informationen zu einer bestimmten Netzwerkschnittstelle ab. |
| Abrufen der IP-Konfiguration einer Netzwerkschnittstelle |[Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -Name "ipconfiguration_name" -NetworkInterface $nic<BR><BR>Ruft Informationen zur IP-Konfiguration der angegebenen Netzwerkschnittstelle ab. Der $nic-Wert gibt das von „Get-AzureRmNetworkInterface“ zurückgegebene Objekt an. |

## <a name="manage-network-resources"></a>Verwalten von Netzwerkressourcen
| Aufgabe | Befehl |
| --- | --- |
| Hinzufügen eines Subnetzes zu einem virtuellen Netzwerk |[Add-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) -AddressPrefix XX.X.X.X/XX -Name "subnet_name" -VirtualNetwork $vnet<BR><BR>Fügt einem vorhandenen virtuellen Netzwerk ein Subnetz hinzu. Der $vnet-Wert gibt das von „Get-AzureRmVirtualNetwork“ zurückgegebene Objekt an. |
| Löschen eines virtuellen Netzwerks |[Remove-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -Name "virtual_network_name" -ResourceGroupName "resource_group_name"<BR><BR>Entfernt das angegebene virtuelle Netzwerk aus der Ressourcengruppe. |
| Löschen einer Netzwerkschnittstelle |[Remove-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -Name "network_interface_name" -ResourceGroupName "resource_group_name"<BR><BR>Entfernt die angegebene Netzwerkschnittstelle aus der Ressourcengruppe. |
| Löschen eines Load Balancers |[Remove-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -Name "load_balancer_name" -ResourceGroupName "resource_group_name"<BR><BR>Entfernt den angegebenen Load Balancer aus der Ressourcengruppe. |
| Löschen einer öffentlichen IP-Adresse |[Remove-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-Name "ip_address_name" -ResourceGroupName "resource_group_name"<BR><BR>Entfernt die angegebene öffentliche IP-Adresse aus der Ressourcengruppe. |

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die soeben erstellte Netzwerkschnittstelle beim [Erstellen einer VM](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Erfahren Sie, wie Sie eine [VM mit mehreren Netzwerkschnittstellen erstellen](../virtual-network/virtual-networks-multiple-nics.md).




<!--HONumber=Dec16_HO2-->


