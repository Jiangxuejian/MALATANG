# MALATANG

## Part-1  convert JCMT file format to CLASS format 

test data: 
in 71.202

/home/malatang/2015raw/20151202/a20151202_00030_01_0001.sdf


#----------------
# UPDATE 13 Dec. 2016: 

Now in linux/mac, just copy the script files to the data folder and run:

    ./all_convert.sh 

There you go (get all the .jcmt files in CLASS format).


### or, if you like to run each script individually: 

    1). ./initialisation                            a20151202_00030_01_0001.sdf 
    2). ipython convert_JCMT_to_sdfits.py           a20151202_00030_01_0001.fits  
    1). class @ convert_fits_to_class.class         a20151202_00030_01_0001

initialisation.sh : use Starlink to convert sdf to fits, get coordinates and
system temperature for each subscan and each receptor 

convert_JCMT_to_sdfits.py : use python to convert fits file (generated by
starlink) to sdfits format (readable for class) for each subscan and each
receptor 

convert_fits_to_class.class : use CLASS to read all fits files (each subscan
and each receptor) into a CLASS file (.jcmt) 

Note: 
1). 
If you use Mac, please install rename by :

        brew install rename 

2). convert_JCMT_to_sdfits.py
uses python 3.5, please install it using anaconda:
https://www.continuum.io/downloads

Please install the plugins (most of them are installed by anaconda by default):

    matplotlib
    scipy 
    numpy
    astropy

3). please specify the coordinate of the reference position in convert_fits_to_class.class  

    Please see the note header in convert_fits_to_class.class 


TODO: 

          1). remove temperary files -- Done 
          2). input output variables -- Done  
          3). more careful check     -- half done 
    

----------------------------
## Part-2 test automated pipeline for quality control

Test file: between eye inspection and autotag code. in 71.202



/malatang/documents/JCMT_Harp_ndf2fits_to_GILDAS_CLASS_format_Conversion/demo/NGC253/20160722/dataset_20151202_00030_HCN/b_receptors_class.30m

Method: 
    1) use CO mapping data (e.g. Nobeyama 45m CO survey ) as reference, for setting windows (line free channels) in the baseline subtraction.  
    2). Use the ratios of theoritical rms and measured rms and the allan variation to qualify each spectrum.

CO data: Nobeyama CO atlas survey 
    
http://www.nro.nao.ac.jp/~nro45mrt/download/COatlas/data/n253/N253RD_TMB.FITS.gz

Useage: 
    class @ qualify.class 
    
    qualify.class calls run.class which calls the following: 

    @ run0.class  ! switch the HCN/HCO+ data to relative velocity frame
    @ run1.class  ! qualify all data and output qualify.dat
    @ run2.class  ! analysis spectral qualities and output plots
    @ run3.class  ! final baseline subtraction with CO reference data




      usage:
      @ run molecule redshift jcmt_file reference_file velocity_resolution_for_qualify weak_or_not
    
      molecule:        
                      Molecule to select HCN or HCO+ for the rest frequencies
      reference_file:  
                      The name of the .fits file, but you need to lmv an output class format first if there is no reference .fits file, then please set it to none, then the code automatically recognises it and set window -100 100 accordingly.

      velocity_resolution_for_qualify:  
                      The velocity resolution for the baseline qualification
      weak_or_not: 
                      if .TRUE. then weak line, then do not set window using the CO data during the qualification but still set CO window in the last baseline subtraction step
                      if .False. then strong line, then set window using the CO data both in qualification and in the last baseline subtraction step.
     ----------------------------------------------------------------------
      Note: two folders are needed for the qualification : tagged, quality
      tagged will contain all the tagged (qualified) data.
      quality will contain all the plots of the qualification and the quality data of each spectrum
    






    TODO: 1). automatically cut edge channels  -- done  
          2). output qualified spectra         -- done. 
          3). more statistic analysis (Histogram fitting, find outliers, and more)  for the output array. -- done. 
          4). check the discripance between resolution and channel width. -- done  


----------------------------
## Part-3 A semi-manual interactive pipeline for data quality inspection 

Useage: 

    @flag_idx.class  input.jcmt  N   ! N means how many polygons you want to use for setting windows

    firstly set a polygon to set window for all spectra: click four corners of setting window regions. 

    Then start data quality inspection and flagging (selection). -- when you
    find some regions with bad data, please click the upper side then the lower
    side of this region. Then the code will automatically neglect this region
    in the output. 



