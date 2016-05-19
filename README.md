Summary
=======

This module replaces self-generated Aegir certificates with Let's Encrypt ones.

Initial version developed by @gboudrias for Advisor Websites. Improved later for BOA by @omega8cc -- Use at your own risk.

Requirements
============

This module works with Apache and Nginx. You must have the third-party letsencrypt.sh library uploaded (see Usage).

Usage
=====

This module uses [letsencrypt.sh](https://github.com/lukas2511/letsencrypt.sh) and makes very minimal assumptions and automates almost everything:

* Aegir and its config may live in any non-standard directory or in the canonical /var/aegir
* The certificate's name in Aegir (and thus its directory) is the same as the site's name
* Your letsencrypt.sh can be found at [aegir_root]/tools/le/letsencrypt.sh
* All site's aliases must have valid DNS names pointing to your server IP address
* Aliases redirection must be disabled if all aliases are expected to be includes as SAN names
* If aliases redirection is enabled, the certificate created will list only the main site name

This module will create all required directories it needs to operate on the first attempt to run site Verify task with SSL option enabled.

Note that this module will regenerate existing symlinks and existing Let's Encrypt key and certificate, if needed, like when you will update site's aliases or the certificate will have less than 30 days to expiration date. You can safely use another third-party's certificate if you so choose, replacing the key and certificates created by Let's Encrypt integration library, if you will create an empty control file: [aegir_root]/tools/le/.ctrl/dont-overwrite-example.com.pid

Likewise, you can simply put your private certificate in [aegir_root]/tools/le/certs/example.com, though you should know Aegir expects openssl.key and openssl.crt (or a symlink with these filenames) to exist, and this module will generate them for you.

To renew or update the Let's Encrypt certificate, with all aliases added as Subject Alternative Names (SAN), it is enough to run the Verify task on the site. It's planned to automate this procedure using different methods (done in BOA via automated sites Verify tasks).

To know more about letsencrypt.sh usage, go to https://github.com/lukas2511/letsencrypt.sh

General Notes
=============

The main important file here is drush/provision\_hosting\_le.drush.inc , which allows us to run code after the verify as the Aegir user. The www-data must not be granted access to letsencrypt.sh for security reasons.

The file hosting.feature.le.inc must be present for Aegir to load the Provision (Drush) extension, and will do so only if the module is enabled.

License
=======

This project is licensed under the GNU GENERAL PUBLIC LICENSE Version 2, see LICENSE.txt for details.
