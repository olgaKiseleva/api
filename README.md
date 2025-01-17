# [Vulners API v3](https://vulners.com) Python wrapper


# Description
Python 2/3 library for the [Vulners Database](https://vulners.com)
provides *search, data retrieval, archive and API's vulnerability scanning* for the integration purposes.
Create powerful security tools and get access to the world largest security database.

## Python version
Library was tested on *python2* and *python3*.

## How to install

Package is available with [PyPI](https://pypi.python.org/pypi) 

You can use pip for the installation

```bash
pip install -U vulners
```

## Obtaining Vulners API key

Please, register at [Vulners website](https://vulners.com).
Go to the personal menu by clicking on your name in the right top corner.
Follow "API KEYS" tab.
Generate an API key with scope "api" and use it with the library.

# Functions and methods

All the callable methods use [Vulners REST API](https://vulners.com/docs).

### Search in database
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
heartbleed_related = vulners_api.search("heartbleed", limit=10)
```
### Get information about document by identificator
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
CVE_2017_14174 = vulners_api.document("CVE-2017-14174")
```
### Get information about multiple documents by identificators
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
CVE_DATA = vulners_api.documentList(["CVE-2017-14174", "CVE-2016-1175"])
```
### Search for the public available exploits
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
wordpress_exploits = vulners_api.searchExploit("wordpress 4.7.0")
```
### Get vulnerabilities and exploits by software name and version
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")

results = vulners_api.softwareVulnerabilities("httpd", "1.3")
exploit_list = results.get('exploit')
vulnerabilities_list = [results.get(key) for key in results if key not in ['info', 'blog', 'bugbounty']]
```
### Get vulnerabilities by CPE product and version string
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")

cpe_results = vulners_api.cpeVulnerabilities("cpe:/a:cybozu:garoon:4.2.1")
cpe_exploit_list = cpe_results.get('exploit')
cpe_vulnerabilities_list = [cpe_results.get(key) for key in cpe_results if key not in ['info', 'blog', 'bugbounty']]
```
### Get references for the vulnerability
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
references = vulners_api.references("CVE-2014-0160")
```
### Get Windows KB superseeding and parentseeding information
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
# Superseeding information will be returned as dict
# with two fields: 'superseeds', 'parentseeds'.
# Superseeds means "what KB are covered by this KB".
# Parentseeds means "what KB are covering this KB".
superseeds = vulners_api.kbSuperseeds("KB4524135")
```
### Get Windows KB updates list and download urls
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
microsoft_updates_for_kb = vulners_api.kbUpdates("KB4524135")
updates_download_links = [update.get('href') for update in microsoft_updates_for_kb]
```
### Score any vulnerability description using [Vulners AI](https://lab.wallarm.com/new-from-wallarm-research-first-ai-based-tool-to-predict-vulnerability-risk-2d0a7e9b3474)
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
text_ai_score = vulners_api.aiScore("My cool vulnerability description")
```
### Get possible query autocompletions
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
possible_autocomplete = vulners_api.autocomplete("heartbleed")

```
### Download whole database collection and work with data locally
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
all_cve = vulners_api.archive("cve")
```
### Audit Windows hosts for installed security KB
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")
win_vulners = vulners_api.kbAudit(os="Windows Server 2012 R2", kb_list=["KB4072650", "KB2959936", "KB2894856", "KB2896496"])
need_2_install_kb = win_vulners['kbMissed']
affected_cve = win_vulners['cvelist']
```
### Audit Linux hosts for vulnerabilities (RPM/DEB based)
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")

# Example for CentOS 7
# You can use it for any RPM based OS
# Execute command: rpm -qa --qf '%{NAME}-%{VERSION}-%{RELEASE}.%{ARCH}\\n'
# Use it as package variable input

centos_vulnerabilities = vulners_api.audit(os= 'centos', os_version= '7', package= ['glibc-common-2.17-157.el7_3.5.x86_64'])
vulnerable_packages = centos_vulnerabilities.get('packages')
missed_patches_ids = centos_vulnerabilities.get('vulnerabilities')
cve_list = centos_vulnerabilities.get('cvelist')
how_to_fix = centos_vulnerabilities.get('cumulativeFix')

# Example for Debian 8
# You can use it for any DEB based OS
# Execute command: dpkg-query -W -f='${Package} ${Version} ${Architecture}\\n'
# Use it as package variable input

debian_vulnerabilities = vulners_api.audit(os= 'debian', os_version= '8', package= ['uno-libs3 4.3.3-2+deb8u7 amd64'])
```

### Download Linux (RPM/DEB based) vulnerability assessment data for local processing
```python
import vulners

vulners_api = vulners.Vulners(api_key="YOUR_API_KEY_HERE")

# Example for CentOS 7
centos_vulnerabilities_data = vulners_api.distributive("CentOS", "7")
```
### Download web application vulnerability detection regex collection
```python
import vulners

vulners_api = vulners.Vulners()

rules = vulners_api.rules()
```
