# Configuring {{ site.data.product.title_short }}

After installing {{ site.data.product.title_short }} and running
it for the first time, you must perform some basic
configuration. To configure {{ site.data.product.title_short }},
you must at a minimum:

1. Add a disk to the infrastructure hosting your appliance.

2. Configure the database.

Configure the {{ site.data.product.title_short }} appliance
using the internal appliance console.

## Accessing the Appliance Console

1. Start the appliance and open a terminal console.

2. Enter the `appliance_console` command. The
   {{ site.data.product.title_short }} appliance summary
   screen displays.

3. Press `Enter` to manually configure settings.

4. Press the number for the item you want to change, and press
   `Enter`. The options for your selection are displayed.

5. Follow the prompts to make the changes.

6. Press `Enter` to accept a setting where applicable.

**Note:**

The {{ site.data.product.title_short }} appliance console
automatically logs out after five minutes of inactivity.

## Configuring a Database

{{ site.data.product.title_short }} uses a database to store
information about the environment. Before using
{{ site.data.product.title_short }}, configure the database
options for it; {{ site.data.product.title_short }} provides the
following two options for database configuration:

- Install an internal PostgreSQL database to the appliance

- Configure the appliance to use an external PostgreSQL database

## Configuring an Internal Database

{% include configuration-db.md %}

## Configuring an External Database

Based on your setup, you will choose to configure the appliance
to use an external PostgreSQL database. For example, we can only
have one database in a single region. However, a region can be
segmented into multiple zones, such as database zone, user
interface zone, and reporting zone, where each zone provides a
specific function. The appliances in these zones must be
configured to use an external database.

The `postgresql.conf` file used with
{{ site.data.product.title_short }} databases requires specific
settings for correct operation. For example, it must correctly
reclaim table space, control session timeouts, and format the
PostgreSQL server log for improved system support. Due to these
requirements, Red Hat recommends that external
{{ site.data.product.title_short }} databases use a
`postgresql.conf` file based on the standard file used by the
{{ site.data.product.title_short }} appliance.

Ensure you configure the settings in the `postgresql.conf` to
suit your system. For example, customize the `shared_buffers`
setting according to the amount of real storage available in
the external system hosting the PostgreSQL instance. In
addition, depending on the aggregate number of appliances
expected to connect to the PostgreSQL instance, it may be
necessary to alter the `max_connections` setting.

**Note:**

- {{ site.data.product.title_short }} requires PostgreSQL
  version 9.5.

- Because the `postgresql.conf` file controls the operation of
  all databases managed by a single instance of PostgreSQL, do
  not mix {{ site.data.product.title_short }} databases with
  other types of databases in a single PostgreSQL instance.

1. Start the appliance and open a terminal console.

2. Enter the `appliance_console` command. The
   {{ site.data.product.title_short }} appliance summary screen
   displays.

3. Press **Enter** to manually configure settings.

4. Select **Configure Application** from the menu.

5. You are prompted to create or fetch a security key.

    - If this is the first {{ site.data.product.title_short }}
      appliance, choose **Create key**.

    - If this is not the first
      {{ site.data.product.title_short }} appliance, choose
      **Fetch key from remote machine** to fetch the key from
      the first appliance.

        **Note:**

        All {{ site.data.product.title_short }} appliances in a
        multi-region deployment must use the same key.

6. Choose **Create Region in External Database** for the
   database location.

7. Enter the database hostname or IP address when prompted.

8. Enter the database name or leave blank for the default
   (`vmdb_production`).

9. Enter the database username or leave blank for the default
   (`root`).

10. Enter the chosen database user’s password.

11. Confirm the configuration if prompted.

{{ site.data.product.title_short }} will then configure the
external database.

### Configuring a Worker Appliance

You can use multiple appliances to facilitate horizontal
scaling, as well as for dividing up work by roles. Accordingly,
configure an appliance to handle work for one or many roles,
with workers within the appliance carrying out the duties for
which they are configured. You can configure a worker appliance
through the terminal. The following steps demonstrate how to
join a worker appliance to an appliance that already has a
region configured with a database.

1. Start the appliance and open a terminal console.

2. Enter the `appliance_console` command. The
   {{ site.data.product.title_short }} appliance summary screen
   displays.

3. Press **Enter** to manually configure settings.

4. Select **Configure Application** from the menu.

5. You are prompted to create or fetch a security key. Since
   this is not the first {{ site.data.product.title_short }}
   appliance, choose **2) Fetch key from remote machine**. For
   worker and multi-region setups, use this option to copy the
   security key from another appliance.

    **Note:**

    All {{ site.data.product.title_short }} appliances in a
    multi-region deployment must use the same key.

6. Choose **Join Region in External Database** for the database
   location.

7. Enter the database hostname or IP address when prompted.

8. Enter the port number or leave blank for the default
   (`5432`).

9. Enter the database name or leave blank for the default
   (`vmdb_production`).

11. Enter the database username or leave blank for the default
    (`root`).

12. Enter the chosen database user’s password.

13. Confirm the configuration if prompted.
