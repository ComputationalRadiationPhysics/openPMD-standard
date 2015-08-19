Domain-Specific Naming Conventions for Electro-Dynamic/Static PIC Codes
=======================================================================

openPMD extension name: `ED-PIC`

openPMD extension ID: `1`

Mesh Based Records (Fields)
---------------------------

### Additional Attributes for the Group `meshesPath`

- **Required:**

  - `fieldSolver`
    - type: *(string)*
    - description: Maxwell/field solver
    - allowed values:
      - `Yee` ([doi:10.1109/TAP.1966.1138693](http://dx.doi.org/10.1109/TAP.1966.1138693))
      - `CK` (*Cole-Karkkainen* type solvers: [doi:10.1016/j.jcp.2011.04.003](http://dx.doi.org/10.1016/j.jcp.2011.04.003), [doi:10.1063/1.168620](http://dx.doi.org/10.1063/1.168620), [doi:10.1109/TAP.2002.801268](http://dx.doi.org/10.1109/TAP.2002.801268); M. Karkkainen et al., *Low-dispersion wake field calculation tools*, ICAP 2006)
      - `Lehe` ([doi:10.1103/PhysRevSTAB.16.021301](http://dx.doi.org/10.1103/PhysRevSTAB.16.021301))
      - `DS` (*Directional Splitting* after Yasuhiko Sentoku, [doi:10.1140/epjd/e2014-50162-y](http://dx.doi.org/10.1140/epjd/e2014-50162-y))
      - `PSTD`
      - `PSATD`
      - `GPSTD`
      - `other`
      - `none`

  - `fieldSolverOrder`
    - type: *(float)*
    - description: order of the `fieldSolver`
    - examples:
      - `2.0`
      - `3.0`
      - use `-1.0` for infinite order (for spectral solvers)

  - `fieldSolverParameters`
    - type: *(string)*
    - description: additional parameters for fields solvers
                   (optional, might be specified further in the future)

  - `currentSmoothing`
    - type: *(string)*
    - description: applied filters to the current field after the particles'
                   current deposition
    - note: might become a particle record attribute in the future
    - allowed values: same as for `fieldSmoothing`

  - `currentSmoothingParameters`
    - type: *(string)*
    - description: required if `currentSmoothing` is not `none`, additional parameters to describe the applied filter further
    - note: might become a particle record attribute in the future
    - allowed values: same as for `fieldSmoothingParameters`

  - `chargeCorrection`
    - type: *(string)*
    - description: applied corrections to fields to ensure charge conservation
    - allowed values:
      - `Marder` ([doi:10.1016/0021-9991(87)90043-X](http://dx.doi.org/10.1016/0021-9991(87)90043-X))
      - `Langdon` ([doi:10.1016/0010-4655(92)90105-8](http://dx.doi.org/10.1016/0010-4655(92)90105-8))
      - `Boris` (Birdsall & Langdon, *Plasma Physics via Computer Simulation*, 15-6)
      - `hyperbolic` (section III-B in [doi:10.1063/1.872648](http://dx.doi.org/10.1063/1.872648),
                      [doi:http://dx.doi.org/10.1006/jcph.2000.6507](http://dx.doi.org/10.1006/jcph.2000.6507),
                      section 2.3 in [doi:10.1016/S0920-3796(96)00502-9](http://dx.doi.org/10.1016/S0920-3796(96)00502-9))
      - `spectral` (various)
      - `other`
      - `none`

  - `chargeCorrectionParameters`
    - description: required if `chargeCorrection` is not `none`
    - example: `period=100`

### Additional Attributes for each `mesh record`

- **Required:**

  - `fieldSmoothing`
    - type: *(string)*
    - description: applied field filters for E and B
    - allowed values:
      - `Binomial`
      - `other`
      - `none`

  - `fieldSmoothingParameters`
    - type: *(string)*
    - description: additional parameters to describe the applied filter further
    - example: `period=10;numPasses=4;compensator=true`
    - reserved for future use: `direction=array()`, `stride=array()`

### Naming Conventions for `mesh records`

- fundamental fields: `E`, `B` for electric and magnetic fields

- auxiliary fields:
  - `J`, `rho` for current density and charge density

  - fields derived from particles: prefix them with `particleShortName_*`:
    - examples:
      - `current`: such as `electron_current`
      - `density`: such as `electron_density`
      - `particleEnergy`: kinetic energy of all particles, with their weighted
                          contribution to a cell
      - `energyDensity`: same as `particleEnergy` but divided by `density`
      - `particleCounter`: ignores the shape of a particle and just checks if the
                           position of it "corresponds" to a cell


Particle Records
----------------

### Additional Attributes for the `Group` of each Particle Species

- **Required:**

  - `particleShape`
    - type: *(float)*
    - description: the order of the particle assignment function shape
                   (see *Hockney* reprint 1989, ISBN:0-85274-392-0, table 5-1)
    - example values:
      - `0.` pointlike (NGP)
      - `1.` linear (CIC)
      - `2.` quadratic (TSC)
      - `3.` quadrilinear (PQS)
      - or an other `floating point` number

  - `currentDeposition`
    - type: *(string)*
    - description: current deposition scheme
    - allowed values:
      - `VillaBune` ([doi:10.1016/0010-4655(92)90169-Y](http://dx.doi.org/10.1016/0010-4655(92)90169-Y))
      - `Esirkepov` ([doi:10.1016/S0010-4655(00)00228-9](http://dx.doi.org/10.1016/S0010-4655(00)00228-9))
      - `ZigZag`
      - `directBoris` (Birdsall & Langdon, *Plasma Physics via Computer Simulation*, 15-5)
      - `directMorseNielson` (Birdsall & Langdon, *Plasma Physics via Computer Simulation*, 15-5)
      - `other`
      - `none`

  - `currentDepositionParameters`
    - type *(string)*
    - description: further parameters for current deposition schemes;
                   reserved for future use

  - `particlePush`
    - type: *(string)*
    - description: Particle-Pushing Algorithm
    - allowed values:
      - `Boris` (J.P. Boris. *Relativistic plasma simulation-optimization of a hybrid code.* USA, 1970)
      - `Vay` ([doi:10.1063/1.2837054](http://dx.doi.org/10.1063/1.2837054))
      - `other`

  - `particleInterpolation`
    - type: *(string)*
    - description: method that was used to interpolate fields to particle positions,
                   as described in [doi:10.1016/j.crme.2014.07.006](http://dx.doi.org/10.1016/j.crme.2014.07.006)
                   section 2.4
    - allowed values:
      - `uniform`: The fields are interpolated directly from the staggered grid
                   to the particle positions (with the same interpolation order
                   in all directions).
      - `energyConserving`: Also known as *Galerkin method*.
                            The fields are interpolated directly from the staggered
                            grid to the particle positions (with reduced
                            interpolation order in the parallel direction). This
                            scheme is energy conserving in the limit of infinitely
                            small time steps.
      - `momentumConserving`: The fields are first interpolated from the staggered
                              grid points to the corners of each cell, and then from
                              the cell corners to the particle position (with the
                              same order of interpolation in all directions). This
                              scheme is momentum conserving in the limit of
                              infinitely small time steps.
      - `other`

  - `particleSmoothing`
    - type: *(string)*
    - description: applied transformations or smoothing filters on copied
                   versions of the fields while interpolating those to 
                   the particle
    - allowed values:
      - `Binomial`
      - `other`
      - `none`

  - `particleSmoothingParameters`
    - type: *(string)*
    - description: required if `particleSmoothing` is not `none`;
                   additional parameters to describe the applied filter further
    - example: `period=1;numPasses=2;compensator=false`
    - reserved for future use: `direction=array()`, `stride=array()`

### Additional Attributes for each Particle `Record`

When using macroparticles (see below for the definition of the
macroparticle `weighting`), there is an ambiguity regarding whether the
particle quantity that is written (e.g. energy, momentum) is that of
the full macroparticle, or that of the underlying individual
particle. Therefore, this extension requires the two following attributes:

- `macroWeighted`
  - type: *(uint32)*
  - description: indicates whether this quantity is written for the
    underlying particle (`macroWeighted = 0`) or for the full
    macroparticle (`macroWeighted = 1`)
  - example: let us assume that a user writes the `charge` attribute
    of a macroparticle that represents 100 electrons. If this user
    chooses to write -1.6e-19 (charge of one individual electron) then
    `macroWeighted` must be 0. If the user writes -1.6e-17 (total
    charge of 100 electrons), then macroweighted must be 1.

- `weightingPower`
  - type: *(double / REAL8)*
  - description: indicates with which power of `weighting` (see below)
    the quantity should be multiplied, in order to go from the
    individual-particle representation to the full-macroparticle representation.
  - example: let us consider a macroparticle that represents 100
    electrons. In this case, `weighting` is w=100 and the
    charge of each underlying individual particle is q=-1.6e-19. Then
    the charge Q of the full macroparticle is given by: Q=q w^1 and
    therefore `weightingPower` must be 1.
  - advice to implementors: reading example (in h5py) and extracting
    charge of the macroparticles in Python. When not absolutely necessary,
    reading the additional `weighting` record can be avoided for performance
    reasons like this:
  ```python
f = h5py.File('example.h5')
species = f[path_to_species_group]
q = species["charge"][:]
u_si = q.attrs["unitSI"]
p = q.attrs["weightingPower"]
if q.attrs["macroWeighted"] == 0 and p != 0:
    w = species["weighting"][:]
    q_macro = u_si * q * w**p
else :
    # No need to read the weighting from disk
    q_macro = u_si * q
```

### Additional `Records` per Particle Species

- **Required:**

  - `charge`
    - type: *(float)*
    - description: electric charge of the macroparticle or of the
      underlying individual particle (depending on the `macroWeighted`
      flag)
    - advice to implementors: must have `weightingPower = 1`

  - `mass`
    - type: *(float)*
    - description: mass of the macroparticle or of the
      underlying individual particle (depending on the `macroWeighted`
      flag)
    - advice to implementors: must have `weightingPower = 1`

  - `weighting`
    - type: *(float)*
    - description: the number of underlying individual particles that
                   the macroparticles represent
    - advice to implementors: must have `weightingPower = 1`,
      `macroWeighted = 1`, `unitSI = 1` and `unitDimension==[0., ..., 0.]`

  - `momentum/` + components such as `x`, `y` and `z`
    - type: each component in *(float)*
    - description: component-wise momentum of the macroparticle or of the
      underlying individual particle (depending on the `macroWeighted` flag)
    - advice to implementors: must have `weightingPower = 1`

  - `position/` + components such as `x`, `y` and `z`
    - type: each component in *(float)*
    - description: component-wise global position of the particle;
                   global position of the particle; can be replaced
                   by splitting the position into the records `positionOfCell`
                   and `positionInCell` (see below)
    - advice to implementors: must have `weightingPower = 0`
    - example: use only `x` and `y` in 2D

- **Recommended:**

  - `positionOfCell/` + components such as `x`, `y` and `z`
    - type: each component in *(int)*
    - description: zero-origin position of the cell the particle is
                   associated with, this record requires the additional
                   attributes `gridSpacing` (N-D array), `gridGlobalOffset`
                   (N-D array) and `gridUnitSI` (scalar) (see base standard
                   for `mesh` records); this record together with
                   `positionInCell` replaces the otherwise required `position`
                   record
    - rationale: keeps the precision of particle positions constant along the
                 simulation, independent of their position inside the global
                 range of the record; the two mesh attributes are required
                 since this record is essentially a unitless cell index,
                 a position scaled to the cell edges
    - advice to implementors: must have `weightingPower = 0`; since this
                              is a cell index use `unitSI = 1` and
                              `unitDimension==[0., ..., 0.]`

  - `positionInCell/` + components such as `x`, `y` and `z`
    - type: each component in *(float)*
    - description: in-cell position, relative to the zero-origin give in
                   `positionOfCell`; must be in the same units (scaled to
                   cell edges) as `positionOfCell` and must therefore fulfill
                   for all values `|max-min| < 1.0`; the maximum allowed value,
                   determining the offset to the cell's zero-origin (given by
                   `positionOfCell`) must not be greater or equal to `1.0`
    - advice to implementors: must have `weightingPower = 0`; common ranges are
                              `[0.0 1.0)` and `[-0.5 0.5)`; since this is the
                              fraction of a cell index use `unitSI = 1` and
                              `unitDimension==[0., ..., 0.]`
    - example: Python code to read the position generically:
```python
f = h5py.File('example.h5')
species = f[path_to_species_group]

if "position" in species.keys():
    h5_position_x = species["position/x"]
    x = ( h5_position_x[:] + h5_position_x.attrs["offset"] ) \
      * h5_position_x.attrs["unitSI"]
else:
    h5_position_of_cell = species["positionOfCell"]
    h5_position_in_cell = species["positionInCell"]
    grid_spacing = position_of_cell.attrs["gridSpacing"] \
                 * position_of_cell.attrs["gridUnitSI"]
    grid_offset  = position_of_cell.attrs["gridGlobalOffset"] \
                 * position_of_cell.attrs["gridUnitSI"]

    x_local = h5_position_in_cell["x"][:] * grid_spacing
    x = h5_position_of_cell["x"][:].astype(float) * grid_spacing \
      + grid_offset + x_local
```

  - `particlePatches`
    - description: if this record is used in combination with the
                   `globalCellId` record, the `position` for `offset` and
                   `extend` refers to the `globalCellId` and not the in-cell
                   `position`
    - advice to implementors: must have `weightingPower = 0`

- **Optional:**
  - `id`
    - type: *(uint64 / UNSIGNED8)*
    - description: a globally-unique identifying integer for each particle,
                       that can be used to, e.g., track particles. This
                       identifying integer should be truly unique within the
                       simulation; in particular, even among different
                       species, two particles should not have the same id.
                       Also, when a particle exits the simulation box, its
                       identifying integer should not be reassigned to a new
                       particle.
    - advice to implementors: must have `weightingPower = 0`

  - `boundElectrons`
    - type: *(float)*
    - description: number of bound electrons of an ion/atom;
                   to provide information to atomic physics algorithms
    - advice to implementors: must have `weightingPower = 1`

  - `protonNumber`
    - type: *(float)*
    - description: the atomic number Z of an ion/atom;
                   to provide information to atomic physics algorithms
    - advice to implementors: must have `weightingPower = 1`

  - `neutronNumber`
    - type: *(float)*
    - description: the neutron number N = the mass number A - the atomic number Z
                   of an ion/atom;
                   to provide information to atomic physics algorithms
    - advice to implementors: must have `weightingPower = 1`
