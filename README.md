# Electronic records accessioning workflow

Note: The following should be considered a "scratchpad" for notes relating to the evolution of electronic records accessioning workflows.  Please consult with Lora to confirm the accuracy and completeness of these notes at any given time.

**Visual Overview**

For a helpful graphical representation of the general steps for accessioning electronic records, see the [OUTDATED Electronic records accessioning workflow](img/20160427born_digital_workflow.jpg). 

**General Assumptions**

*   BitCurator/FRED
    *   Generally speaking, these instructions assume you are booting into BitCurator on the FRED workstation.
*   Using the terminal
    *   When providing examples of a file path, a syntax such as `/path/to/file` has been used.  Change this to whatever is appropriate.
    *   In example file paths, information supplied within square brackets `[ ]` should be replaced with the relevant text.
    *   When needing to navigate to a file path where a directory of file name includes spaces, these spaces can be escaped with `"\ "`.  So, for example, JHU Client Drive should be entered as `JHU\ Client\ Drive`.
    *   If preferable, file paths do not need to be typed out.  You may alternatively drag and drop the desired directory into the terminal window to type out the path to that directory.
*   File management  
    *   Naming conventions for accession folder:
        *   Folders containing individual files (not disk images) should be given titles consisting of the accession number followed by **-bag** (e.g. 2012-13.ua.028-bag).
        *   Folders containing disk images should be given titles consisting of the accession number followed by **-unproc-images-bag** (e.g. 2011-12.ms.005-unproc-images-bag).
    *   It is assumed that new accessions will be stored in appropriately named accession folders (as described above) on the FRED's RAID (/media/bcadmin/RAID#1) while in process.  If the files are stored elsewhere, amend the file paths provided below.
    *   Similarly, it is assumed that bagged and tarred accessions will be transferred to the electronic-records folder on the sam mount (/mnt/sam/archiveimages/electronic-records), but if this is not the case, amend the examples provided below.  

**Mounting the GDrive in BitCurator**

The GDrive is *not set* to automatically mount on startup within BitCurator, however, a shell script has been written to accomplish this task.  To run the script:

```
$ cd Desktop
$ sudo ./mountSpecCollImages.sh
```

**Disk Imaging** (*Physical media only*)

Image with [Guymager](http://wiki.bitcurator.net/index.php?title=Creating_a_Disk_Image_Using_Guymager).

**Virus Check**

Check with [ClamTK](http://www.clamav.net/) in BitCurator.

**Print directory** (*Optional, but strongly recommended*)

Directory listings provide a textual listing of the full contents of the accession, and should be generated in the BitCurator environment using [tree](http://linux.die.net/man/1/tree).  Within BitCurator, open Terminal and type:

```
$ cd /mnt/GDrive/archives-and-manuscripts/accessions/[accessionnumber]
$ sudo tree -sDno [accessionnumber]-directory-listing.txt /media/bcadmin/RAID#1/Images/[accessionnumber]
```

Tree provides a number of different options, but those above (-sDno) are likely to be the most relevant in most cases.  They accomplish the following:

|Option      |Action    |
|------------|----------|
|-s          |prints the size of the file in bytes|
|-D          |prints the last modified date of the file|
|-n          |defaults to black and white output (no color coding)|
|-o          |outputs to the filepath immediately following the options (as opposed to printing the results to the terminal screen itself)|

**Bag**

As of June 2016 we now use the [bagit-python module](https://github.com/LibraryOfCongress/bagit-python).  Open BagIt a terminal window, and enter: 

```
$ bagit.py --source-organization 'jhu' --contact-name '[yourname]' --external-identifier '[accessionnumber]' /media/bcadmin/RAID#1/Images/[accessionnumber-bag]
```

The tags above are the minimum required for our bags, but others may be added as needed/relevant.  To see a full list of the available tags that can be assigned and added to the bag-info.txt file you may use:

```
$ bagit.py --help
```

After the bag is created, verify the generated bag with the following:

```
$ bagit.py --validate /media/bcadmin/RAID#1/Images/[accessionnumber-bag]
```

After creating the bag, make a copy of the manifest-md5.txt file and move it to the appropriate accession folder on the G: drive.  Append the accession number to the front of the file name, changing the name to something like "2016-17.ms.001-manifest-md5.txt".

**Tar**

After generating and verifying the bag, the directory should be [tarred](http://linuxcommand.org/man_pages/tar1.html) in preparation for transfer to the SAM.  Open the terminal and type:

```
$ cd /media/bcadmin/RAID#1/Images
$ tar -cvf [accessionnumber]-bag.tar [accessionnumber]-bag
```

If the bag contains unprocessed disk images the above should be amended to:

```
$ cd /media/bcadmin/RAID#1/Images
$ tar -cvf [accessionnumber]-unproc-images-bag.tar [accessionnumber]-unproc-images-bag
```

Again, there are many options that may be set, but the following should be most appropriate to our uses:

|Option     |Action   |
|-----------|---------|
|-c         |creates a new tar file|
|-v         |common verbose option which provides detailed information in the terminal screen as the command is run|
|-f         |allows you to specify the file name of the bag to be tarred|

**Rsync**

**NOTE:** If you will be transferring more than 1TB of content at any one time, please alert SAM Sys Admins to ensure sufficient tape storage is available!

While it is possible to simply drag and drop the tarred bag from the FRED's RAID to the SAM, this is less than desirable (especially for larger bags) because all progress will be lost if, for some reason, the connection with SAM is lost during the transfer.  Therefore, it is best to use [rsync](http://linuxcommand.org/man_pages/rsync1.html) to transfer files.  Open a terminal window within BitCurator and type the following:

```
$ cd /media/bcadmin/RAID#1/Images
$ rsync -av --progress [accessionnumber]-bag.tar /mnt/sam/archiveimages/electronic-records/
```

Again, there are many rsync options that may be set, but those above should be most relevant in our use case.  They do the following:

|Option     |Action|
|-----------|------|
|-a         |recurses and preserves nearly everything in the directory|
|-v         |common verbose option which provides detailed information in the terminal screen as the command is run|
|--progress |prints information showing the progress of the transfer in the terminal window (especially useful for large transfers)|

**Event records in ArchivesSpace**

ArchivesSpace events are based on [PREMIS Event Types](http://id.loc.gov/vocabulary/preservation/eventType.html).  All electronic records events should be recorded as ArchivesSpace events records as follows:

|Event Type     |Usage|
|---------------|-----|
|capture        |Records the moment the repository actively obtains a digital object. <br/>Specific examples: Records service (cloud, email, etc.) used to transfer the object; additional documentation relevant to the transfer. **Not** used to document the transfer of the digital object to preservation storage (see: ingestion).|
|compression    |*(rarely used)*<br/> Records when and how a digital object is compressed.|
|creation       |*(rarely used)*<br/> Records the creation of a new object.|
|deaccession    |*(rarely used)*<br/> Self explanatory.|
|decompression  |*(rarely used)*<br/> Records when and how a digital object is decompressed.|
|decryption     |*(rarely used)*<br/> Records the conversion of encrypted data to plain text.|
|deletion       |*(rarely used)*<br/> Records removal of a digital object.|
|digital signature validation|*(rarely used)*<br/> Records that a digital signature matched the expected value.|
|fixity check   |*(rarely used)*<br/> Records that an object has not changed in a given period.|
|ingestion      |Records that a digital object has been transferred to a preservation repository.<br/> Specific examples: Used to record the moment a tarred and bagged accession is transferred to **SAM storage**.|
|message digest calculation|Records the creation of a message digest ("hash").<br/> Specific examples: Used to record the **bagging** of a digital item.|
|migration      |*(rarely used)*<br/> Records the transformation of an object to a different format. Differs from normalization because the migration happens due to pending threat of format obsolescence.|
|normalization  |Records the transformation of an object to a more preservation-worthy version.<br/> Specific examples: **Normalizing** proprietary Outlook PST to MBox using ReadPST.|
|replication    |Records the creation of a bit-level copy of a digital object.<br/> Specific examples: Used to record the creation of a **disk image**.|
|validation     |Records the outcome of comparing a digital object to a given standard and noting compliance or exceptions.<br/> Specific examples: Used to record format **validation** such as that done via JHOVE.|
|virus check    |Records the outcome of a virus scan.<br/> Specific examples: Used to record **virus scan**. Record a "pass" value for this event type.|
