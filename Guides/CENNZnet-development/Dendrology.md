# Dendrology

Here is a list of branches and their purposes:

- Active branches:
  - `develop` Main development branch
  - `X.X.X` Fixed release branches
  - `X.X.X-rcX` Fixed release candidate branches
- Stale branches:
  - `master` Legacy development branch, will be updated as working release commits only ✂️
  - `master-hotfix` Hotfix branch on top of [dba02695](https://github.com/cennznet/cennznet/commits/dba02695) ✂️
  - `stable` Legacy stable branch ✂️
  - `stable-hotfix` Legacy hotfix branch on top of stable branch ✂️
  - `rimu` Production (Rimu) branch frozen at [dba02695](https://github.com/cennznet/cennznet/commits/dba02695), dependencies point to [cennznet/plug/rimu](https://github.com/cennznet/plug-blockchain/tree/rimu)
    - Note: [cennznet/plug/rimu](https://github.com/cennznet/plug-blockchain/tree/rimu) has 2 commits on top of [184369ae](https://github.com/cennznet/plug-blockchain/commits/184369ae) to resolve minor compile errors
  - `rimu-local` Same as above but uses local dependencies of plug-blockchain, created to support local testing and debugging of plug-blockchain
- `backports/doughnut-verification` Full doughnut verification support back ported into CENNZnet master (i.e. compatible with Rimu) ✂️

Note: ✂️ indicates it will be removed
