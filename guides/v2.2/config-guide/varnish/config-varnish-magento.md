---
layout: default
group: config-guide
subgroup: 09_Varnish
title: Configure Magento to use Varnish
menu_title: Configure Magento to use Varnish
menu_order: 15
menu_node:
version: 2.2
github_link: config-guide/varnish/config-varnish-magento.md
---
## Configure Magento to use Varnish {#config-varnish-magento}

To configure Magento to use Varnish:

1.	Log in to the {% glossarytooltip 18b930cf-09cc-47c9-a5e5-905f86c43f81 %}Magento Admin{% endglossarytooltip %} as an administrator.
2.	Click **STORES** > **Configuration** > ADVANCED > **System** > **Full Page Cache**
3.	From the **Caching Application** list, click **Varnish Caching**
4.	Enter a value in the **TTL for public content** field.
5.	Expand **Varnish Configuration** and enter the following information:

	<table>
	<col width="30%">
  	<col width="70%">
	<tbody>
		<tr>
			<th>Field</th>
			<th>Description</th>
		</tr>
	<tr>
		<td>Access list</td>
		<td><p>Enter the fully qualified host name, IP address, or <a href="https://www.digitalocean.com/community/tutorials/understanding-ip-addresses-subnets-and-cidr-notation-for-networking" target="_blank">Classless Inter-Domain Routing (CIDR)</a> notation IP address range for which to invalidate content.</p>
			<p><a href="https://www.varnish-cache.org/docs/3.0/tutorial/purging.html" target="_blank">More information</a></p></td>
	</tr>
	<tr>
		<td>Backend host</td>
		<td><p>Enter the fully qualified host name or IP address and listen port of the Varnish <em>backend</em> or <em>origin server</em>; that is, the server providing the content Varnish will accelerate. Typically, this is your web server. </p>
		<p><a href="https://www.varnish-cache.org/docs/trunk/users-guide/vcl-backends.html" target="_blank">More information</a></p></td>
	</tr>
	<tr>
		<td>Backend port</td>
		<td>Origin server's listen port.</td>
	</tr>
	<tr>
		<td>Grace period</td>
		<td>The grace period determines how long Varnish serves stale content if the backend is not responsive. The default value is 300 seconds.</td>
		</tr>
		</tbody>
		</table>

6.	Click **Save Config**.
7.	Click one of the export buttons to create a <code>varnish.vcl</code> you can use with Varnish.

	For example, if you have Varnish 4, click **Export VCL for Varnish 4**

	The following figure shows an example.<br><br>
	<img src="{{ site.baseurl }}common/images/config_varnish_admin_22.png" alt="Configure Magento to use Varnish in the Admin">

8.	Back up your existing <code>default.vcl</code>. Then rename the <code>varnish.vcl</code> file you just exported to <code>default.vcl</code>. Then copy the file to the <code>/etc/varnish/</code>. directory.

		cp /etc/varnish/default.vcl /etc/varnish/default.vcl.bak2
		mv <download_directory>/varnish.vcl default.vcl
		cp <download_directory>/default.vcl /etc/varnish/default.vcl
9.	We recommend you open `default.vcl` and change the value of `acl purge` to the IP address of the Varnish host. (You can specify multiple hosts on separate lines or you can use CIDR notation as well.)

	For example,

		acl purge {
		   "localhost";
		}

10. If you want to customize the Vagrant health checks or grace mode or saint mode configuration, see [Advanced Varnish configuration]({{page.baseurl}}config-guide/varnish/config-varnish-advanced.html).

11.	Restart Varnish and your web server:

		service varnish restart
		service httpd restart

## Cache Static Files

Static files should not be cached by default, but if you want to cache them, you can edit the section `Static files caching` in the VCL to have the following content:

```
# Static files should not be cached by default
  return (pass);

# But if you use a few locales and don't use CDN you can enable caching static files by commenting previous line (#return (pass);) and uncommenting next 3 lines
  #unset req.http.Https;
  #unset req.http./* {{ ssl_offloaded_header }} */;
  #unset req.http.Cookie;
```

You need to make these changes before you configure Magento to use Varnish.

## Next steps
<p><a href="{{page.baseurl}}config-guide/varnish/config-varnish-advance.html">Advanced Varnish configuration</a> (Optional)</p>
<p><a href="{{page.baseurl}}config-guide/varnish/config-varnish-final.html">Final verification</a></p>
