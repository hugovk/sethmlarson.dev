# Security Developer-in-Residence – Weekly Report #1

<blockquote>
  <center>This critical role would not be possible without funding from the <a href="https://alpha-omega.dev">OpenSSF Alpha-Omega Project</a>.<br>
  Massive thank-you to Alpha-Omega for investing in the security of the Python ecosystem!</center>
</blockquote>

Welcome to the first report from me fulfilling the role of Security Developer-in-Residence (SDIR).
Since this role serves the Python community I would like to make publishing these reports weekly
to give some insights into what I'm working on and thinking about.

This report covers a bit more time than a single week because I started the job on a short week
and was in [stealth mode](https://pyfound.blogspot.com/2023/06/announcing-our-new-security-developer.html)
until Thursday of last week so there was a limited amount of things and people I could engage with publicly.

## Meeting all the people!

The focus of the past week has been meeting all the lovely folks I'll be working closely with to
achieve [this role's mission](https://sethmlarson.dev/security-developer-in-residence#responsibilities-and-keys-to-success).
Below is a subset of the folks I met with or made introductions with:

- OpenSSF [Alpha-Omega](https://alpha-omega.dev/)
- Python Software Foundation [Staff](https://www.python.org/psf/records/staff/) and [Board](https://www.python.org/psf/board/)
- [Python Steering Council](https://peps.python.org/pep-8104/#results) and the [CPython Developer-in-Residence](https://lukasz.langa.pl/)
- [Python Security Response Team](https://www.python.org/dev/security/)
- [CPython Core Developers](https://discuss.python.org/t/introduction-from-security-developer-in-residence/28588/3)

To no surprise at all knowing the Python community: Everyone I've met has been very warm, welcoming, and excited to collaborate.
There are many familiar faces and existing relationships between me and the folks listed above which highlights one
of the big benefits for filling roles like the SDIR from existing community members.

## Python Security Response Team

After announcing myself and my mission to the CPython Core Developers I was quickly invited to join the Python Security Response Team (PSRT)
in order to help with triaging and responding to security reports for CPython. So now when you email `security@python.org` there's
a chance I'll be the one helping you out. I've also become a moderator of the mailing list to help with sorting through spam and invalid requests.

In addition to joining the team, I've been gathering feedback from members of the PSRT in order to improve the processes that
already exist. From listening to folks, I've heard three common topics and have put together a set of proposed changes to be discussed by PSRT members
and ultimately approved by the Python Steering Council. The three topics I've heard are:

- **CVE ID Assignment**: Currently we have no control over the CVEs that are issued for CPython which can result in delays and confusion when information on a CVE needs to be updated
  and doesn't allow for CPython core developers to provide input into the CVE process like providing remediation and other guidance alongside the CVE disclosure.
- **Follow-up and Collaboration**: Currently reports to the PSRT are managed entirely through the mailing list which as someone who's new or isn't frequently
  actively participating can make it tough to know right away what the statuses of each report is, whether it needs additional help or if it's been completed.
- **Transparency and Membership**: There currently is no public list of PSRT members and membership doesn't have any criteria beyond being added to the mailing list by an admin.
  Many projects will list the security contacts so everyone knows who receives security reports and so that the folks volunteering their time can be recognized.

Since the discussions are quite early on I will hold off on details until the next update where I provide more information.

## OpenSSF Working Groups

Part of the role is sharing the perspective of the Python ecosystem in working groups
like the ones hosted by the OpenSSF. I have identified the following meetings to contribute to regularly
and was able to attend most of them that occurred this week and introduce myself:

- [Securing Software Repositories WG](https://github.com/ossf/wg-securing-software-repos)
- [Securing Critical Projects WG](https://github.com/ossf/wg-securing-critical-projects)
- [Vulnerability Disclosures WG](https://github.com/ossf/wg-vulnerability-disclosures)
- [Supply Chain Integrity WG](https://github.com/ossf/wg-supply-chain-integrity)
- [Best Practices for Open Source Developers WG](https://github.com/ossf/wg-best-practices-os-developers)

### Securing Critical Projects WG

**Caleb Brown** shared a project on malicious package detections on multiple different
package repositories, including PyPI. The next stage of the plan is to start hooking this
data source up to data intakes.

I pointed Caleb towards the [recent funding](https://discuss.python.org/t/pypi-malware-detection-project/28222) that PyPI has received from the
[Center for Security and Emerging Technology](https://cset.georgetown.edu/)
in order to develop functionality around an API to accept malware reports
from trusted sources and to use this information for quicker resolution times
and potentially automated resolution via soft-deletes of releases.

The entire group was excited for this work and will be following closely.
Caleb will engage with the PyPI project through the [provided Google form](https://forms.gle/ixRoNJEPVNekFN7H7)
intended to gather information from groups that are already doing this sort of malware analysis.

### Securing Package Repositories WG

**Zach Steindler** presented a [draft guide](https://github.com/ossf/wg-securing-software-repos/pull/17) for helping other
package repositories add build provenance through [in-toto](https://in-toto.io/) and [SLSA](https://slsa.dev). This guide uses [NPM provenance as an example](https://github.blog/2023-04-19-introducing-npm-package-provenance/)
and shows how that work can be replicated by other repositories.

The draft guide also mentions that using OpenID Connect for authentication similar to PyPI's [Trusted Publishers](https://docs.pypi.org/trusted-publishers) are a great first step towards build provenance since
they don't require the use of Sigstore but build on the same primitives as would be needed for build provenance.

Even where there is no availability of OpenID Connect, like when building+publishing from your own computer, there is still
some attestation that the package repository can provide installers. For NPM this is called a "[publish attestation](https://github.com/npm/attestation/tree/main/specs/publish/v0.1)" and similarly uses in-toto
but only contains the package name (PURL), version, digest, and registry. This attestation doesn't provide provenance for the package.

## Trusted Publisher adoption

[Trusted Publishers](https://docs.pypi.org/trusted-publishers) are already a big operational security improvement for packages publishing to PyPI and as you've seen above may be
only the first step on being able to provide build provenance for Python packages. Due to this, I would like to drive adoption of
Trusted Publishers in as many Python packages as possible.

It's tough to improve what you're not measuring, so I've submitted my [first pull request to Warehouse](https://github.com/pypi/warehouse/pull/14044)
to add metrics for the number of projects that have configured and successfully used Trusted Publishers. My plan is to use this data to create public dashboards similar to
the 2FA adoption dashboards so we can track how adoption over time.

I also [created a proposal](https://github.com/pypa/gh-action-pypi-publish/issues/164) to nudge users of the popular
[pypa/gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish) GitHub Action to use Trusted Publishers since
most folks using GitHub Actions for releases can likely already adopt Trusted Publishers.

## Security news in the Python-verse

- [Manifest Confusion in NPM](https://blog.vlt.sh/blog/the-massive-hole-in-the-npm-ecosystem). This applies in some minor ways to Python packaging metadata. Will investigate this more.
- [Draft PR](https://github.com/pypi/warehouse/pull/13943) to integrate [Repository Service for TUF](https://github.com/repository-service-tuf/repository-service-tuf) with Warehouse.
- [PyPI Malware Detection project](https://discuss.python.org/t/pypi-malware-detection-project/28222) funded by [CSET](https://cset.georgetown.edu/)

That's all for this week! 👋 If you're interested in more you can read [next week's report](https://sethmlarson.dev/security-developer-in-residence-weekly-report-2).
