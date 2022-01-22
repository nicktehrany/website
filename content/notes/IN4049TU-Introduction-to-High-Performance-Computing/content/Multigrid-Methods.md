---
title: "Multigrid Methods"
date: 2022-01-22T17:54:50+01:00
publishdata: 2022-01-22T17:54:50+01:00
type: "notes"
mathjax: true
---

The goal of multigrid methods is that with the prior methods such as Jacobi, SOR, information has to move across the grid, whereas with multigrid methods, we approximate on a coarser grid, solve the problem on there and then use that solution to start on finer grid problems. Coarser grids work recursively by using an even coarser grid. If the coarse grid solution is a good approximation for the fine grid, the final solution will be successful.

## Multigrid V-Cycle

Solves its initial equation $t_i x_i = b_i$ with a $b_i$ given and a guess for $x_i$. If it was a good guess return, else improve the solution with finer grid.

You therefore start with a fine grid, iterate on it, include this into a coarser grid and then iterate on the coarser grid. Then we go back to finer grid (which is where the V shape for its name comes from). The including into a finer grid is done by using the residual which depicts how the current matrix (result) is not satisfying the solution (since we don't have the error but the results for the current matrix).
