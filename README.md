# Test particle simulation
Test particle code to study particle transport or energization.
The code is parallelized using MPI and OpenMP, and it use HDF5 for output.

## Compile
1. You need a MPI library and parallel HDF5.
2. You may need to change some variables in `Makfile`, including `CC`, `LD`, and `HDF5_ROOT`.

Then, simply `make` in the root directory.

## Run the code
1. Copy `config_files/init.dat` and `config_files/test_particle.sh` into the directory
that you are going to run the code. And you need to change `init.dat` and `test_particle.sh`.
The variables in the files should be self-explained.
   * The included `test_particle.sh` is designed for using the fields generated by 3D VPIC simulations.
   You may have to write some code and scripts to load the fields from your own simulations.
2. You can run the code by `./test_particle.sh`.
3. In default, the code generates four files.
   * `diff_coeffs.dat` (ASCII): diffusion coefficients. Please check `diagnostics.c` for details.
   * `espectrum.dat` (ASCII): energy spectrum. The first row is particle energy bins. The rest rows
   are spectrum at a series of time frames.
   * `particle_diagnostics.h5` (HDF5): particle trajectories.
   * `particles_fields_init.h5`: initial particle information, including particle position,
   velocity, time, charge/mass, electric field, and magnetic field.

## Plot
You can use the provided Python scripts to plot the data.
1. Diffusion coefficients
```sh
python reconnection_analysis.py --run_name $run_name --run_dir $run_dir \
       --tinterval $tinterval --tframe $tframe --diff_coeff --ptl_vel 0.7
```
where you have to define the simulation's parameters. For multiple injection velocity,
add `--multi_vel` flag.
2. Energy spectrum: change `--diff_coeff` to `plot_spectrum` in the above script.
3. Particle trajectory:
   * `particle_diagnostics.h5` saves each partilce information in a dataset.
   In order to read the data using ParaView + H5Part, we need to transfer the HDF5
   file to a H5Part file using
   ```sh
   python reconnection_analysis.py --run_name $run_name --run_dir $run_dir \
          --tinterval $tinterval --tframe $tframe --tran_hdf5 --ptl_vel 0.7
   ```
