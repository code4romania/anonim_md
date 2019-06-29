# Anonim.md

[Anonim.md](https://anonim.md) is a [GlobaLeaks](https://github.com/globaleaks/GlobaLeaks) instance that provides secure and anonymous communication between whistleblowers and journalists from Republic of Moldova.

This repository provides the instructions and the asset files for setting a custom homepage for a GlobaLeaks instance.

### Setting a custom homepage in Globaleaks 3.x
Globaleaks version 2.x used to offer a mean to set a custom homepage (or custom content, for that matter) from the administration panel. With version 3.x, this functionality has changed to provide more flexibility; the latest versions of the platform allows further customization through user-provided CSS and JavaScript
files that are automatically loaded in every GlobaLeaks page.

Mainly, there are two approaches for adding new content or setting your own homepage in GlobaLeaks 3.x:

1. Modify the default page by manipulating the DOM through the custom script. This is the method currently employed in Anonim. For more details please have a look at the commented code of [Anonim's custom script](asses/custom.js). 

2. Build a separate HTML page, upload it on the platform then use redirects using the custom script to set it as the landing page.

While the first method allows to integrate your own content with the GlobaLeaks submission forms within the same page, it is prone to break if elements/attributes are being changed in future releases of Globaleaks, thus the second approach is always prefered.

### Critical security aspects to be looked for
Since anonymity is the top priority for GlobaLeaks instances, every single asset used by a deployment **must** be served from the instance's host system. That means that under no condition should resources that are automatically requested by the browser (such as CSS/JS files, images, fonts and so on) be served from other domain/IP address/CDN.

The right way to do it is to download the resource locally, upload it on the instance then link it in your pages using a relative href (e.g `/s/header_image.png`).

### Steps to replicate the deployment of anonim.md
On a fresh Ubuntu box (version 18.04 preferred) do the following:

1. Install a fresh instance of Globaleaks using the [official documentation](https://docs.globaleaks.org/en/latest/setup/InstallationGuide.html);

2. Set up the platform and the administrator user using the [wizard](https://docs.globaleaks.org/en/latest/setup/PlatformWizard.html);

3. Log in to the administration panel (located at `/admin`) and upload the following [assets](assets/):
    - files under [assets/files](assets/files) to `Site settings > Files`
    - `custom.css`, `custom.js`, `favicon.ico` to `Site settings > Theme customization`
    - `logo.png` to `Site settings > Main configuration > Logo`

4. You're done.
