---
title: Modify a packet core instance
titleSuffix: Azure Private 5G Core Preview
description: In this how-to guide, you'll learn how to modify a packet core instance using the Azure portal. 
author: b-branco
ms.author: biancabranco
ms.service: private-5g-core
ms.topic: how-to
ms.date: 09/29/2022
ms.custom: template-how-to
---

# Modify the packet core instance in a site

Each Azure Private 5G Core Preview site contains a packet core instance, which is a cloud-native implementation of the 3GPP standards-defined 5G Next Generation Core (5G NGC or 5GC). In this how-to guide, you'll learn how to modify a packet core instance using the Azure portal; this includes modifying the packet core's custom location, connected Azure Stack Edge device, and access network configuration. You'll also learn how to add and modify the data networks attached to the packet core instance.

If you want to modify a packet core instance's local access configuration, follow [Modify the local access configuration in a site](modify-local-access-configuration.md).

## Prerequisites

- If you want to make changes to the packet core configuration or access network, refer to [Collect packet core configuration values](collect-required-information-for-a-site.md#collect-packet-core-configuration-values) and [Collect access network values](collect-required-information-for-a-site.md#collect-access-network-values) to collect the new values and make sure they're in the correct format.

    > [!NOTE]
    > You can't update a packet core instance's **Technology type** or **Version** field.
    >
    > - To change the technology type, you'll need to [delete the site](delete-a-site.md) and [recreate it](create-a-site.md).
    > - To change the version, [upgrade the packet core instance](upgrade-packet-core-azure-portal.md).


- If you want to make changes to the attached data networks, refer to [Collect data network values](collect-required-information-for-a-site.md#collect-data-network-values) to collect the new values and make sure they're in the correct format.
- Ensure you can sign in to the Azure portal using an account with access to the active subscription you used to create your private mobile network. This account must have the built-in Contributor or Owner role at the subscription scope.

## Plan a maintenance window

The following modifications will trigger a packet core reinstall, during which your service will be unavailable:

- Attaching a new or existing data network to the packet core instance.
- Detaching a data network from the packet core instance.
- Changing the packet core instance's custom location.

If you're making any of these changes, we recommend modifying your packet core instance during a maintenance window to minimize the impact on your service. The packet core reinstall will take approximately 45 minutes, but this time may vary between systems. You should allow up to two hours for the process to complete.

If you're making a change that doesn't trigger a reinstall, you can skip the next step and move to [Select the packet core instance to modify](#select-the-packet-core-instance-to-modify).

## Back up deployment information

The following list contains the data that will be lost over a packet core reinstall. If you're making a change that triggers a reinstall, back up any information you'd like to preserve; after the reinstall, you can use this information to reconfigure your packet core instance.

1. If you want to keep using the same credentials when signing in to [distributed tracing](distributed-tracing.md), save a copy of the current password to a secure location.
1. If you want to keep using the same credentials when signing in to the [packet core dashboards](packet-core-dashboards.md), save a copy of the current password to a secure location.
1. Any customizations made to the packet core dashboards won't be carried over the upgrade. Refer to [Exporting a dashboard](https://grafana.com/docs/grafana/v6.1/reference/export_import/#exporting-a-dashboard) in the Grafana documentation to save a backed-up copy of your dashboards.
1. Most UEs will automatically re-register and recreate any sessions after the upgrade completes. If you have any special devices that require manual operations to recover from a packet core outage, gather a list of these UEs and their recovery steps.

## Select the packet core instance to modify

In this step, you'll navigate to the **Packet Core Control Plane** resource representing your packet core instance.

1. Sign in to the [Azure portal](https://portal.azure.com/).
2. Search for and select the **Mobile Network** resource representing the private mobile network.

    :::image type="content" source="media/mobile-network-search.png" alt-text="Screenshot of the Azure portal. It shows the results of a search for a Mobile Network resource.":::

3. In the **Resource** menu, select **Sites**.
4. Select the site containing the packet core instance you want to modify.
5. Under the **Network function** heading, select the name of the **Packet Core Control Plane** resource shown next to **Packet Core**.

    :::image type="content" source="media/packet-core-field.png" alt-text="Screenshot of the Azure portal showing the Packet Core field.":::

6. Select **Modify packet core**.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-configuration.png" alt-text="Screenshot of the Azure portal showing the Modify packet core option.":::

7. Choose the next step:
   - If you want to make changes to the packet core configuration or access network values, go to [Modify the packet core configuration](#modify-the-packet-core-configuration).
   - If you want to configure a new or existing data network and attach it to the packet core instance, go to [Attach a data network](#attach-a-data-network).
   - If you want to make changes to a data network that's already attached to the packet core instance, go to [Modify attached data network configuration](#modify-attached-data-network-configuration).

## Modify the packet core configuration

To modify the packet core and/or access network configuration:

1. If you haven't already, [select the packet core instance to modify](#select-the-packet-core-instance-to-modify).
2. In the **Configuration** tab, fill out the fields with any new values.
  
   - Use the information you collected in [Collect packet core configuration values](collect-required-information-for-a-site.md#collect-packet-core-configuration-values) for the top-level configuration values.
   - Use the information you collected in [Collect access network values](collect-required-information-for-a-site.md#collect-access-network-values) for the configuration values under **Access network**.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-configuration-tab.png" alt-text="Screenshot of the Azure portal showing the Modify packet core Configuration tab.":::

3. Choose the next step:
   - If you've finished modifying the packet core instance, go to [Submit and verify changes](#submit-and-verify-changes).
   - If you want to configure a new or existing data network and attach it to the packet core instance, go to [Attach a data network](#attach-a-data-network).
   - If you want to make changes to a data network that's already attached to the packet core instance, go to [Modify attached data network configuration](#modify-attached-data-network-configuration).

## Attach a data network

To configure a new or existing data network and attach it to your packet core instance:

1. If you haven't already, [select the packet core instance to modify](#select-the-packet-core-instance-to-modify).
2. Select the **Data networks** tab.
3. Select **Attach data network**.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-data-networks-attach.png" alt-text="Screenshot of the Azure portal showing the Modify packet core Data networks tab. The option to attach a data network is highlighted.":::

4. In the **Data network** field, choose an existing data network from the dropdown or select **Create new** to create a new one. Use the information you collected in [Collect data network values](collect-required-information-for-a-site.md#collect-data-network-values) to fill out the remaining fields.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-attach-data-network.png" alt-text="Screenshot of the Azure portal showing the Attach data network screen.":::

5. Select **Attach**.
6. Repeat the steps above for each additional data network you want to configure.
7. Choose the next step:
   - If you've finished modifying the packet core instance, go to [Submit and verify changes](#submit-and-verify-changes).
   - If you want to make changes to a data network that's already attached to the packet core instance, go to [Modify attached data network configuration](#modify-attached-data-network-configuration).

## Modify attached data network configuration

To make changes to a data network attached to your packet core instance:

1. If you haven't already, [select the packet core instance to modify](#select-the-packet-core-instance-to-modify).
2. Select the **Data networks** tab.
3. Select the data network you want to modify.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-data-networks-modify.png" alt-text="Screenshot of the Azure portal showing the Modify packet core Data networks tab. A data network is highlighted.":::

4. Use the information you collected in [Collect data network values](collect-required-information-for-a-site.md#collect-data-network-values) to fill out the fields in the **Modify attached data network** window.

    :::image type="content" source="media/modify-packet-core/modify-packet-core-modify-data-network.png" alt-text="Screenshot of the Azure portal showing the Modify attached data network screen.":::

5. Select **Modify**. You should see your changes under the **Data networks** tab.
6. Go to [Submit and verify changes](#submit-and-verify-changes).

## Submit and verify changes

1. Select **Modify**.
2. Azure will now redeploy the packet core instance with the new configuration. The Azure portal will display the following confirmation screen when this deployment is complete.

    :::image type="content" source="media/site-deployment-complete.png" alt-text="Screenshot of the Azure portal showing the confirmation of a successful deployment of a packet core instance.":::

3. Navigate to the **Packet Core Control Plane** resource as described in [Select the packet core instance to modify](#select-the-packet-core-instance-to-modify).

    - If you made changes to the packet core configuration, check that the fields under **Connected ASE device**, **Custom ARC location** and **Access network** contain the updated information.
    - If you made changes to the attached data networks, check that the fields under **Data networks** contain the updated information.

## Restore backed up deployment information

If you made changes that triggered a packet core reinstall, reconfigure your deployment using the information you gathered in [Back up deployment information](#back-up-deployment-information).

1. Follow [Access the distributed tracing web GUI](distributed-tracing.md#access-the-distributed-tracing-web-gui) to restore access to distributed tracing.
1. Follow [Access the packet core dashboards](packet-core-dashboards.md#access-the-packet-core-dashboards) to restore access to your packet core dashboards.
1. If you backed up any packet core dashboards, follow [Importing a dashboard](https://grafana.com/docs/grafana/v6.1/reference/export_import/#importing-a-dashboard) in the Grafana documentation to restore them.
1. If you have UEs that require manual operations to recover from a packet core outage, follow their recovery steps.

## Next steps

Use Log Analytics or the packet core dashboards to confirm your packet core instance is operating normally after you modify it.

- [Monitor Azure Private 5G Core with Log Analytics](monitor-private-5g-core-with-log-analytics.md)
- [Packet core dashboards](packet-core-dashboards.md)
