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
    1). class @ convert_fits_to_class.class  NGC253_a20151202_00030_01_0001

initialisation.sh : use Starlink to convert sdf to fits, get coordinates and
system temperature for each subscan and each receptor 

convert_JCMT_to_sdfits.py : use python to convert fits file (generated by
starlink) to sdfits format (readable for class) for each subscan and each
receptor 

convert_fits_to_class.class : use CLASS to read all fits files (each subscan
and each receptor) into a CLASS file (.jcmt) 


TODO: 

          1).remove temperary files 
          2). input output variables 
          3). more careful check  
    

----------------------------
## Part-2 test automated pipeline for quality control

Test file: between eye inspection and autotag code. in 71.202



/malatang/documents/JCMT_Harp_ndf2fits_to_GILDAS_CLASS_format_Conversion/demo/NGC253/20160722/dataset_20151202_00030_HCN/b_receptors_class.30m


方法：使用Nobeyama 45m 的CO 数据得到合理的 window范围，对HCN每个位置的数据自动设置window，同时使用理论和实测rms 差别以及Allan variation 来判断谱线质量并打分.

CO data: Nobeyama CO atlas survey 
    
http://www.nro.nao.ac.jp/~nro45mrt/download/COatlas/data/n253/N253RD_TMB.FITS.gz


使用： 执行run.class( 调用run0.class 和run1.class )



    @ run0.class  ! align the CO data to the HCN data 
    @ run1.class  ! baseline subtraction, and qualify the data, output qualify.dat 


    TODO: 1). smartly cut edge channels 
          2). output qualified spectra (instead of an array file). 
          3). more statistic analysis (Histogram fitting, find outliers, and more)  for the output array. 


----------------------------
## Part-3 A semi-manual interactive pipeline for data quality inspection 

Useage: 

    @flag_idx.class  input.jcmt  N   ! N means how many polygons you want to use for setting windows

    firstly set a polygon to set window for all spectra: click four corners of setting window regions. 

    Then start data quality inspection and flagging (selection). -- when you
    find some regions with bad data, please click the upper side then the lower
    side of this region. Then the code will automatically neglect this region
    in the output. 



