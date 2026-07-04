# Group Tour Service Dataset

## Overview

This repository provides a comprehensive dataset for group tour services, designed to support research in preference-aware and constraint-sensitive
large-scale differentiated service customization. The dataset comprises three core components:

-   **User Preferences:** Both latent and explicit preference representations for 65,878 users.
-   **Service Resources:** QoS, price, and capacity information across eight tourism domains.
-   **Satisfaction Prediction Model:** A pre-trained model for estimating user satisfaction with specific service combinations.

---

## Directory Structure & Data Description

### 📁 `user/`

This directory contains user preference data derived from preference learning and clustering methods.

| File | Shape | Description |
| :--- | :--- | :--- |
| `user_embeddingVector_and_probabilityToGroups.csv` | [65878, 74] | Latent user preference embeddings (64-dim) concatenated with soft cluster assignment probabilities (10-dim). The 10-dimensional tail represents each user's probability of belonging to one of 10 fine-grained segments identified via **PCA + GMM** clustering. |
| `all_user_w_b.csv` | [65878, 256] | Explicit preference parameters for each user over 128 QoS attributes. Each row contains a weight vector **W** (128-dim) and a bias vector **b** (128-dim). The first QoS dimension corresponds to *activity start time*, while the remaining 127 dimensions map to the eight service domains. |
| `group_center_w_b.csv` | [10, 256] | Cluster-center-level explicit preference parameters (**W** and **b**) for each of the 10 segments obtained via **PCA + GMM**. Serves as representative preference profiles for each segmented group. |
| `all_user_group_ID_UPC.npy` | (65878,) | Hard cluster assignment (segment ID) for each user based on **PCA + GMM** clustering. |
| `all_user_group_ID_CV.npy` | (65878,) | Customer Value (CV) segment ID for each user. Higher values indicate higher customer value tiers. |
| `user_value_group_PD.npy` | [10, 128] | Common preference direction for each CV segment across all 128 QoS attributes. `True` indicates a positive preference (higher QoS → higher satisfaction); `False` indicates a negative preference (lower QoS → higher satisfaction). |

> **Note:** Files with `.npy` extension are stored in NumPy binary format and can be loaded via `numpy.load()`.

---

### 📁 `resource/`

This directory contains service resource data for eight tourism domains. Each CSV file includes quality attributes, **price** (second-to-last column), and **capacity** (last column).

| File | Shape | Domain |
| :--- | :--- | :--- |
| `Flight.csv` | [191, 14] | Flight services (12 quality attrs + price + capacity) |
| `Transportation.csv` | [23, 4] | Ground transportation (2 quality attrs + price + capacity) |
| `Accommodation.csv` | [2051, 73] | Accommodation / Hotels (71 quality attrs + price + capacity) |
| `Catering.csv` | [275, 8] | Catering / Dining (6 quality attrs + price + capacity) |
| `Attraction.csv` | [302, 19] | Tourist attractions (17 quality attrs + price + capacity) |
| `Shopping.csv` | [313, 15] | Shopping (13 quality attrs + price + capacity) |
| `Entertainment.csv` | [51, 5] | Entertainment (3 quality attrs + price + capacity) |
| `Free Activity.csv` | [26, 5] | Free / Leisure activities (3 quality attrs + price + capacity) |

---

### 📁 `model/`

This directory stores the pre-trained **customer satisfaction prediction model**, which estimates a given user's QoE sentiment scores for a specified combination of group tour services. 

---

## Key Terminology

-   **QoS (Quality of Service):** Measurable service attributes used to characterize resource quality across different domains.
-   **PCA + GMM:** Principal Component Analysis followed by Gaussian Mixture Model clustering, used for latent user segmentation.
-   **CV (Customer Value):** A value-based segmentation method where segment IDs reflect relative customer value tiers.
-   **Explicit Preference (W, b):** Nonlinear preference parameters where **W** captures attribute importance weights and **b** captures baseline bias per QoS dimension.