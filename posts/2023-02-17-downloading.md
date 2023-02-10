---
title: How to download data ⬇
summary: Multiple avenues to access and download data that are presented on the site.
thumbnail_url: 'https://raw.githubusercontent.com/janelia-cosem/openorganelle-blog/main/assets/...jpg'
carousel_url: 'https://raw.githubusercontent.com/janelia-cosem/openorganelle-blog/main/assets/...jpg'
tags: ["big data","download", "data access"]
authors: ["Davis Bennett", "Aubrey Weigel", "John Bogovic"]
date: "2023-02-17T01:01"
published: False
---

When working with imaging datasets that greatly exceed the memory available on a single computer, best practice is to store the dataset in small chunks and use programs that leverage the chunked representation. This is the approach we recommend for the datasets displayed on OpenOrganelle. Accordingly, we do not offer a browser-based tool for directly downloading entire datasets, as downloading hundreds of thousands of files totalling hundreds of gigabytes is not feasible in the web browser.

# Fiji 
Fiji is a popular tool for image processing; download it here here. Several Fiji plugins can be used to access OpenOrganelle datasets. To open a dataset with a Fiji-based tool, you will need the URL for a dataset (i.e., the location of the data in cloud storage). Clicking on the Fiji icon blue fiji logo copies the dataset URL to the clipboard, which can be pasted in the Fiji app.

## N5 Import
To open the datasets in Fiji and make use of all of the Fiji tools (e.g., saving to disk), select the File dropdown menu, then Import, then N5, and paste the URL for the dataset of interest in the text entry field. For instructions on opening our datasets in Fiji please refer to this documentation. Be advised that this method causes Fiji to load an entire image volume into memory, which will fail for large image volumes. Additionally, this option currently does not generate a representation of data that supports interactive 3D visualization.

### Opening as a virtual image
### Cropping a region of interest
## N5-Viewer
Maybe just want to look around at the data. N5-viewer plugin takes advantage of the multiscale pyramids for faster browsing and visualization.

The BigDataViewer plugin allows interactive 3D visualization of our datasets. This plugin can be accessed by selecting the Plugins dropdown menu, then selecting BigDataViewer, and finally N5 Viewer. Paste the URL for the dataset of interest in the text entry field of the plugin. For detailed instructions refer to the N5 Viewer documentation. Note that this plugin does not allow directly saving full volumes to disk (but you can crop out a subregion of the data for saving).

### Cropping a region of interest
## How Do I know what to choose?

# Python
To programatically access generic data stored on s3 from Python, we recommend the package fsspec. For efficient access to image data, we recommend combining fsspec with the zarr-python library and the parallelization library Dask, all of which can be installed via pip:
$ pip install "fsspec[s3]" "zarr" "dask"
Or conda:
$ conda install -c conda-forge s3fs fsspec zarr dask
The following example demonstrates browsing an s3 bucket with fsspec, then accessing data via zarr and dask.
>>> import fsspec, zarr
>>> import dask.array as da # we import dask to help us manage parallel access to the big dataset
>>> group = zarr.open(zarr.N5FSStore('s3://janelia-cosem-datasets/jrc_hela-2/jrc_hela-2.n5', anon=True)) # access the root of the n5 container
>>> zdata = group['em/fibsem-uint16/s0'] # s0 is the the full-resolution data for this particular volume
>>> zdata
<zarr.core.Array '/em/fibsem-uint16/s0' (6368, 1600, 12000) uint16>
>>> ddata = da.from_array(zdata, chunks=zdata.chunks)
>>> ddata
dask.array<array, shape=(6368, 1600, 12000), dtype=uint16, chunksize=(64, 64, 64), chunktype=numpy.ndarray>
>>> result = ddata[0].compute() # get the first slice of the data as a numpy array

For convenience, all of the above functionality is contained in a python library we maintain ( fibsem_tools). As in the Fiji examples, the Python library addresses datasets through their URL. See the fibsem_tools documentation for code examples.

# AWS CLI
As all our datasets are stored publicly on AWS S3, it is possible to access the underlying files through any tool that can read from S3. We recommend using the AWS command line interface (AWS CLI). 
When run from the command line, this command lists the contents of our data bucket:
aws cli ls s3://janelia-cosem/
The AWS CLI can also copy data from S3 to local storage, but be advised that this may take a long time. Detailed instructions for using this tool can be found in the user guide.

# Cyberduck

# Globus
