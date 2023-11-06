# PYSOLATOR

**PYSOLATOR** is a software that allows you to remove the orbital modulation from a binary pulsar and/or its companion.

In essence, it subtracts the predicted Roemer delay for the given orbit and then resamples the time series so as to make the signal appear as if it were emitted from the barycenter of the binary system.

For this reason, PYSOLATOR is the ideal tool to search for pulsations from a putative second pulsar in systems like pulsars with neutron star companions. By removing the orbital motion of the companion, PYSOLATOR makes the search for pulsations much easier and faster!

## Installation


PYSOLATOR has been designed with the end user in mind, so as to require the minimum effort to get it go. For this reason, no real installation is needed. To get started with PYSOLATOR:

1.  Download the code using git with:
 `git clone https://github.com/alex88ridolfi/PYSOLATOR.git`
 
     Alternatively, you can download the zipped repository by clicking on the [this link](https://github.com/alex88ridolfi/PYSOLATOR/archive/refs/heads/master.zip).
    
    
2.  Add the PYSOLATOR directory path to your PATH environment variable in your `.bashrc` file in your home directory.
`echo "export PATH=$(realpath PYSOLATOR):\${PATH}" >> ~/.bashrc`

3. Log-out and re-login to your terminal to make the changes effective.

4. Run PYSOLATOR with `pysolator`. 

If you run it with no arguments, you should get the following usage help text:
```
Usage1: pysolator [-Q] -par <normal_parfile> -datfile <BARYCENTERED!!_time_series> [-dt 0.000070] [-numout N] [-ncpus N] [-Mp_Mc "1.4,2.0"] {-comp_only | -pulsar_only} [-o outfile_basename]

Output:  isol_<datfile_basename>_p.dat   ---> Demodulated orbit for the pulsar, useful to check whether the demodulation was done correctly
         isol_<datfile_basename>_c.dat   ---> Demodulated orbit for the companion, which you may want to search
```


## Basic usage

To successfully use PYSOLATOR, you need:

1.  A  barycentered  [PRESTO](https://www.cv.nrao.edu/~sransom/presto/)  dedispersed time series (".dat" file, plus the corresponding ".inf" file) containing the signal of the pulsar of interest.  
    To generate a barycentered PRESTO dedispersed time series, you can use the PRESTO's  *prepdata* routine on the original search mode data, without using the  -nobary option.
    
2.  A phase-connected ephemeris (".par" file) of known pulsar with the DDGR binary model for the orbit and at least two post-Keplerian parameters.  
    The latter are necessary to measure the masses of the pulsar and its companion (assuming General Relativity), which in turn give access to a precise description of the companion orbit, generally not known unless the ratio between the mass of the pulsar and that of the companion is known.
    

Assuming to have a dedispersed time series called "observation_J0000+0000.dat" and a corresponding ephemeris called "ephemeris_J0000+0000.par", we can remove the orbit from the pulsar and the companion by simply typing:

`pysolator -datfile observation_J0000+0000.dat -par ephemeris_J0000+0000.par`

After demodulating the time series for the pulsar and/or the companion orbit, PYSOLATOR produces a number of output files, namely:

-   `isol_observation_J0000+0000_q_0.7143_p.dat` (+ corresponding .inf file):  
    Time series demodulated for the pulsar orbit: here the pulsar signal resembles that of an isolated pulsar located at the binary barycenter.
    
-   `isol_observation_J0000+0000_q_0.7143_c.dat` (+ corresponding .inf file):  
    Time series demodulated for the companion orbit: here the signal coming from the putative companion pulsar will resemble that of an isolated pulsar located at the binary barycenter.
    
-   `ephemeris_J0000+0000_noorb.par`:  
    Copy of the original ephemeris but without the orbital parameters. Useful to fold the demodulated time series of the pulsar, to check that the modulation of the known signal was done correctly.
    
-   `ephemeris_J0000+0000_q_0.7143_comp.par`:  
    Ephemeris containing the orbital parameters of the companion, calculated assuming a mass ratio q=Mp/Mc=0.7143, in turn derived from the MTOT and M2 parameters of the orginal ephemeris. **NOTE:** the spin period is set to be the same as that of the pulsar! Don't try to use this ephemeris to fold, if you haven't searched for the companion yet!

