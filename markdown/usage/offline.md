---
title: Running offline
subtitle: Using nf-core pipelines without an internet connection.
---

# Introduction

Nextflow supports fetching nearly everything it needs to run a pipeline over the web automatically: pipeline code, software requirements, reference genomes and even remote data sources.

If you need to run your analysis on a system that has no internet connection, don't panic!
There are just a few extra steps required to get everything you need available locally.

The general principle is to fetch all of the things that you'll need on a system that _does_ have an internet connection (typically your personal computer).
Then, transfer these to your offline system by whatever method you have available.

Generally you will need three things: a working version of [Nextflow](#nextflow), the [pipeline assets](#pipeline-code) and any required [reference genomes](#reference-genomes).

## Nextflow

First of all, you need to have Nextflow installed on your system.
Go to the Nextflow releases page on GitHub: [https://github.com/nextflow-io/nextflow/releases](https://github.com/nextflow-io/nextflow/releases).
Each release has a dropdown with associated _Assets_.
One of these should have the suffix `-all`, _e.g._ `nextflow-19.10.0-all`.
Download this file and transfer to your offline system.
Run it to install Nextflow (it is a very large _bash_ file).

Once installed, you can stop nextflow from looking for updates online by adding the following environment variable in your `~/.bashrc` file:

```bash
export NXF_OFFLINE='TRUE'
```

## Pipeline code

To run a pipeline offline you need the pipeline code, the software requirements and the shared nf-core/configs configuration profiles.
To help with this process, we have created a helper tool as part of the _nf-core_ package to automate this for you.

On a computer with an internet connection, run `nf-core download <pipeline>` to download the pipeline and config profiles.
Add the argument `--container singularity` to also fetch the singularity container(s).

The pipeline and requirements will be downloaded, configured with their relative paths and packaged in to a `.tar.gz` file by default.
This can then be transferred to your offline system and unpacked.

Inside you will see directories called `workflow` (the pipeline files), `config` (a copy of [nf-core/configs](https://github.com/nf-core/configs)) and if you used `--container singularity` a directory called `singularity`.
The pipeline code is adjusted by the download tool to expect these relative paths, so as long as you keep them together it should work out of the box.

To run the pipeline, simply do `nextflow run <download_directory>/workflow [pipeline flags]`

### Shared storage

If you are downloading _directly_ to the offline storage (eg. a head node with internet access whilst compute nodes are offline), you can use the `--singularity-cache-only` option for `nf-core download` and set the `$NXF_SINGULARITY_CACHEDIR` environment variable.
This downloads the singularity images to the `$NXF_SINGULARITY_CACHEDIR` folder and does not copy them into the target downloaded pipeline folder.
This reduces total disk space usage and is faster.

For more information, see the [documentation for `nf-core download`](https://nf-co.re/tools#downloading-pipelines-for-offline-use).

## Reference genomes

Some pipelines require reference genomes and have built-in integration of AWS-iGenomes.
If you wish to use these references you must download them and transfer to your offline cluster.
Once transferred, follow the [reference genomes documentation](reference_genomes.md) to configure the base path for the references.

## Bytesize talk

Here is a bytesize talk explaining the necessary steps to run pipelines offline.

<!-- markdownlint-disable -->
<iframe width="560" height="315" src="https://www.youtube.com/embed/N1rRr4J0Lps" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<!-- markdownlint-restore -->
