# OSC_Basic_use_tutorial


## Table of Contents
<a href="#Documentation">Documentation</a></br>
<a href="#Intro to using OSC">Intro to using OSC</a></br>
<a href="#Logging in">Logging in</a></br>
<a href="#Common Linux Commands">Common Linux Commands</a></br>
<a href="#Making Job scripts">Making Job scripts</a></br>
<a href="#Submitting jobs">Submitting jobs</a></br>
</br>

## <a name="Documentation">Documentation</a>
Emacs: https://www.gnu.org/software/emacs/manual/html_node/emacs/index.html

OSC: https://www.osc.edu/resources/technical_support/supercomputers/owens

## <a name="Intro to using OSC">Intro to using OSC</a>

### <a name="Logging in">Logging in</a>
Use the command ssh to log in!  Example:
```{bash eval=FALSE}
"ssh username@owens.osc.edu"

ssh janedoe@owens.osc.edu
```
Afterwards It'll ask for your password. If its the first time logging in from a new computer it may ask to confirm you trust the key (follow the yes prompt to continue)

### <a name="Common Linux Commands">Common Linux Commands</a>
mkdir = make directory (Exapmle: mkdir name_of_new_directory)
```{bash eval=FALSE}
mkdir tutorial
```
cd = change directory (can also use cd .. to go back)
```{bash eval=FALSE}
cd tutorial
```
ls = list the contents of current directory (ls -lh for more detail)
```{bash eval=FALSE}
ls
```
mv = to move or rename a file (Examples, Move: mv file /new/directory Rename: mv original file_name new_file_name)
```{bash eval=FALSE}
mv tutorial cool_name
```
pwd = full path name of working directory
```{bash eval=FALSE}
pwd
```
rm = remove a file (be careful theres no going back with this command)
```{bash eval=FALSE}
rm unwanted_file_name
```
sftp = transferring files to and from different clusters or local computer (Example: sftp username@viz3.acg.maine.edu)
```{bash eval=FALSE}
sftp username@cluster.edu
```
Within sftp you can transfer or obtain files using the put or get commands
```{bash eval=FALSE}
get desired_file_name
put file_to_be_transfered
```
more = Use to see contents without opening the full file, always use more if trying to check a large file (space to go deeper into the file, q to quit)
```{bash eval=FALSE}
more hello_world.txt
```
emacs = make or edit a new file (Example: emacs new_script.sh, see documentation for how to use emacs)
```{bash eval=FALSE}
emacs hello_world.txt
```

### <a name="Making Job scripts">Making Job scripts</a>
Every job script must have #!/bin/bash following #SBATCH commands that set the parameters for that job before all job code. I'd recommend making a separate directory for all of your scripts for within that project folder (example file structure: Data->Project_A->scripts)
```{bash eval=FALSE}
#!/bin/bash
#SBATCH --account=PYS1234                            #Account name/ID
#SBATCH --job-name=summarize_demux                   #Job name
#SBATCH --time=02:00:00                              #Job time limit hhmmss
#SBATCH --nodes=1                                    #total_nodes
#SBATCH --ntasks-per-node=8                          #total_tasks
#SBATCH --mail-type=ALL                              #email notifications of job status 
#SBATCH --mail-user=my.email@maine.edu               #Your email for job notifications     

#not necessary, but I like to use the "date" command in my scripts to see how long things run (will show in output log)
date  

#load any needed modules (see list of modules by typing "module avail" in terminal, or search for specific module with "module spider")
module load singularity     

#path to working directory
cd /user/PYS1234/janedoe/data   

#job code
singularity run docker://qiime2/core:2018.11 qiime demux summarize --i-data AmericanGut-demux-single-end.qza --o-visualization AmericanGut-demux-single-end.qzv  

#record the time and date at the end of the job
date    

```

### <a name="Submitting jobs">Submitting jobs</a>
jobs are submitted by using the sbatch command 
```{bash eval=FALSE}
sbatch your_script_name.sh
```

You can check the status of your job with squeue. Using option -u lets you search by username 
```{bash eval=FALSE}
squeue -u janedoe
```

After the job is done running you can check the log to see if there were any errors (slurm-JOB_ID.out)
