---
title: "A list of things that are about parallel matrix storage and computation"
date: "2009-05-28"
---

I want a thing that stores sparse matrices over N computers and I can do math on those matrices.

My Dream API:

**matrix = new(matrix\_name, cols, rows)**

**matrix.put(col, row, value) # (or matrix.put\_col(row, data) , matrix.put\_row(col, data) )**

**value = matrix.get(col, row)**

**new\_matrix = matrix.multiply(matrix\_B)**

**matrix.transpose()**

**new\_matrix = matrix.invert(iterations=0)**

**\[U, S, V\] = matrix.svd(iterations=0)**

… etc. Why not use MATLAB hurf durf, right. The trick: the backend has to be some distributed thingy that I can boot new machines to both support storage and compute at any time. The matrix I have in my tiny brain is about 100K cols x 1bn rows, with about 1% nonzeros. And that’s just one of them. That’s more data than can fit on a computer and more compute that can run on a computer.

I would think this is something the world has been furiously working on in the four years since I last logged out of a lamboot’d terminal at MIT. It was pretty bad back then, you had to have the right fortran compilers, you had to know what rcp did, set up your own NFS servers on firewire drives, get BLACS working before CBLAS before SCALAPACK etc etc. So when I fired up my searches this a.m. I was hoping to see a sea of rounded corners, drop shadows and product names with missing vowels all solving my problems. I was expecting Erlang implementations of Block Lanczos, maybe acts\_as\_golub. [No dice](http://www.youtube.com/watch?v=2F_2eKJ5-L4).

My first stop was Hadoop, Doug Cutting’s post-Lucene MapReduce impl:

- [Welcome to Hama project](http://incubator.apache.org/hama/) - not sure what’s up with this one, doesn’t seem to be much there but the guy (EDWARD YOON) seems to camp on every mailing list and answer every question about matrix storage / compute in hadoop with “HAMA will do this!” which isn’t very helpful when all it seems to have is a multiplier after two years. It does however have the adorable [An meeting with Professor Choi J.](http://wiki.apache.org/hama/ScaLAPACK#head-c74c0cf0321c30027308f16e68016f8ac2def50b) , which tipped over the “OK this is not real” meter to red immediately. 
- [MAHOUT-6](https://issues.apache.org/jira/browse/MAHOUT-6) is a good read all about how the Mahout project (machine learning on top of Hadoop) is going to store matrices. They seem to have committed a sparse and dense implementation but there don’t seem to many compute tasks that are available on top of it. For example, the SVD [MAHOUT-76](https://issues.apache.org/jira/browse/MAHOUT-76) is non-parallel and not iterative.

Next I tenderly looked up my old friends that use MPI. These get at the compute part but not the storage part.

- [ScaLAPACK](http://www.netlib.org/scalapack/) is the tired and true player in the space. No real changes since last time. Note this requires an MPI install and configuration and a lot of very very fiddly library installation, especially if you are looking for anything that touches your actual CPU’s hardware instead of generic “486” compiles.
- [PSPASES](http://www.cs.umn.edu/~mjoshi/pspases) is a sparse SPD solver that uses MPI, could be very useful. A bit scared that their example platforms include the RS6000 and the Cray T3E.
- [Parallel Linear Algebra Package (PLAPACK)](http://www.cs.utexas.edu/users/plapack) uses MPI but is only for dense matrices
- [The PETSc Scientific Computing Libraries](http://www.mcs.anl.gov/petsc/) looks great and there’s even a [petsc4py](http://www.cimec.org.ar/python/petsc4py.html) , always a good sign.
- Relying on PETSc is the hopeful [SLEPc - Scalable Library for Eigenvalue Problems](http://www.grycap.upv.es/slepc) , currently my front runner.
- Aztec- [http://acts.nersc.gov/aztec/index.html](http://acts.nersc.gov/aztec/index.html) might be the ticket for parallel iterative CG solving of Ax=b.

Non-parallel eigenvalue problem “solvers” include:

- [SVDLIBC](http://tedlab.mit.edu/~dr/svdlibc/) - a block lanczos (iterative) SVD 
- [SVDPACK](http://www.netlib.org/svdpack/) - all sorts of algorithms

So it looks like I am going to end up using MPI. Everything serious relies on it. Now I’ve got to figure out how to manage this without having to deal with configuration cruft. AMZN EC2-to-MPI bridges include:

- [elasticwulf - Google Code](http://code.google.com/p/elasticwulf/) - I was able to get this running in no time, it seems great. Two default images (master & worker) and then some scripts to manage them. The images include MPI and set them up automatically. Very slick.

Now, what about storage? Could I really hack up SLEPc or SVDLIBC to use our DHT key-value store to get at matrix values? Is that insane? I have no interest in putting 8TB of data on a EBS and NFS exporting it for the rest of the cluster. And what, [do I use an ASCII sparse matrix format](http://tedlab.mit.edu/~dr/svdlibc/SVD_F_ST.html)?
