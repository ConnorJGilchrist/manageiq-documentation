1. Browse to menu: **Compute > Containers > Providers**.

2. Click **Configuration**, then click
   ![Add a New Containers Provider](../images/1862.png) **Add a New Containers Provider**.

3. Enter a **Name** for the provider.

4. From the **Type** list, select **OpenShift Container Platform**.

5. Enter the appropriate **Zone** for the provider. If you do not
   specify a zone, it is set to `default`.

6. In the **Default** tab, configure the following for the OpenShift provider:

    1. Select a **Security Protocol** method to specify how to authenticate the provider:

        - **SSL**: Authenticate the provider securely using a trusted Certificate Authority. Select this option if the provider has a valid SSL certificate and it is signed by a trusted Certificate Authority. No further configuration is required for this option.

        - **SSL trusting custom CA**: Authenticate the provider with a self-signed certificate. For this option, copy your provider’s CA certificate to the **Trusted CA Certificates** box in PEM format.

        **Note:**

        You can obtain your OpenShift Container Platform provider’s CA certificate for all endpoints (default, metrics, alerts) from `/etc/origin/master/ca.crt`. Paste the output (a block of text starting with `-----BEGIN CERTIFICATE-----`) into the **Trusted CA Certificates** field.

        - **SSL without validation**: Authenticate the provider insecurely (not recommended).

    2. Enter the **Hostname** (or IPv4 or IPv6 address) of the provider.

        **Important:**

        The **Hostname** must use a unique fully qualified domain name.

    3. Enter the **API Port** of the provider. The default port is `8443`.

    4. Enter a token for your provider in the **Token** box.

        **Note:**

        To obtain a token for your provider, run the `oc get secret` command on your provider; see
        [Obtaining an OpenShift Container Platform Management Token](../managing_providersindexhtml#obtaining-an-openshift-container-platform-management-token).

        For example:

        \# oc get secret --namespace management-infra
        management-admin-token-8ixxs --template='{{index .data
        "ca.crt"}}' | base64 --decode

    5. Click **Validate** to confirm that {{ site.data.product.title_short }} can connect to the OpenShift Container Platform provider.

7. In the **Alerts** endpoint tab optionally configure the alerts service:

    1. Select a service type, or leave disabled.  Prometheus alerts are only supported on OCP v3

    2. Select a **Security Protocol** method to specify how to authenticate the service:

        - **SSL**: Authenticate the provider securely using a trusted Certificate Authority. Select this option if the provider has a valid SSL certificate and it is signed by a trusted Certificate Authority. No further configuration is required for this option.

        - **SSL trusting custom CA**: Authenticate the provider with a self-signed certificate. For this option, copy your provider’s CA certificate to the **Trusted CA Certificates** box in PEM format.

        - **SSL without validation**: Authenticate the provider insecurely using SSL. (Not recommended)

    3. Enter the **Hostname** (or IPv4 or IPv6 address) or alert **Route**.

    4. Enter the **API Port** if your Prometheus provider uses a non-standard port for access. The default port is `443`.

    5. Click **Validate** to confirm that {{ site.data.product.title_short }} can connect to the alerts service.

8. In the **Metrics** endpoint tab optionally configure the metrics service details:

    1. Select a service type (Prometheus or Hawkular) or leave disabled

    2. Select a **Security Protocol** method to specify how to authenticate the service:

        - **SSL**: Authenticate the provider securely using a trusted Certificate Authority. Select this option if the provider has a valid SSL certificate and it is signed by a trusted Certificate Authority. No further configuration is required for this option.

        - **SSL trusting custom CA**: Authenticate the provider with a self-signed certificate. For this option, copy your provider’s CA certificate to the **Trusted CA Certificates** box in PEM format.

        **Note:**

        In OpenShift, the default deployment of the router generates certificates during installation, which can be used with the **SSL trusting custom CA** option. Connecting a Hawkular endpoint with this option requires the CA certificate that the cluster uses for service certificates, which is stored in `/etc/origin/master/service-signer.crt` on the first master in a cluster.

        - **SSL without validation**: Authenticate the provider insecurely using SSL. (Not recommended)

    3. Enter the **Hostname** (or IPv4 or IPv6 address) of the metrics service.

        In order to find the hostname you can use the `oc get route` command.

        On OCPv4 you can get the Prometheus route by running:

        `oc get route prometheus-k8s -n openshift-monitoring`

        Example:
        ```
        oc get route prometheus-k8s -n openshift-monitoring
        NAME             HOST/PORT                                            PATH             SERVICES  PORT      TERMINATION          WILDCARD
        prometheus-k8s   prometheus-k8s-openshift-monitoring.ocp.example.com  prometheus-k8s   web       reencrypt/Redirect   None
        ```

        In this example you would use `prometheus-k8s-openshift-monitoring.ocp.example.com` as the metrics hostname.

        On OCPv3 you can get the Hawkular route by running:

        `oc get route hawkular-metrics -n openshift-infra`

        Or the Prometheus route:

        `oc get route prometheus -n openshift-metrics`

    4. Enter the **API Port** if your Hawkular or Prometheus provider uses a non-standard port for access. The default port is `443`.

    5. Click **Validate** to confirm that {{ site.data.product.title_short }} can connect to the metrics endpoint.

9. Click the **Advanced** tab to add image inspector settings for scanning container images onyour provider using OpenSCAP.

    **Note:**

    - These settings control downloading the image inspector container image from the registry and obtaining the Common Vulnerabilities and Exposures (CVE) information (for effective scanning) via a proxy.

    - CVE URL that {{ site.data.product.title_short }} requires to be open for OpenSCAP scanning: <https://www.redhat.com/security/data/metrics/ds/>. This information is based on the source code of OpenSCAP.

    1. Enter the proxy information for the provider in either **HTTP**, **HTTPS**, or **NO Proxy** depending on your environment.

    2. Enter the **Image-Inspector Repository** information. For example, `openshift3/image-inspector`.

    3. Enter the **Image-Inspector Registry** information. For example, `registry.access.redhat.com`.

    4. Enter the **Image-Inspector Tag** value. A tag is a mark used to differentiate images in a repository, typically by the application version stored in the image.

    5. Enter `https://www.redhat.com/security/data/metrics/ds/` in **CVE location**.

10. Click **Add**.

**Note:**

You can also set global default image-inspector settings for all OpenShift providers in the
advanced settings menu by editing the values under `ems_kubernetes`, instead of setting this
for each provider.

For example:

    :image_inspector_registry: registry.access.redhat.com
    :image_inspector_repository: openshift3/image-inspector
