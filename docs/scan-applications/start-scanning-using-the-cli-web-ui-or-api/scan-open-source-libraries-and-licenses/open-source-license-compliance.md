# Open-source license compliance

{% hint style="info" %}
**Feature availability**\
Licenses are available for all paid plans. For details, see [pricing plans](https://snyk.io/plans/).
{% endhint %}

## Overview of licenses

Every time you test your code in the [Snyk Web UI](../../../getting-started/explore-snyk-through-the-web-ui.md), the [Snyk CLI](../../../snyk-cli/), or using [PR Checks](../../../scan-application-code/run-pr-checks/), your repositories are scanned not only for vulnerabilities but also for license compliance. This includes all of your direct and indirect dependencies. Snyk scans your manifest files and then checks for license issues against Snyk known licenses.

### Default license policy

To enable customers to get started with license compliance faster, Snyk created a default license policy. The default policy is a baseline that tries to meet the requirements of multiple types of applications, SaaS, distributed, and so on. It may be used as a starting point to calibrate additional license policies. The default policy does not endorse or criticize any license.

By default, Snyk determines the severity of license issue in the following way:

* High severity - licenses that definitely present issues for commercial software.
* Medium severity - licenses with clauses that may be of concern and should be reviewed.

For more information, see [License policies](../../../manage-issues/policies/license-policies/).

### How Snyk uses licenses

To facilitate the onboarding of your developers, Snyk recommends that your teams check these defaults, update severities, and add instructions according to license type based on the policies outlined specifically by your Legal teams.

After the license policy is updated, when Snyk detects a license violation, it is displayed for all users in the Organization in the test results on the [Snyk Web UI](../../../getting-started/explore-snyk-through-the-web-ui.md), the [Snyk CLI](../../../snyk-cli/), or [PR Checks](../../../scan-application-code/run-pr-checks/). in the same way as a security vulnerability, and including the severity and instructions you configured.

An example follows:

<div align="left"><figure><img src="../../../.gitbook/assets/image5 (2).png" alt="License card overview."><figcaption><p>License card overview</p></figcaption></figure></div>

### View and manage license policies

You can view an inventory of all of your licenses across all your Projects. Snyk also lists packages that have dual licenses and multiple licenses. For more information, see [View licenses](../../../manage-issues/dependencies-and-licenses/view-licenses.md).

Different customers may have different needs and tolerance for different license types. We encourage you to ensure you have made the needed changes or created new policies that fit the specific requirements of your company.

New licenses added by Snyk will inherit the **Unknown** license type severity. In cases where this severity was not set to **None**, newly-added licenses will appear in the licenses compliance results.

If you notice a license with the wrong license type assigned to it, you can contact Snyk support. Snyk will investigate the request and update the license type if needed.

## **Supported package managers**

{% hint style="warning" %}
Snyk does not support scanning for license issues for package versions that include a git commit hash.
{% endhint %}

* npm
* RubyGems
* Maven
* Pip
* Nuget
* Go
* Composer
* Cocoapods

## **Supported unmanaged ecosystems**

{% hint style="warning" %}
Snyk does not support scanning for license issues for package versions that include a git commit hash.
{% endhint %}

C/C++ is not supported for scanning for license issues.
