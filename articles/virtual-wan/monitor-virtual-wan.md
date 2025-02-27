---
title: 'Monitoring Azure Virtual WAN'
description: Learn about Azure Virtual WAN logs and metrics using Azure Monitor.
services: virtual-wan
author: cherylmc

ms.service: virtual-wan
ms.topic: how-to
ms.date: 06/30/2021
ms.author: cherylmc

---

# Monitoring Virtual WAN

You can monitor Azure Virtual WAN using Azure Monitor. Virtual WAN is a networking service that brings together many networking, security, and routing functionalities to provide a single operational interface. Virtual WAN VPN gateways, ExpressRoute gateways, and Azure Firewall have logging and metrics available through Azure Monitor.

This article discusses metrics and diagnostics that are available through the portal. Metrics are lightweight and can support near real-time scenarios, making them useful for alerting and fast issue detection.

### Monitoring Secured Hub (Azure Firewall) 

You can monitor the Secured Hub using Azure Firewall logs. You can also use activity logs to audit operations on Azure Firewall resources.

If you have chosen to secure your Virtual Hub using Azure Firewall, relevant logs and metrics are available here: [Azure Firewall logs and metrics](../firewall/logs-and-metrics.md).

## Metrics

Metrics in Azure Monitor are numerical values that describe some aspect of a system at a particular time. Metrics are collected every minute, and are useful for alerting because they can be sampled frequently. An alert can be fired quickly with relatively simple logic.

### Site-to-site VPN gateways

The following metrics are available for Azure site-to-site VPN gateways:

#### Tunnel Packet Drop Metrics
| Metric | Description|
| --- | --- |
| **Tunnel Egress Packet Drop Count** | Count of Outgoing packets dropped by tunnel.|
| **Tunnel Ingress Packet Drop Count** | Count of Incoming packets dropped by tunnel.|
| **Tunnel NAT Packet Drops** | Number of NATed packets dropped on a tunnel by drop type and NAT rule.|
| **Tunnel Egress TS Mismatch Packet Drop** | Outgoing packet drop count from traffic selector mismatch of a tunnel.|
| **Tunnel Ingress TS Mismatch Packet Drop** | Incoming packet drop count from traffic selector mismatch of a tunnel.|

#### IPSEC Metrics
| Metric | Description|
| --- | --- |
| **Tunnel MMSA Count** | Number of MMSAs getting created or deleted.|
| **Tunnel QMSA Count** | Number of  IPSEC QMSAs getting created or deleted.|

#### Routing Metrics
| Metric | Description|
| --- | --- |
| **BGP Peer Status** | BGP connectivity status per peer and per instance.|
| **BGP Routes Advertised** | Number of routes advertised per peer and per intance.|
| **BGP Routes Learned** | Number of routes learned per peer and per intance.|
| **VNET Address Prefix Count** | Number of VNET address prefixes that are used/advertised by the gateway.|

You can review per peer and instance metrics by slecting **Apply splitting** and choosing the preferred value. 

#### Traffic Flow Metrics
| Metric | Description|
| --- | --- |
| **Gateway Bandwidth** | Average site-to-site aggregate bandwidth of a gateway in bytes per second.|
| **Tunnel Bandwidth** | Average bandwidth of a tunnel in bytes per second.|
| **Tunnel Egress Bytes** | Outgoing bytes of a tunnel. |
| **Tunnel Egress Packets** | Outgoing packet count of a tunnel. |
| **Tunnel Ingress Bytes** | Incoming bytes of a tunnel.|
| **Tunnel Ingress Packet** | Incoming packet count of a tunnel.|
| **Tunnel Peak PPS** | Number of packets per second per link connection in the last minute.|
| **Tunnel Flow Count** | Number of distinct flows created per link connection.|

### Point-to-site VPN gateways

The following metrics are available for Azure point-to-site VPN gateways:

| Metric | Description|
| --- | --- |
| **Gateway P2S Bandwidth** | Average point-to-site aggregate bandwidth of a gateway in bytes per second. |
| **P2S Connection Count** |Point-to-site connection count of a gateway. Point-to-site connection count of a gateway. To ensure you are viewing accurate Metrics in Azure Monitor, please select the **Aggregation Type** for **P2S Connection Count** as **Sum**. You may also select **Max** if you also Split By **Instance**. |
| **User VPN Routes Count** | Number of User VPN Routes configured on the VPN Gateway. This metric can be broken down into **Static** and **Dynamic** Routes.

### Azure ExpressRoute gateways

The following metrics are available for Azure ExpressRoute gateways:

| Metric | Description|
| --- | --- |
| **BitsInPerSecond** | Bits ingressing Azure per second.|
| **BitsOutPerSecond** | Bits egressing Azure per second. |

### <a name="metrics-steps"></a>View gateway metrics

The following steps help you locate and view metrics:

1. In the portal, navigate to the virtual hub that has the gateway.

2. Select **VPN (Site to site)** to locate a site-to-site gateway, **ExpressRoute** to locate an ExpressRoute gateway, or **User VPN (Point to site)** to locate a point-to-site gateway. On the page, you can see the gateway information. Copy this information. You will use it later to view diagnostics using Azure Monitor.

3. Select **Metrics**.

   :::image type="content" source="./media/monitor-virtual-wan/metrics.png" alt-text="Screenshot shows a site to site V P N pane with View in Azure Monitor selected.":::

4. On the **Metrics** page, you can view the metrics that you are interested in.

   :::image type="content" source="./media/monitor-virtual-wan/metrics-page.png" alt-text="Screenshot that shows the 'Metrics' page with the categories highlighted.":::

## <a name="diagnostic"></a>Diagnostic logs

### Site-to-site VPN gateways

The following diagnostics are available for Azure site-to-site VPN gateways:

| Metric | Description|
| --- | --- |
| **Gateway Diagnostic Logs** | Gateway-specific diagnostics such as health, configuration, service updates, as well as additional diagnostics.|
| **Tunnel Diagnostic Logs** | These are IPsec tunnel-related logs such as connect and disconnect events for a site-to-site IPsec tunnel, negotiated SAs, disconnect reasons, as well as additional diagnostics.|
| **Route Diagnostic Logs** | These are logs related to events for static routes, BGP, route updates, as well as additional diagnostics. |
| **IKE Diagnostic Logs** | IKE-specific diagnostics for IPsec connections. |

### Point-to-site VPN gateways

The following diagnostics are available for Azure point-to-site VPN gateways:

| Metric | Description|
| --- | --- |
| **Gateway Diagnostic Logs** | Gateway-specific diagnostics such as health, configuration, service updates, as well as other diagnostics. |
| **IKE Diagnostic Logs** | IKE-specific diagnostics for IPsec connections.|
| **P2S Diagnostic Logs** | These are User VPN (Point-to-site) P2S configuration and client events. They include client connect/disconnect, VPN client address allocation, as well as other diagnostics.|

### <a name="diagnostic-steps"></a>View diagnostic logs

The following steps help you locate and view diagnostics:

1. In the portal, navigate to your Virtual WAN resource. In the **Overview** section of the Virtual WAN page in the portal, select **Essentials** to expand the view and obtain resource group information. Copy the resource group information.

   :::image type="content" source="./media/monitor-virtual-wan/3.png" alt-text="Screenshot that shows the 'Overview' section with an arrow pointing to the 'Copy' button.":::

2. Navigate to **Monitor** from the search bar and in the Settings section, select **Diagnostic settings**, then input the resource group, resource type and resource information. This is the resource group information that you copied in Step 2 from the [View gateway metrics](#metrics-steps) section, earlier in this article.

   :::image type="content" source="./media/monitor-virtual-wan/4.png" alt-text="Screenshot that shows the 'Monitoring' section with an arrow pointing to the 'Resource' drop-down.":::

3. On the results page, select **+Add diagnostic setting**, then select an option. You can choose to send to Log Analytics, stream to an event hub, or to simply archive to a storage account.

   :::image type="content" source="./media/monitor-virtual-wan/5.png" alt-text="metrics page":::

### <a name="sample-query"></a>Log Analytics sample query

Logs are located in **Azure Log Analytics Workspace**. You can set up a query in Log Analytics. The following example contains a query to obtain site-to-site route diagnostics.

```AzureDiagnostics | where Category == "RouteDiagnosticLog"```

Replace the values below, after the **= =**, as needed.

* "GatewayDiagnosticLog"
* "IKEDiagnosticLog"
* "P2SDiagnosticLog”
* "TunnelDiagnosticLog"
* "RouteDiagnosticLog"

## <a name="activity-logs"></a>Activity logs

**Activity log** entries are collected by default and can be viewed in the Azure portal. You can use Azure activity logs (formerly known as *operational logs* and *audit logs*) to view all operations submitted to your Azure subscription.

## Next steps

* To learn how to monitor Azure Firewall logs and metrics, see [Tutorial: Monitor Azure Firewall logs](../firewall/firewall-diagnostics.md).
* To learn more about metrics in Azure Monitor, see [Metrics in Azure Monitor](../azure-monitor/essentials/data-platform-metrics.md).
