# MUographY Simulation Code (MUYSC) tomography module
> author: Kevin Dlaikan kdlaikanj@gmail.com

Muon tomography is the process in which a volume is reconstructed given a set of projections (muograms) taken in different angles across the structure. The 3D reconstruction has the inner density distribution, which allows to study structures such as active volcanos.

This repository contains the tomography module of MUYSC which contains:

1. Muogram reading and processing.

2. Distance and angle estimation.

3. Tomographic reconstruction of structures.

4. Estimation of reconstruction region.

5. Visualization of reconstruction slices and observation point ubication.

## How to run

This code runs with TomoPy library. To install this library you need to have [Conda](https://www.anaconda.com/products/distribution) installed first, then open a terminal or a command prompt window and run:

`conda install -c conda-forge tomopy`

To use the module, you need to have a directory that contains ONLY the muograms obtained by the MUYSC muography module. The path to that directory is later used to read the data.

Another path is used to read the topography of the region, that is later used to visualize observation point ubication and reconstruction comparison.

## Module methods

- `get_data`: Reads the muogram data given the path oof the directory containing it. It returns the processed data as two dimensional muograms, and an array containing the reference and observation points coordinates (latitude and longitude), as well as useful information contained in the metadata such as the region coordinates, reference and observation points altitude (meters), and muogram's angular resolution (degrees).

- `plot_coords`: Plot the reference and observation points within the topography of the region.

- `get_angles`: Returns the angle (degrees) of the observation points respect to the reference point.

- `get_recon`: Returns the reconstructed volume given muograms and angles. Optionally, the following arguments can be given:

    - algorithm: Name of the reconstruction algorithm. The list can be found in TomoPy's [documentation](https://tomopy.readthedocs.io/en/stable/api/tomopy.recon.algorithm.html). However, it should be noted that our research shows that the 'art' algorithm (default algorithm of this module) gives best results. 

    - negatives: Boolean value. If true (default), negatives values of the reconstructed volume are changed to 0. Negative values appear as an estimation of the algorithm for non-observed regions (e.g altitudes below lowest observed altitude).

    - idx: index array of selected points to use in reconstrucion. If no array is given, reconstruction is made with all the data. User might want to use the `remove_points` function to get an index array of equally spaced points.

- `get_recviews`: Returns views of all axis of the reconstruction. By default views are the sum of all slices across the axis, unless an array is given containing the index of the desired specific slices in [x,y,z]. The top view can be normalized (default) for later comparison with original top view.

- `remove_points`: Returns index array of equally spaced points, given the number of desired points to have.

- `get_recregion`: Returns an array of the reconstruction region, as well as the lowest observable altitude. This region should be used to get a new topography on the muography module if reconstrcution comparison is wanted.

- `get_oriviews`: Returns processed original top view (removed values below lowest observable altitude) and muograms closer to 0 and 90 degrees (original front and lateral views).

- `get_recaxis`: Uses reconstruction region and angular resolution to create axis for plotting original and reconstructed views.
