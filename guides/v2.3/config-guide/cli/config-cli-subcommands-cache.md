---
group: configuration-guide
title: Manage the cache
functional_areas:
  - Configuration
  - System
  - Setup
---

{% include config/cli-intro.md %}

## Overview of cache types {#config-cli-subcommands-cache-clean-over}

Magento 2 has the following {% glossarytooltip 0bc9c8bc-de1a-4a06-9c99-a89a29c30645 %}cache{% endglossarytooltip %} types:

<table>
  <tbody>
    <tr>
      <th>Cache type "friendly" name</th>
      <th>Cache type code name</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><p>Configuration</p></td>
      <td><p>config</p></td>
      <td><p>Magento collects configuration from all modules, merges it, and saves the merged result to the cache. This cache also contains store-specific settings stored in the file system and database. </p>
	<p>Clean or flush this cache type after modifying configuration files.</p></td>
    </tr>
    <tr>
      <td><p>Layout</p></td>
      <td><p>layout</p></td>
      <td><p>Compiled page layouts (that is, the layout components from all components).</p>
	<p>Clean or flush this cache type after modifying layout files.</p></td>
    </tr>
    <tr>
      <td><p>Block HTML output</p></td>
      <td><p>block_html</p></td>
      <td><p>HTML page fragments per block.</p>
	<p>Clean or flush this cache type after modifying the view layer.</p></td>
    </tr>
    <tr>
      <td><p>Collections data</p></td>
      <td><p>collections</p></td>
      <td><p>Results of database queries.</p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type if your custom module uses logic that results in cache entries that Magento cannot clean.</p></td>
    </tr>
    <tr>
      <td><p>DDL</p></td>
      <td><p>db_ddl</p></td>
      <td><p>Database schema. </p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type after you make custom changes to the database schema. (In other words, updates that Magento does not make itself.)</p>
      	<p>One way to update the database schema automatically is using the <a href="{{ page.baseurl }}/install-gde/install/cli/install-cli-subcommands-db.html">magento setup:db-schema:upgrade</a> command.</p></td>
    </tr>
    <tr>
      <td><p>Compiled Config</p></td>
      <td><p> compiled_config </p></td>
      <td><p>Compilation configuration</p></td>
    </tr>
    <tr>
      <td><p>Entity attribute value (EAV)</p></td>
      <td><p>eav</p></td>
      <td><p>Metadata related to EAV attributes (for example, store labels, links to related PHP code, attribute rendering, search settings, and so on).</p>
        <p>You should not typically need to clean or flush this cache type.</p></td>
    </tr>
    <tr>
      <td><p>Page cache</p></td>
      <td><p>full_page</p></td>
      <td><p>Generated HTML pages. </p>
      	<p>If necessary, Magento cleans up this cache automatically, but third-party developers can put any data in any segment of the cache. </p>
      	<p>Clean or flush this cache type after modifying code level that affects HTML output. It’s recommended to keep this cache enabled because caching HTML improves performance significantly.</p></td>
    </tr>
    <tr>
      <td><p>Reflection</p></td>
      <td><p>reflection</p></td>
      <td>Removes a dependency between the Webapi module and the Customer module.</td>
    </tr>
    <tr>
      <td><p>Translations</p></td>
      <td><p>translate</p></td>
      <td><p>After merging translations from all modules, the merger cache will be cleaned.</p></td>
    </tr>
    <tr>
      <td><p>Integration configuration</p></td>
      <td><p>config_integration</p></td>
      <td><p>Compiled integrations.</p>
      	<p>Clean or flush this cache after changing or adding integrations.</p></td>
    </tr>
    <tr>
      <td><p>Integration API configuration</p></td>
      <td><p>config_integration_api</p></td>
      <td><p>Compiled integration APIs configuration of the Store’s Integrations.</p></td>
    </tr>
    <tr>
      <td><p>Web services configuration</p></td>
      <td><p>config_webservice</p></td>
      <td><p>Caching the Web API Structure.</p></td>
    </tr>
    <tr>
      <td><p>Customer Notification</p></td>
      <td><p>customer_notification</p></td>
      <td><p>Temporary notifications that appear in the user interface.</p></td>
    </tr>
    <tr>
      <td><p>Vertex tax calculation data</p></td>
      <td><p>vertex</p></td>
      <td><p>Compiled tax calculation data in the shopping cart based on the tax profile of each product.</p></td>
    </tr>
  </tbody>
</table>

## View the cache status

To view the status of the cache, enter

	bin/magento cache:status

<!-- where `--bootstrap=` is a URL-encoded associative array of Magento [application bootstrap parameters]({{ page.baseurl }}/config-guide/bootstrap/magento-how-to-set.html#config-bootparam-overview) and values. -->

A sample follows:

                config: 1
                layout: 1
            block_html: 1
           collections: 1
                db_ddl: 1
       compiled_config: 1
                   eav: 1
             full_page: 1
             translate: 1
    config_integration: 1
config_integration_api: 1
 customer_notification: 1
     config_webservice: 1
                vertex: 1

## Enable or disable cache types {#config-cli-subcommands-cache-en}

This command enables you to enable or disable all cache types or only the ones you specify. Disabling cache types is useful during development because you see the results of your changes without having to flush the cache; however, disabling cache types has an adverse effect on performance.

{:.bs-callout .bs-callout-info}
Starting in version 2.2, you can enable or disable cache types _only_ using the command line. Before doing so, you must manually make `<your Magento install dir>/app/etc/env.php` writeable by the [Magento file system owner]({{ page.baseurl }}/install-gde/prereq/file-system-perms.html).

You can clean (also referred to as _flush_ or _refresh_) cache types using either the command line or the {% glossarytooltip 18b930cf-09cc-47c9-a5e5-905f86c43f81 %}Magento Admin{% endglossarytooltip %}.
</div>

Command options:

	bin/magento cache:enable [type] ... [type]
	bin/magento cache:disable [type] ... [type]

Where omitting `[type]` enables or disables all cache types at the same time. The `type` option is a space-separated list of cache types.

<!-- `--bootstrap=` is a URL-encoded associative array of Magento [application bootstrap parameters]({{ page.baseurl }}/config-guide/bootstrap/magento-how-to-set.html#config-bootparam-overview) and values. -->

To list cache types and their status:

	bin/magento cache:status

For example, to disable the full page cache and the DDL cache:

	bin/magento cache:disable db_ddl full_page

Sample result:

	Changed cache status:
                        db_ddl: 1 -> 0
                     full_page: 1 -> 0

{:.bs-callout .bs-callout-info}
Enabling a {% glossarytooltip 65f9a5a1-79ee-4f27-aac7-29abe24db40d %}cache type{% endglossarytooltip %} automatically clears that cache type.

## Clean and flush cache types {#config-cli-subcommands-cache-clean}

To purge out-of-date items from the cache, you can *clean* or *flush* cache types:

-   Cleaning a cache type deletes all items from enabled Magento cache types only. In other words, this option does not affect other processes or applications because it cleans only the cache that Magento uses.

    Disabled cache types are not cleaned.

-   Flushing a cache type purges the cache storage, which might affect other processes applications that are using the same storage.

Flush cache types if you've already tried cleaning the cache and you're still having issues that you cannot isolate.

Command usage:

	bin/magento cache:clean [type] ... [type]
	bin/magento cache:flush [type] ... [type]

Where `[type]` is a space-separated list of cache types. Omitting `[type]` cleans or flushes all cache types at the same time. For example, to flush all cache types, enter

	bin/magento cache:flush

Sample result:

	Flushed cache types:
	config
	layout
	block_html
	collections
	db_ddl
	compiled_config
	customer_notification
	eav
	full_page
	translate
	config_integration
	config_integration_api
	config_webservice
	vertex

{:.bs-callout .bs-callout-info}
You can also clean and flush cache types in the {% glossarytooltip 18b930cf-09cc-47c9-a5e5-905f86c43f81 %}Magento Admin{% endglossarytooltip %}. Go to **System** > **Tools** > **Cache Management**. **Flush Cache Storage** is equivalent to `bin/magento cache:flush`. **Flush Magento Cache** is equivalent to `bin/magento cache:clean`.
