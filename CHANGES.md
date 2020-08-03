## caman changes

### Changelog

**0.5.1**, 2020-08-03: Updates from [Nathan Green](https://github.com/nathansgreen)
* Switch to gnu-sed (``gsed`) if macOS is detected.
* Add ``dump`` command to show cert in text format.
* Add ``localityName`` to default template.

**0.5.0**, 2018-06-07: Support client certificates
* Add support for signing and renewing client certificates with commands
 ``client_sign`` and ``client_renew`` from
 [Jiri Machalek](https://github.com/machalekj)
* PKCS12: include "friendly name" for 389ds from
 [Tiziano Müller](https://github.com/dev-zero)

**0.4.0**, 2018-03-06: Support new commands
* Added ``list`` command from [Tiziano Müller](https://github.com/dev-zero)
* Generate Java Keystore from [Tiziano Müller](https://github.com/dev-zero)
* Multiple bug fixes from [Tiziano Müller](https://github.com/dev-zero)
* Added ``crl`` command to regenerate the CRL
* Bug fix from [Tobias Johnson](https://github.com/irontoby)
* Added ``create`` command to copy config files before running ``init``
 from  [Wesley Hartford](https://github.com/wfhartford)

**0.3.2**, 2016-11-24: Fix from [TakeMeNL](https://github.com/TakeMeNL)
* Exit after "cannot renew" message

**0.3.1**, 2015-09-17: Fixes from [foudfou](https://github.com/foudfou)
* ``new`` accepts arguments for SAN certificates again
* Changed shebang for better compatibility
* Removed trailing whitespace

**0.3.0**, 2015-09-08: Add intermediate CA support
* See [Upgrading](#upgrading) below for upgrade instructions
* ``init`` command changed to accept optional path to root caman CA dir
* ``sign`` command changed to create combined chain/crt file
* ``revoke`` command changed to support revoking intermediate CAs
* ``caman`` script now detects and uses absolute paths, so can be installed
  system-wide
* Certificates are now signed with ``-notext`` so certificates are smaller and
  are accepted by more systems
* Password prompt is now managed by caman, so only needs to be entered once per
  command
* ``openssl`` commands are now run in non-interactive mode

**0.2.0**, 2015-08-21: Add SAN support
* See [Upgrading](#upgrading) below for upgrade instructions
* ``new`` command changed to replace OUN with alt hostnames to support SANs
* Version number now displayed when called with missing or incorrect arguments

**0.1.0**, 2015-05-16: Initial release


<a name="upgrading"></a>
### Upgrading

#### Upgrading from 0.2.0

Host certificates are now created with the ``-notext`` option to suppress the
human-readable text summary at the top of certificates, as it unnecessarily
increases their file size, and can apparently cause problems for some systems.
If you haven't had any problems there is no need to modify existing host
certificates, but you can ``renew`` host certificates to generate new ones
without the text summary, or manually remove the text summaries from your
``.crt.pem`` and ``.keycrt.pem`` files (the human-readable section which
starts ``Certificate``, before ``-----BEGIN CERTIFICATE-----``).

No other changes are required for this version; your existing CA can be used as
a root CA without any changes to its config, or to certificates which are
already in use.


#### Upgrading from 0.1.0

The file ``ca/caconfig.cnf.default`` has been changed, so you need to update
your customised ``caconfig/host.cnf``:
* The section ``[ CA_default ]`` has a new setting ``copy_extensions = copy``


The file ``ca/host.cnf.default`` has been changed, and your customised
``ca/host.cnf`` needs to be changed too:
* The ``organizationalUnitName`` now has to be set manually in this file;
  change ``<<OUN>>`` to an appropriate value for your certificates. This can
  be changed manually on a per-host basis once you have run the ``new`` command.
* In the section ``[ v3_req ]``, after the line ``keyUsage``, add the line
  ``<<ALT_HOSTNAMES>>``

The changes to ``ca/host.cnf`` do not affect the configuration for existing
hosts.

* The ``new`` command no longer accepts an OUN; this now has to be set manually
  in your ``ca/host.cnf``, and customised per-host as necessary after running
  ``new``.
