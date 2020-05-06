# Troubleshooting

Most errors that occur when running scripts and programs are typing errors or *logical* errors. 
Examples of logical errors is specifying the input for a program incorrectly, or forgetting to 
decalre a variable. Wrt. variable declaratioin, keep in mind that variable names are case sensitive, 
such that all these variables are unique. A good practicse is to adopt a consistent rule for 
varibale names. 

```bash 
$imagepath
$Imagepath
$imagePath
$ImagePath
```

Below are some more subtile errors you may encounter. 

## Backslash
One may want to break a long command into several lines to enhance readability. For example


```bash 
# hard to read 
antsRegistrationSyNQuick.sh -d 3 -f B.nii.gz -m A.nii.gz -o RegA2B
		
# easy to read
antsRegistrationSyNQuick.sh -d 3 \
		-f B.nii.gz \
		-m A.nii.gz \
		-o RegA2B
```

Note however that there has to be a *newline* directly afther the "\", if there is a
space the remaining argimenst will not be passed to the prgrams or script.


## DOS / Linux textfiles
If you prepare a textfile on a Windows system and transfer it to a Linux system. You might get 
errors if a script or program attempts to read the file. The problem is that newlines etc. have 
different character codes. You can avoid potential problems by using the `dos2unix` program.


```bash 
dos2unix textfile.txt
```




