# hetdex-unhdf5

## Introduction 

[HETDEX](http://hetdex.org/) will be storing its data in structures saved into [HDF5](https://www.hdfgroup.org/solutions/hdf5/) files. This tool will allow the user to extract data from one of these HDF5 files, e.g. a datacube or spectra from a particular shot and IFU, and either store it as a [FITs](https://fits.gsfc.nasa.gov/fits_documentation.html) file or output it on command line. Since HETDEX HDF5 files will have a unique datastructure, we decided to implement out own
conversion code rather than using an off-the-shelf module like e.g. [hdf2fits](https://github.com/telegraphic/fits2hdf). 

**All of this is just the planned design. It is not yet implemented**

## Planned Usage

```
usage: unhdf5 [-h] [--fout fn_out] fn_hdf5 whichdata args [args ...] 

```

Where the arguments are

| Argument  | Description                                                                                 |
|-----------|---------------------------------------------------------------------------------------------|
| fn_hdf5   |Filename of the HDF5 file                                                                    |
| whichdata |Type of data to extract e.g. multifits, flux limit cube etc                                  |
| args      |Variable list of arguments that depends on whichdata. For multifits it could be e.g. IFUSLOT |
| --fout    |Optional output filename. If not included send output to standard out.                       |

The idea behind the ```whichdata``` and ```args``` approach is to be flexible and expandable. All of the arguments
in args will be collected and passed to a function that is selected based off of ```whichdata```. This function
handles the generation of what we want to display or save. This allows us to add new possible output types easily. 

## Planned Example usage

Extract a multifits style file for IFU 034 from the HDF5 file and display it in ds9

```
unhdf5 20190117v09.h5 multifits 034  | xpaset ds9
```

Save a multifits style file of IFU 082 to a FITs file

```
unhdf5 --fout 20190115v11_082.fits 20190117v09.h5 multifits 082
```

Wacky example: say we had a ```whichdata``` option called ```allsci``` which output all of the science fibers of
a shot. We could then display a 2D image of all of a shot's science fibers with

```
unhdf5 20190117v09.h5 allsci | xpaset ds9
```

## References

Some of our work and design was influenced by [hdf2fits](https://github.com/telegraphic/fits2hdf)

