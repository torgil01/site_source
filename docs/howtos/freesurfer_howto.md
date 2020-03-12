# FreeSurfer Howto

## Recon all

```bash
export SUBJECTS_DIR=xx
recon-all -i t1w -all -subjid subj-name
```

You can also call `recon-all` without setting SUBJECTS_DIR

```bash
recon-all -i t1w -all -subjid subj-name -sd SUBJECTS_DIR
```

## Table of subcortical segmentation

```bash
asegstats2table --subjects bert ernie fred margaret \
	--meas volume \
	--tablefile aseg_stats.txt
```
_Note_: Use `-d comma` to write the tablefile as a csv file


## Table of cortical segmentation 

*Note* 
* **aparc** = Desikan-Killiany Atlas (default)
* **aparc.a2009s** = Destrieux Atlas

```bash
aparcstats2table --hemi lh \
        --subjects 004 021 040 067 080 092 \
        --meas thickness \
        --parc aparc \
        --tablefile lh.aparc.a2009.thickness.table
```


## Table of white matter parcellation volumes

```bash
asegstats2table \
        --subjects 004 021 040 067 080 092 \
        --segno 3007 3021 3022 4022 \
        --stats wmparc.stats \
        --tablefile wmparc.vol.table
```

mris_anatomical_stats ??

## Convert FreeSurfer mgz to nii
Reference space = 001.mgz 
File to convert  =  aseg.mgz

```bash
mri_convert -rl 001.mgz -rt nearest aseg.mgz aseg_nii.nii.gz
```
