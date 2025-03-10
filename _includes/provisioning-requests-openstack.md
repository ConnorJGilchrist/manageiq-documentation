Create a request to provision Red Hat OpenStack Platform cloud instances from images, volumes, and volume snapshots using {{ site.data.product.title_short }}. Only bootable volumes not in use willbe available.

1. Browse to menu: **Compute > Clouds > Instances**

2. Click ![2007](../images/2007.png) **Lifecycle**, then click
   ![1862](../images/1862.png) **Provision Instances**.

3. Select an OpenStack image, volume or volume snapshot from the list presented.
   These files must be available on your OpenStack provider.

4. Click **Continue**.

5. On the **Request** tab, enter information about this provisioning request.
   In **Request Information**, type in at least an email address. This email is used to
   send the requester status emails during the provisioning process for items such as
   auto-approval, quota, provision complete, retirement, request pending approval, and
   request denied. The other information is optional. If the
   {{ site.data.product.title_short }} Server is configured to use LDAP, you can use the
   **Look Up** button to populate the other fields based on the email address.

    **Note:**

    Parameters with a \* next to the label are required to submit the provisioning request.
    To change the required parameters, see [Customizing Provisioning Dialogs](#customizing-provisioning-dialogs).

6. Click the **Purpose** tab to select the appropriate tags for the provisioned instance.

7. Click the **Catalog** tab for basic instance options.

    1. To change the source file to use as a basis for the instance, select it from the list of images, volumes, or volume snapshots.

    2. Select the **Number of Instances** to provision.

    3. Type a **Instance Name** and **Instance Description**.

8. Click the **Environment** tab to select the instance’s **Cloud Tenant**,
   **Availabilty Zones**, **Cloud Network**, **Security Groups**, and **Public IP Address**.
   If no specific Cloud Tenant is required, select the **Choose Automatically** checkbox.

9. Click the **Properties** tab to set provider options such as flavors and security settings.

    1. Select a flavor from the **Instance Type** list.

    2. Select a **Guest Access Key Pair** for access to the instance. For more information about key pairs, see [Managing Key Pairs](#managing-key-pairs).

10. Click the **Volumes** tab to provision any volumes with the instance. Volumes are
    useful for augmenting ephemeral storage of instances with persistent, general-purpose
    block storage:

    1. Fill in the **Volume Name** and **Size (gigabytes)** fields.

    2. If you want the volume to be deleted once the instance terminates (thereby making it non-persistent), check **Delete on Instance Terminate**.

    3. To provision and add multiple volumes to the instance, click **Add Volume**. Doing so will add new fields you can fill in.

        For more information about persistent storage in OpenStack, see the Red Hat OpenStack Platform *Storage Guide*.

11. Click the **Customize** tab to set additional instance options.

    1. Under **Credentials**, enter a **Root Password** for the **root** user access to the instance.

    2. Enter a **IP Address Information** for the instance. Leave as **DHCP** for automatic IP assignment from the provider.

    3. Enter any **DNS** information for the instance if necessary.

    4. Select a **Customize Template** for additional instance configuration. Select from the Cloud-Init scripts stored on your appliance.

12. Click the **Schedule** tab to set the provisioning and retirement date and time.

    1. In **Schedule Info**, choose whether the provisioning begins upon approval, or at a specific time. If you select **Schedule**, you will be prompted to enter a date and time.

    2. In **Lifespan**, select whether to power on the instances after they are created, and whether to set a retirement date. If you select a retirement period, you will be prompted for when to receive a retirement warning.

13. Click **Submit**.

The provisioning request is sent for approval. For the provisioning to begin, a user with
the admin, approver, or super admin account role must approve the request. The admin and
super admin roles can also edit, delete, and deny the requests. You will be able to see all
provisioning requests where you are either the requester or the approver.

After submission, the appliance assigns each provision request a **Request ID**. If an
error occurs during the approval or provisioning process, use this ID to locate the request
in the appliance logs. The Request ID consists of the region associated with the request
followed by the request number. As regions define a range of one trillion database IDs,
this number can be several digits long.

**Request ID Format**

Request 99 in region 123 results in Request ID 123000000000099.
