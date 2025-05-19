# DFL-Linux-Docker
Deep Face Lab solution for Linux &amp; Docker for arm64 Jetson

Here's the complete **English version** of the instructions tailored for your **Jetson AGX Orin 64GB** running **JetPack 6.2 (CUDA 12.2, cuDNN 8.9, TensorRT 8.6)** on **Ubuntu 22.04**.

---

### ✅ Recommended: Custom Dockerfile based on NVIDIA L4T base image

JetPack 6.2 maps to **L4T R36.2**, so we’ll start from the official base image:

```
nvcr.io/nvidia/l4t-base:r36.2.0
```

---

### 🧪 Build the Docker image

```bash
docker build -t Jetson-DFL:latest .
```

---

### 🚀 Run the container

```bash
# docker compose run jetson-tf

docker compose run --rm Jetson-DFL

docker compose run --rm --user root 
```

or

```bash
docker run -it --runtime nvidia --network host --privileged \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e DISPLAY=$DISPLAY \
    jetson-tf-conda:r36.2
```

---

### ✅ Version Compatibility Summary

| Component  | Version (JetPack 6.2)         |
| ---------- | ----------------------------- |
| CUDA       | 12.2                          |
| cuDNN      | 8.9                           |
| TensorRT   | 8.6.x                         |
| TensorFlow | 2.13.0+nv23.09 (NVIDIA wheel) |
| Python     | 3.10                          |
| Conda      | Miniconda3 latest (aarch64)   |

---


