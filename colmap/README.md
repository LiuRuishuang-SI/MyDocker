DockerFile of COLMAP
=======================

A minimal Docker wrapper around the official **[`colmap/colmap`](https://hub.docker.com/r/colmap/colmap)** image, plus recipes for  
* GPU (CUDA) execution  
<!-- * Sparse & dense reconstruction from the command line   -->
* Launching the COLMAP GUI via X11  
* Exporting PLY format for MeshLab / CloudCompare / Blender

#### Tested on
* Ubuntu 22.04 + NVIDIA driver 560.xx  
* Docker ≥ 24.x  
* NVIDIA Container Toolkit (nvidia‑docker2)  
* CUDA 12.6 inside the container

![alt text](image.png)

### Build the image

```bash
docker build -t colmap:latest .
```


### Prepare a dataset

Put your images to `/datasets/{your-dataset}/images` or download sample dataset from https://demuc.de/colmap/datasets/ and unzip them to `/datasets` folder.

Recommended layout (relative to the repo root):
```
datasets/
└── south-building/
    └── images/
└── other-customized-dataset/

workspace/
```

```bash
docker compose run --rm colmap bash
```

```bash
docker exec -it colmap bash
```

### Run the GUI (GPU or CPU)

```bash
colmap gui
```

- In the GUI choose File ▸ Open ▸ `/workspace`
- Import images (`/datasets/{your-dataset}/images`) if prompted
- Run Reconstruction ▸ Feature ▸ Match ▸ Mapper as usual

### One‑shot sparse reconstruction (CLI)

```bash
colmap feature_extractor --database_path /workspace/db.db \
                         --image_path /datasets/south-building/images \
                         --ImageReader.camera_model SIMPLE_RADIAL \
                         --ImageReader.single_camera 1 && \
colmap exhaustive_matcher --database_path /workspace/db.db && \
mkdir -p /workspace/sparse && \
colmap mapper --database_path /workspace/db.db \
              --image_path    /datasets/south-building/images \
              --output_path   /workspace/sparse
```

### Visualise results

#### 1. Inside COLMAP GUI

Open `/workspace/sparse/0` (or `dense/fused.ply`) in the left sidebar.

#### 2. MeshLab / CloudCompare / Blender

Convert text model to PLY

```bash
colmap model_converter \
  --input_path  /workspace/sparse/0 \
  --output_path /workspace/sparse/0/points3D.ply \
  --output_type PLY
```