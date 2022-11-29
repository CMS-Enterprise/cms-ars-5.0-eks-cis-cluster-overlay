# cms-ars-5.0-eks-cis-cluster-overlay

InSpec profile to validate the secure configuration of AWS EKS, against [CIS](https://www.cisecurity.org/cis-benchmarks/)'s CIS Amazon Elastic Kubernetes Service (EKS) Benchmark version 1.0.1 (Cluster requirements) tailored for CMS ARS 5.0.

The baseline includes tests that must be executed against the Kubernetes API and tests that must be executed against the underlying nodes over SSH. This profile handles only the cluster tests, and is intended to be run alongside the [CMS ARS 5.0 EKS CIS Node Overlay profile](https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-node-overlay).

	## Getting Started  
### InSpec (CINC-auditor) setup
For maximum flexibility/accessibility, we’re moving to “cinc-auditor”, the open-source packaged binary version of Chef InSpec, compiled by the CINC (CINC Is Not Chef) project in coordination with Chef using Chef’s always-open-source InSpec source code. For more information: https://cinc.sh/

This profile should be executed from a runner host with network access to the EKS cluster under test. The runner host must have InSpec installed. It is intended and recommended that CINC-auditor and this profile overlay be run from a __"runner"__ host (such as a DevOps orchestration server, an administrative management system, or a developer's workstation/laptop) against the target. This can be any Unix/Linux/MacOS or Windows runner host, with access to the Internet.

**For the best security of the runner, always install on the runner the _latest version_ of InSpec and supporting Ruby language components.**		__For the best security of the runner, always install on the runner the _latest version_ of CINC-auditor.__ 

Latest versions and installation options are available at the [InSpec](http://inspec.io/) site.		__The simplest way to install CINC-auditor is to use this command for a UNIX/Linux/MacOS runner platform:__
```
curl -L https://omnitruck.cinc.sh/install.sh | sudo bash -s -- -P cinc-auditor
```

__or this command for Windows runner platform (Powershell):__
```
. { iwr -useb https://omnitruck.cinc.sh/install.ps1 } | iex; install -project cinc-auditor
```
To confirm successful install of cinc-auditor:
```
cinc-auditor -v
```
> sample output:  _4.24.32_

Latest versions and other installation options are available at https://cinc.sh/start/auditor/.

## Specify your BASELINE system categorization as an environment variable
### (if undefined defaults to Moderate baseline)

```
# BASELINE (choices: Low, Low-HVA, Moderate, Moderate-HVA, High, High-HVA)
# (if undefined defaults to Moderate baseline)

on linux:
BASELINE=High

on Powershell:
$env:BASELINE="High"
```
## Tailoring to Your Environment
The following inputs may be configured in an inputs ".yml" file for the profile to run correctly for your specific environment. More information about InSpec inputs can be found in the [InSpec Profile Documentation](https://www.inspec.io/docs/reference/profiles/).		


```yaml	
# Used by FIX HERE









```

## Running the Profile

Executing the profile by downloading it to the runner:

```	
git clone https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-cluster-overlay.git
cd cms-ars-5.0-eks-cis-cluster-overlay
# How to run (linux)
BASELINE=<your_system_categorization> cinc-auditor exec . -t ssh://ec2-user@<node 1 IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:node1results.json
...
BASELINE=<your_system_categorization> cinc-auditor exec . -t ssh://ec2-user@<node N IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:nodeNresults.json
```

Executing the profile by executing it from this GitHub repository:

```
# How to run (linux)
BASELINE=<your_system_categorization> cinc-auditor exec https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-cluster-overlay/archive/main.tar.gz -t ssh://ec2-user@<node 1 IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:cluster1results.json
...
BASELINE=<your_system_categorization> cinc-auditor exec https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-cluster-overlay/archive/main.tar.gz -t ssh://ec2-user@<node N IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:clusterNresults.json
```

## Running This Baseline from a local Archive copy

If your runner is not always expected to have direct access to GitHub, use the following steps to create an archive bundle of this profile and all of its dependent tests:

(Git is required to clone the InSpec profile using the instructions below. Git can be downloaded from the [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) site.)	

```
mkdir profiles
cd profiles
git clone https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-cluster-overlay.git
cinc-auditor archive cms-ars-5.0-eks-cis-cluster-overlay
sudo cinc-auditor exec <archive name> -t ssh://ec2-user@<node N IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:clusterNresults.json
```

For every successive run, follow these steps to always have the latest version of this baseline and dependent profiles:		For every successive run, follow these steps to always have the latest version of this baseline and dependent profiles:

```
cd cms-ars-5.0-eks-cis-cluster-overlay
git pull
cd ...
cinc-auditor archive cms-ars-5.0-eks-cis-cluster-overlay --overwrite
sudo inspec exec <archive name> --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:cluster-results.json		sudo cinc-auditor exec <archive name> -t ssh://ec2-user@<node N IP address> -i private_key.pem --input-file <path_to_your_input_file/name_of_your_input_file.yml> --reporter cli json:clusterNresults.json
```

## Using Heimdall for Viewing the JSON Results

![Heimdall Lite 2.0 Demo GIF](https://github.com/mitre/heimdall2/blob/master/apps/frontend/public/heimdall-lite-2.0-demo-5fps.gif)

The JSON results output file can be loaded into **[heimdall-lite](https://heimdall-lite.cms.gov/)** for a user-interactive, graphical view of the InSpec results.	

The JSON InSpec results file may also be loaded into a **[full heimdall server](https://github.com/mitre/heimdall2)**, allowing for additional functionality such as to store and compare multiple profile runs.		

## Authors

- Will Dower - [wdower](https://github.com/wdower)
- Shivani Karikar - [karikarshivani](https://github.com/karikarshivani)
- Sumaa Sayed - [ssayed118](https://github.com/ssayed118)

## Special Thanks
- Rony Xavier - [rx294](https://github.com/rx294)
- Eugene Aronne - [ejaronne](https://github.com/ejaronne)

## Contributing and Getting Help
To report a bug or feature request, please open an [issue](https://github.com/CMS-Enterprise/cms-ars-5.0-eks-cis-cluster-overlay/issues/new).

### NOTICE

© 2018-2022 The MITRE Corporation.

Approved for Public Release; Distribution Unlimited. Case Number 18-3678.

### NOTICE		

MITRE hereby grants express written permission to use, reproduce, distribute, modify, and otherwise leverage this software to the extent permitted by the licensed terms provided in the LICENSE.md file included with this project.

### NOTICE
This software was produced for the U. S. Government under Contract Number HHSM-500-2012-00008I, and is subject to Federal Acquisition Regulation Clause 52.227-14, Rights in Data-General.

No other use other than that granted to the U. S. Government, or to those acting on behalf of the U. S. Government under that Clause is authorized without the express written permission of The MITRE Corporation.		

For further information, please contact The MITRE Corporation, Contracts Management Office, 7515 Colshire Drive, McLean, VA 22102-7539, (703) 983-6000.

## NOTICE

CIS Benchmarks are published by the Center for Internet Security (CIS), see: https://www.cisecurity.org/cis-benchmarks/.
