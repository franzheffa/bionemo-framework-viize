# Access and Startup BioNeMo Framework

The BioNeMo Framework is free-to-use and easily accessible. We recommend accessing the software through
the BioNeMo Docker container, which provides a seamless and hassle-free way to develop and execute code. By using the
Docker container, you can bypass the complexity of handling dependencies, ensuring that you have a consistent and
reproducible environment for your projects.

In this section of the documentation, we will guide you through the process of pulling the BioNeMo Docker container and
setting up a local development environment. By following these steps, you will be able to quickly get started with the
BioNeMo Framework and begin exploring its features and capabilities.

## Access the BioNeMo Framework

### Brev.Dev Access

The BioNeMo Framework container can run in a brev.dev launchable: [![ Click here to deploy.](https://uohmivykqgnnbiouffke.supabase.co/storage/v1/object/public/landingpage/brevdeploynavy.svg)](https://console.brev.dev/launchable/deploy/now?launchableID=env-2pPDA4sJyTuFf3KsCv5KWRbuVlU). It takes about 10 minutes to deploy this notebook as a Launchable. After launching the instance, launch a Terminal session in the Jupyter Lab UI.

**Notes**:

- This links to the nightly release and may be out of sync with these docs.
- Access to Brev.Dev requires credit card information.

### NGC Account and API Key Configuration

You can also access the BioNeMo Framework container using a free NVIDIA GPU Cloud (NGC) account and an API key linked to that account.

NGC is a portal of enterprise services, software, and support for artificial intelligence and high-performance computing
(HPC) workloads. The BioNeMo Docker container is hosted on the NGC Container Registry. To pull and run a container from
this registry, you will need to create a free NGC account and an API Key using the following steps:

1. Create a free account on [NGC](https://ngc.nvidia.com/signin) and log in.

2. At the top right, click on the **User > Setup > Generate API Key**, then click **+ Generate API Key** and
   **Confirm**. Copy and store your API Key in a secure location.

You can now view the BioNeMo Framework container
at this direct link in the
[NGC Catalog](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/clara/containers/bionemo-framework) or by searching the
NGC Catalog for “BioNeMo Framework”. You can also explore the other resources available to you in the catalog.

### NGC CLI Configuration

The NGC Command Line Interface (CLI) is a command-line tool for managing resources in NGC, including datasets and model
checkpoints. You can download the CLI on your local machine using the instructions
[on the NGC CLI website](https://org.ngc.nvidia.com/setup/installers/cli).

Once you have installed the NGC CLI, run `ngc config set` at the command line to setup your NGC credentials:

- **API key**: Enter your API Key
- **CLI output**: Accept the default (ASCII format) by pressing `Enter`
- **org**: Choose your preferred organization from the supplied list
- **team**: Choose the team to which you have been assigned from the supplied list
- **ace** : Choose an ACE, if applicable, otherwise press `Enter` to continue

**Note**: The **org** and **team** commands are only relevant when pulling private containers/datasets from NGC created by you or
your team. To access BioNeMo Framework, you can use the default value.

## Startup Instructions

BioNeMo is compatible with a wide variety of computing environments, including both local workstations, data centers,
and Cloud Service Providers (CSPs), such as Amazon Web Services, Microsoft Azure, Google Cloud Platform, and Oracle Cloud
Infrastructure, and NVIDIA’s own DGX Cloud.

### Running the Container on a Local Machine

To run the BioNeMo Framework container on a local workstation:

1. Pull the BioNeMo Framework container using the following command:

```bash
docker pull {{ docker_url }}:{{ docker_tag }}
```

2. Run it as you would a normal Docker container. For
example, to get basic shell access you can run the following command:

```bash
docker run --rm -it --gpus all \
  {{ docker_url }}:{{ docker_tag }} \
  /bin/bash
```

Because BioNeMo is distributed as a Docker container, standard arguments can be passed to the `docker run` command to
alter the behavior of the container and its interactions with the host system. For more information on these arguments,
refer to the [Docker documentation](https://docs.docker.com/reference/cli/docker/container/run/).

Refer to the next section, [Initialization Guide](./initialization-guide.md), for useful `docker run` command
variants for common workflows.

## Running on Any Major CSP with the NVIDIA GPU-Optimized VMI

The BioNeMo Framework container is supported on cloud-based GPU instances through the
**NVIDIA GPU-Optimized Virtual Machine Image (VMI)**, available for
[AWS](https://aws.amazon.com/marketplace/pp/prodview-7ikjtg3um26wq#pdp-pricing),
[GCP](https://console.cloud.google.com/marketplace/product/nvidia-ngc-public/nvidia-gpu-optimized-vmi),
[Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nvidia.ngc_azure_17_11?tab=overview), and
[OCI](https://cloudmarketplace.oracle.com/marketplace/en_US/listing/165104541).
NVIDIA VMIs are built on Ubuntu and provide a standardized operating system environment across cloud infrastructure for
running NVIDIA GPU-accelerated software. These images are pre-configured with software dependencies, such as NVIDIA GPU
drivers, Docker, and the NVIDIA Container Toolkit. For more information about NVIDIA VMIs, refer to the
[NGC Catalog](https://catalog.ngc.nvidia.com/orgs/nvidia/collections/nvidia_vmi).

The general steps for launching the BioNeMo Framework container using a CSP are as follows:

1. Launch a GPU-equipped instance running the NVIDIA GPU-Optimized VMI on your preferred CSP. Follow the instructions for
   launching a GPU-equipped instance provided by your CSP.
2. Connect to the running instance using SSH and run the BioNeMo Framework container exactly as outlined in the
   [Running the Container on a Local Machine](#running-the-container-on-a-local-machine) section on
   the Access and Startup page.
