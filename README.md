# T3A HiSA AUSM Case

OpenFOAM-v2312 / HiSA flat-plate T3A case using AUSMPlusUp and characteristic boundary conditions.

## Case status

This version applies the corrections requested for the HiSA/AUSM flat-plate run:

- `AUSMPlusUp` flux scheme
- characteristic farfield boundary conditions for `U`, `p`, and `T`
- characteristic wall conditions for `p` and `T`
- no-slip / boundary-corrected wall velocity on the plate
- less diffusive reconstruction with valid `wVanLeer` scheme
- first-order/upwind transport for transition/turbulence scalar convection
- parallel-ready `decomposeParDict`

## Main files

```text
0/
  U, p, T, k, omega, nut, alphat, gammaInt, ReThetat
  include/freestreamConditions

constant/
  polyMesh/
  thermophysicalProperties
  momentumTransport
  transportProperties
  turbulenceProperties

system/
  controlDict
  fvSchemes
  fvSolution
  decomposeParDict
  sampleBLDict
  sampleBLDict_hisa
```

## Run serial

```bash
hisa 2>&1 | tee log.hisa
```

## Run parallel

```bash
decomposePar -force

mpirun --allow-run-as-root -np 4 hisa -parallel 2>&1 | tee log.hisa_parallel

reconstructPar -latestTime
```

If not running as root, remove `--allow-run-as-root`.

## Notes

Generated files such as `processor*/`, `postProcessing/`, solver logs and time directories are intentionally ignored by `.gitignore`.
