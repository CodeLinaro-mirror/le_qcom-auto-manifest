# QCOM Auto Manifest – Build & Release Guide

## Overview

This document provides instructions to set up the environment and build images using the supported build methods.

---

## Prerequisites

<details>
<summary>Click to expand</summary>

### QPM Access

- Configure a `.netrc` file with QPM credentials.
  - Required to download Qualcomm proprietary meta layers and codebase.

- Pass the netrc file during build:

```bash
NETRC_FILE=<NETRC_FILE> kas-container
```

### Proprietary Repository Access

- Configure Git to redirect Qualcomm proprietary repository URLs to the customer-specific repository namespace:

```bash
git config --global \
  url."https://qpm-git.qualcomm.com/home2/git/<customer_nametag_in_qpm>/".insteadof \
  "https://qpm-git.qualcomm.com/home2/git/qualcomm/"
```

### Metadata Configuration

- Update the commit ID referenced by `snapdragon-auto-2026-le-1-1_hlos_oem_metadata` in `kas/deps.yml`.
  - Replace the existing commit ID with the corresponding commit ID from the customer repository for the required tag.
  - Verify that the commit reference matches the intended release tag before starting the build.

### Customer ID
- Ensure that the cust_id specified in include/base.yml is updated to the appropriate customer ID configured in QPM.

</details>

---

## Build Options

<details>
<summary>Click to expand</summary>

There are two supported build methods:

- **Kas Container Build**
  - Single machine configuration
  - Multi machine configuration

- **Meta Layer Manifest Build**

</details>

---

## Kas Container Build

<details>
<summary>Click to expand</summary>

### Sync Source

```bash
git clone -b LY.AU.0.2.1.r2-02200-gen5meta.0 \
    https://git.codelinaro.org/clo/le/qcom-auto-manifest

cd qcom-auto-manifest
```

### Single Machine Configuration

#### sa8797

```bash
kas-container build kas/sa8797.yml:kas/qc-buildserver.yml
```

#### sa8775-flex

```bash
kas-container build kas/sa8775-flex.yml:kas/qc-buildserver.yml
```

### Multi Machine Configuration

#### sa8775-flex + sa8797

```bash
kas-container build \
    kas/sa8775-flex-sa8797-multiconfig.yml:kas/qc-buildserver.yml
```

</details>

---

## Meta Layer Manifest Build

<details>
<summary>Click to expand</summary>

### Sync Source

```bash
repo init \
    -u https://git.codelinaro.org/clo/le/qcom-auto-manifest \
    -b commonrelease \
    -m LY.AU.0.2.1.r2-02200-gen5meta.0.xml

repo sync -j10
```

### Build sa8797

```bash
source poky/build/conf/set_bb_env.sh -t sa8797
build-sa8797-image
```

### Build sa8775-flex

```bash
source poky/build/conf/set_bb_env.sh -t sa8775-flex
build-sa775-flex-image
```

</details>

---
