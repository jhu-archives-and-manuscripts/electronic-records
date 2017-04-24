# electronic-records
Electronic records accessioning workflow

Note: The following should be considered a "scratchpad" for notes relating to the evolution of electronic records accessioning workflows.  Please consult with Lora to confirm the accuracy and completeness of these notes at any given time.

**Visual Overview**

For a helpful graphical representation of the general steps for accessioning electronic records, see the <ac:link><ri:attachment ri:filename="20160427born_digital_workflow.jpg"><ac:plain-text-link-body></ac:plain-text-link-body></ri:attachment></ac:link>. 

**General Assumptions**

*   BitCurator/FRED
    *   Generally speaking, these instructions assume you are booting into BitCurator on the FRED workstation.
*   Using the terminal
    *   When providing examples of a file path, a syntax such as /path/to/file has been used.  Change this to whatever is appropriate.
    *   In example file paths, information supplied within square brackets [ ] should be replaced with the relevant text.
    *   When needing to navigate to a file path where a directory of file name includes spaces, these spaces can be escaped with "\ ".  So, for example, JHU Client Drive should be entered as JHU\ Client\ Drive.
    *   If preferable, file paths do not need to be typed out.  You may alternatively drag and drop the desired directory into the terminal window to type out the path to that directory.
*   File management  

    *   Naming conventions for accession folder:

        *   Folders containing individual files (not disk images) should be given titles consisting of the accession number followed by **-bag** (e.g. 2012-13.ua.028-bag).

        *   Folders containing disk images should be given titles consisting of the accession number followed by **-unproc-images-bag** (e.g. 2011-12.ms.005-unproc-images-bag).

    *   It is assumed that new accessions will be stored in appropriately named accession folders (as described above) on the FRED's RAID (/media/bcadmin/RAID#1) while in process.  If the files are stored elsewhere, amend the file paths provided below.
    *   Similarly, it is assumed that bagged and tarred accessions will be transferred to the electronic-records folder on the sam mount (/mnt/sam/archiveimages/electronic-records), but if this is not the case, amend the examples provided below.  

**Mounting the GDrive in BitCurator**

The GDrive is _not _set to automatically mount on startup within BitCurator, however, a shell script has been written to accomplish this task.  To run the script:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="3f0da204-7f2c-4995-b9f8-12922b32cc81"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

**Disk Imaging** (_Physical media only)_

Image with [Guymager](http://wiki.bitcurator.net/index.php?title=Creating_a_Disk_Image_Using_Guymager).  See <ac:link><ri:page ri:content-title="Disk Imaging"></ri:page></ac:link> for additional guidance.

**Virus Check**

Check with [ClamTK](http://www.clamav.net/) in BitCurator.  See <ac:link><ri:page ri:content-title="Scanning Electronic Records for Malware"></ri:page></ac:link> for additional guidance.

**Print directory ** _(Optional, but strongly recommended)_

Directory listings provide a textual listing of the full contents of the accession, and should be generated in the BitCurator environment using [tree](http://linux.die.net/man/1/tree).  Within BitCurator, open Terminal and type:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="af77f9b8-2533-4cb8-acfc-6c3380125a47"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

Tree provides a number of different options, but those above (-sDno) are likely to be the most relevant in most cases.  They accomplish the following:

<table>

<tbody>

<tr>

<th>Option</th>

<th>Action</th>

</tr>

<tr>

<td>-s</td>

<td>prints the size of the file in bytes</td>

</tr>

<tr>

<td>-D</td>

<td>prints the last modified date of the file</td>

</tr>

<tr>

<td colspan="1">-n</td>

<td colspan="1">defaults to black and white output (no color coding)</td>

</tr>

<tr>

<td colspan="1">-o</td>

<td colspan="1">outputs to the filepath immediately following the options (as opposed to printing the results to the terminal screen itself)</td>

</tr>

</tbody>

</table>

**Bag**

As of June 2016 we now use the [bagit-python module](https://github.com/LibraryOfCongress/bagit-python).  Open BagIt a terminal window, and enter: 

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="173d8971-33c7-4986-a0df-cb1e5b7d13b9"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

The tags above are the minimum required for our bags, but others may be added as needed/relevant.  To see a full list of the available tags that can be assigned and added to the bag-info.txt file you may use:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="e6dc3600-ea75-4e22-a312-8ce06e85ceed"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

After the bag is created, verify the generated bag with the following:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="780ab760-6c34-4439-b2f0-c24fba39660a"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

After creating the bag, make a copy of the manifest-md5.txt file and move it to the appropriate accession folder on the G: drive.  Append the accession number to the front of the file name, changing the name to something like "2016-17.ms.001-manifest-md5.txt".

**Tar**

After generating and verifying the bag, the directory should be [tarred](http://linuxcommand.org/man_pages/tar1.html) in preparation for transfer to the SAM.  Open the terminal and type:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="017f2098-2bff-4497-b2bf-2ec7209bdf6b"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

If the bag contains unprocessed disk images the above should be amended to:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="1660adf5-fe92-40b3-9104-e0f9e390d20a"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

Again, there are many options that may be set, but the following should be most appropriate to our uses:

<table><colgroup><col><col></colgroup>

<tbody>

<tr>

<th>Option</th>

<th>Action</th>

</tr>

<tr>

<td>-c</td>

<td>creates a new tar file</td>

</tr>

<tr>

<td>-v</td>

<td>common verbose option which provides detailed information in the terminal screen as the command is run</td>

</tr>

<tr>

<td>-f</td>

<td>allows you to specify the file name of the bag to be tarred</td>

</tr>

</tbody>

</table>

**Rsync**

**NOTE:** If you will be transferring more than 1TB of content at any one time, please alert <ac:link></ac:link> to ensure sufficient tape storage is available!

While it is possible to simply drag and drop the tarred bag from the FRED's RAID to the SAM, this is less than desirable (especially for larger bags) because all progress will be lost if, for some reason, the connection with SAM is lost during the transfer.  Therefore, it is best to use [rsync](http://linuxcommand.org/man_pages/rsync1.html) to transfer files.  Open a terminal window within BitCurator and type the following:

<ac:structured-macro ac:name="code" ac:schema-version="1" ac:macro-id="99b61cc2-023d-435e-9ab5-07d1b0c93e79"><ac:parameter ac:name="language">text</ac:parameter><ac:plain-text-body></ac:plain-text-body></ac:structured-macro>

Again, there are many rsync options that may be set, but those above should be most relevant in our use case.  They do the following:

<table>

<tbody>

<tr>

<th>Option</th>

<th>Action</th>

</tr>

<tr>

<td>-a</td>

<td>recurses and preserves nearly everything in the directory</td>

</tr>

<tr>

<td>-v</td>

<td>common verbose option which provides detailed information in the terminal screen as the command is run</td>

</tr>

<tr>

<td>--progress</td>

<td>prints information showing the progress of the transfer in the terminal window (especially useful for large transfers)</td>

</tr>

</tbody>

</table>

**Event records in ArchivesSpace**

ArchivesSpace events are based on [PREMIS Event Types](http://id.loc.gov/vocabulary/preservation/eventType.html).  All electronic records events should be recorded as ArchivesSpace events records as follows:

<table>

<tbody>

<tr>

<th>Event Type</th>

<th>Usage</th>

</tr>

<tr>

<td>capture</td>

<td>Records the moment the repository actively obtains a digital object.  
Specific examples: Records service (cloud, email, etc.) used to transfer the object; additional documentation relevant to the transfer. **Not** used to document the transfer of the digital object to preservation storage (see: ingestion).</td>

</tr>

<tr>

<td>compression</td>

<td>_(rarely used)_  
Records when and how a digital object is compressed.</td>

</tr>

<tr>

<td>creation</td>

<td>_(rarely used)_  
Records the creation of a new object.</td>

</tr>

<tr>

<td>deaccession</td>

<td>

_(rarely used)_

Self explanatory.

</td>

</tr>

<tr>

<td>decompression</td>

<td>_(rarely used)_  
Records when and how a digital object is decompressed.</td>

</tr>

<tr>

<td>decryption</td>

<td>_(rarely used)_  
Records the conversion of encrypted data to plain text.</td>

</tr>

<tr>

<td>deletion</td>

<td>_(rarely used)_  
Records removal of a digital object.</td>

</tr>

<tr>

<td>digital signature validation</td>

<td>

_(rarely used)_<span> </span>

Records that a digital signature matched the expected value.

</td>

</tr>

<tr>

<td>fixity check</td>

<td>

_(rarely used)_<span> </span>

Records that an object has not changed in a given period.

</td>

</tr>

<tr>

<td>ingestion</td>

<td>

Records that a digital object has been transferred to a preservation repository.

<span>Specific examples: Used to record the moment a tarred and bagged accession is transferred to **SAM storage**.</span>

</td>

</tr>

<tr>

<td>message digest calculation</td>

<td>

Records the creation of a message digest ("hash").

<span>Specific examples: Used to record the **bagging** of a digital item.</span>

</td>

</tr>

<tr>

<td>migration</td>

<td>

_(rarely used)_

Records the transformation of an object to a different format. Differs from normalization because the migration happens due to pending threat of format obsolescence.

</td>

</tr>

<tr>

<td>normalization</td>

<td>

Records the transformation of an object to a more preservation-worthy version.

<span>Specific examples: **Normalizing** proprietary Outlook PST to MBox using ReadPST.</span>

</td>

</tr>

<tr>

<td>replication</td>

<td>

Records the creation of a bit-level copy of a digital object.

<span>Specific examples: Used to record the creation of a **disk image**.</span>

</td>

</tr>

<tr>

<td>validation</td>

<td>

Records the outcome of comparing a digital object to a given standard and noting compliance or exceptions.

Specific examples: Used to record format **validation** such as that done via JHOVE.

</td>

</tr>

<tr>

<td>virus check</td>

<td>

Records the outcome of a virus scan.

<span>Specific examples: Used to record **virus scan**. Record a "pass" value for this event type.</span>

</td>

</tr>

</tbody>

</table>
