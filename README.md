# AmberDPRc Docker

Docker for [AmberDPRc](https://gitlab.com/RutgersLBSR/AmberDPRc).

## Software

- [AmberTools](https://ambermd.org/) 24 rc4
- [DeePMD-kit](https://github.com/deepmodeling/deepmd-kit) (C++) v2.2.10
- [xtb](https://github.com/grimme-lab/xtb) v6.7.0
- [CUDA Toolkit](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/) 12.4 (You can still use DeePMD-kit without a GPU)

## Usage

You can run `sander.MPI` on two CPUs:
```
docker run -v $PWD:/work -w /work --cpus 2 ghcr.io/njzjz/amberdprc mpirun -n 2 sander.MPI -O -i mdin -p prmtop -c inpcrd
```

Or run `sander` on a GPU:
```
docker run -v $PWD:/work -w /work --gpus all ghcr.io/njzjz/amberdprc sander -O -i mdin -p prmtop -c inpcrd
```
