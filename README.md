# ZoomAttendanceTool
AWK script (and auxiliary batch commands) to capture Zoom meeting attendance and duration of participation
Version 1.1 (Sept. 12, 2020)

[Click here](https://bit.ly/35oReVv) to download the tool itself (a ZIP file); the documentation is included there as well.

* _Please email me if you have any questions/requests (e.g., seeing outputs in a different format) or spot any mistakes or unclear points in the documentation or bugs in the tool._
* _Also, let me know if you come across a similar tool._
* _A Canvas version of this tool might be do-able, if there's demand._

<!-- Best printed from Firefox to remove URL -->
<!-- Nah, just print from C:/Temp in Chrome -->

## Introduction

Jay's **Zoom Attendance Tool** (aka AttendanceTool) provides a report containing:
1. A list of absent students.
2. A list of Zoom meeting attendees and the duration (in minutes) of each attendees' total presence.

The full tool uses two input CSV files:
1. An _extract_ of just the names in your course's participant list Excel file ("Class list").
   * _A more simplified report on just the attendee times does not require this file._
2. The Zoom participant list for your meeting ("Zoom list"), downloadable from Zoom.

Instructions for obtaining these files appear later below.

### Preliminary notes and warnings
* In order to have Zoom produce a meeting participant list, I believe the meeting has to have been scheduled (i.e. not an adhoc meeting).
* My tool works best when **accents** in the input files are replaced by basic, non-accented letters. I cannot guarantee the tool's accuracy if accents are left in. The tool might be modified in the future to handle accents, if there's demand.
* This document provides instructions for three different computer user levels, based on familiarity of Zoom, data formats, and script running. These levels are  [non-expert](#non-expert), [expert](#expert), and [super-expert](#super-expert).
* But first, read the next section describing the output report.

<div style="break-after:page"></div>
## <a name="Output"></a>Output

The default output ("full report") is shown on-screen and also saved in a file named `Report_Full_Absence_and_Time.txt`.

### Attendance -- absence

* The 1st part of the output report shows which students were absent.
* The tool takes the "Class list" and matches names in the "Zoom list" using all parts of each full student's name.
* If a student is confirmed to be absent, the tag `ABSENT:` will appear by their name.
* If a student *might* be absent (due to a Zoom name's matching multiple people), the report will show the tag `ABSENT?` next to the name.
  * This can happen if students use only parts of their full name for their Zoom screen name and whatever they use in Zoom happens to match multiple people in the Class list.
  * Dealing with these cases are discussed below.

### Attendance -- time

* The 2nd part of the report shows the total duration of each student's participation (in minutes), sorted by how long they were in meeting.
* Each line in this part is formatted as follows:
`<time>,<class name>,<other info>`
where `<other info>` can include the Zoom name(s), unmatchable names, and possible names. For example:

1. `122,Mike Wagner,(Zoom name:Wag)` -- means Mike Wagner in the Class list attended for 122 minutes. The Zoom name used was `Wag` and that name did not match any other names in the Class list.
2. `120,Charles,(POSSIBLE class names: Charles Rhoades, Charles Brown)` -- means you had a Zoom attendee named `Charles` but two people in the Class list have that 1st name. Thus, you will need resolve which Charles this was. Also, the absence part will show `ABSENT? Charles Rhoades` and `ABSENT? Charles Brown`.
3. `90,Kate Sacker,(Zoom name: Kate, K.Sacker)` -- means that Kate Sacker had two separate entries in the Zoom list, possibly because her connection was broken and she re-entered the meeting with a different name. Also, she attended only 90 minutes (of my two-hour course) and so I might want to check the Zoom file to see if she was late or left early. Also, there is no one else in the class named 'Kate' or 'Sacker'.
4. `135,Taylor Mason,(UNKNOWN: Cannot find in Class_list.csv)` -- means you had an attendee whose name does not appear or cannot be matched to anyone in the Class list. This might be the instructor's name or a student's nickname that's not recorded in the Class list.
    * For cases #3 and #4, some students may use [adhoc/nick names](#nicknames), and also their use of these names may change slightly week to week (unless you force them to use Zoom registered names).
    * You can avoid these situations by adding any known nicknames in the Class list file (see the [nick names](#nicknames) section).
    * OR asking students to use Zoom names that will match the Class list. Usually, including both first and last names will do the trick.
* The tool will sum up duration times for each entry (using matched names if possible). 
    * Sometimes students may leave the room or their connection breaks and then come back. 
    * Sometimes they come back using a different name, which I've seen happen if they change devices.
    * If you need to see the exact entry and exit times, examine the downloaded Zoom participant CSV (in Excel).
	
* Minor note: Some of the lines in the report start with a letter like 'a', 'b', 'c' ... 'z'. These are there for sorting purposes.

Now, you can jump to one of these subsections: [non-expert](#non-expert), [expert](#expert), or [super-expert](#super-expert).

<!-- <div style="break-after:page"></div> -->
## <a name='expert'></a>Expert instructions

1. Unzip the `AttendanceTool.zip` file into some location you can execute programs. See [below](#eur_laptop) if you have an EUR maintained laptop, as there are restrictions on the folders from which you can execute programs you install yourself.
2. Obtain and rename your Zoom meeting's participant list (Excel) to `Zoom_list.csv`.
3. Extract the full, middle, and last names columns from your course's class/participant list and save as `Class_list.csv`. Best to filter for your own tutorial, if applicable.
4. Copy or move both into the same folder as the `AttendanceTool`.
5. Double click on the script for your OS (Windows or Mac). See [Running AttendanceTool](#running).

<div style="break-after:page"></div>
## <a name="non-expert"></a>Non-expert instructions

### Some technical background:
* The input files -- that you'll obtain using instructions below -- are CSV files, which stands for "comma separated value" files. These are comma-delimited text files that Excel can read/produce and have the file extension `.csv`.
* A "text file" is like a Word document, but much more simplified, containing only basic letters, numbers, and punctuation.
  * These can be easily opened in Word. However, the default program for opening these (by double-clicking) is usually different depending on the OS. On Windows, it's Notepad. On Macs, I think it's TextEdit.
  * An unstructured text file (i.e. not a CSV) usually has the file extension `.txt`.

### Preparation:

1. Unzip the downloaded ZIP file
   * This usually entails double-clicking on it from the Downloads folder and copying its contents to somewhere on your computer.
   <a name="eur_laptop"></a>
   * If you have an EUR-maintained laptop and you do not have Local Administrator privileges, the program may only run from a specific folder:
     * This folder is under your Documents folder, called "NO-APP-CONTROL". If it doesn't exist, then create it and copy the contents of the ZIP archive into there
     * If you do have Local Admin privileges, you should be able to run the program from "NO-APP-CONTROL" and also anywhere under "Program Files". It is unlikely you'll be able to run the program from just anywhere else, even with Local Admin privileges, unless your laptop is non-EUR (please let me know if your experience is different).
   * Note that the top level folder for the tool itself is called `AttendanceTool`.

2. _"Zoom list"_: Obtain the meeting participant list CSV file from Zoom:
   a. In your Zoom account, go to **Reports** -> **Usage**
   b. If you don't see the meeting, choose the date range appropriately.
   c. Scroll to the right and click on the number that appears under the column _Participants_.
   d. In the dialog window that opens, make sure _'Show unique users'_ is **unchecked**
      * The tools should work whether or not _'Export meeting with data'_ is checked (but best to uncheck it anyways).
   e. Click on the **Export** button.
   f. Rename (or copy) the downloaded file as "**Zoom_list.csv**".
      * Note that the filename is case-sensitive for Macs.
	  <!-- * If you think you can handle running the tool from the command line (Terminal in Macs), you can name this file whatever you want. -->
   g. Copy or move **Zoom_list.csv** into the `AttendanceTool` folder.
	  
2. _"Class list"_: Create a CSV file containing the list of names of all the students in the course/tutorial:
   1. Open the participants Excel XLSX file for your course.
      * You might want to use the column filter to show only your tutorial.
   2. Open a separate blank workbook (Win: `Ctrl+N`, Mac: `Command+N`)
   3. Copy/paste just the "First name", "Middle name", and "Last name" columns to the new workbook.
      * It is not critical whether or not you include the headers.
   4. Save the new workbook as _CSV (Comma-delimited) (\*.csv)_ under the name "**Class_list.csv**".
      * The tool should also work if you save as _CSV UTF-8 (Comma-delimited) (\*.csv)_
   5. Copy or move **Class_list.csv** into the `AttendanceTool` folder.

<!-- <div style="break-after:page"></div> -->
## <a name="running"></a>Running AttendanceTool

* _**WARNING:**_ If your laptop's regional settings are European, the columns in your Class list file will be semicolon-separated and not comma-separated. Use the `semicolon` versions of the below commands.
* Now that the files are in place, you just need to double-click on one of the scripts.
   * For Windows laptops:
     * `Windows_Full_Report` for the full report described in [Output](#output).
   * For Mac laptops:
     * `Mac_Full_Report`
* There are some older execution scripts that shows the absence and time separately. E.g., if you only want to see attendance times and do not want to create a Class list CSV.
   * Note that these reports are considered V1.0 (first version) of the tool and may not behave as well as the full report.
     * `Windows_Time_Report` for a report on total participation duration for each attendee. This list will be automatically sorted from lowest to highest.
     * `Windows_Absence_Report` for a report on those present and absent.
     * `Mac_Time_Report` 
     * `Mac_Absence_Report`
     * Remember: the outputs for these will written to the console as well as a separate text file, e.g., `Report_Time.txt` or `Report_Absence.txt`.
	 
## Improvements and notes

<a name="nicknames"></a>
* Student nicknames:
  * You can easily add nicknames/alternate names that students may use on Zoom into the Class_list.csv file.
  * Just include the name within either the First name or Last name column:
    * For example: "Robert" (First name) "Axelrod" (Last name) could contain "Bobby Robert" as the first name or "Bobby Axelrod" as the last name, so when he uses "Bobby" on Zoom, this will match.
<!-- * Lower-casing: -->
<!--   * To facilitate matching in the absence report, all names will appear lower-cased for the absence report. This is the default behavior of the tool. However, through the super-expert approach, names can appear properly capitalized. -->
* Time Report matching:
  * The time report -- run by itself and not as full report -- does not take into account attendees who re-enter the room with a slightly different name.
  * You'll have to watch out for these.
  * This is taken care of in the full report.
* Entry/exit times: The entry exit times could also be included in the report, if there's demand for it.
* Accents: The tool could be modified to handle accents.

<div style="break-after:page"></div>
## <a name='super-expert'></a>Super-expert - Running from command-line

1. Follow the [expert instructions](#expert) for obtaining the files BUT
2. For super-experts, the input file names can be whatever you want.
3. Just run the script executable with the appropriate arguments/parameters. See below for your OS, but the Mac section has some details that apply to both.
* You can also have the output filename be whatever you want and also capitalize names in the absence report.

### Mac OSX

1. Open the Terminal application.
2. Change folder (using `cd` command) to where AttendanceTool is.
3. Run the following (remember to substitute the arguments):

For the "full report", outputted to the Terminal, type into the Teriminal prompt:\
`awk -f AttendanceTool.awk -v mode=both -v classListFile=<ClassFile> <ZoomFile> | sort -n`

* `awk` is the script interpreter that should come with all Macs  
* `mode` should be `both` for the full report
* `<ClassFile>` can be any name you designated for the 3 column participant CSV (e.g., Class_list.csv). If you leave out `-v classListFile= ...`, the script assumes the file is named Class_list.csv.  
* `<ZoomFile>` is the participant CSV you downloaded from Zoom, e.g., `participants_782414819.csv`. This file needs to be included in the command-line.
  * If you renamed this to Zoom_list.csv, then it would appear as `Zoom_list.csv` on the command-line.
* `| sort -n` pipes to output to be sorted numerically.

<!-- * You can rename the output/report file by including, `-v outputFile=<some name.txt>` in the command -->
* To output either to a file and not on-screen, just redirect the output at the end of the command-line, e.g,:\
   `awk -f AttendanceTool.awk ... > Week1_Absence.txt`\
   will write the output to the `Week1_Absences.txt` text file (`...` denotes the rest of the arguments).
   
<!-- * To have names in the "absence report" show up as capitalized, include `-v capNames=1` in the command-line. -->

### Windows

1. The Windows super-expert command-line is similar to MAC but the executable paths are required.
2. From the CMD prompt, change the folder to where `AttendanceTool` is.
3. From the Mac command above, just replace `awk` with `bin\gawk`, and `sort` with `bin\sort` and type it in the CMD prompt:

`bin\gawk -f AttendanceTool.awk -v mode=both participants_782414819.csv | bin\sort -n`

* `gawk` and `sort` is provided in the ZIP package as it does not come with Windows.

* Note that you should use a backslash `\` and **not** a forward slash `/` (Windows is annoying that way).

### Separate reports

If you want just the absence report or just the time report, you can do the following. Note that these reports are considered V1.0 (first version) of the tool and may not behave as well as the full report.

* Macs:
  * For the "absence report", outputted to the Terminal:\
`awk -f AttendanceTool.awk -v mode=abs -v classListFile=<ClassFile> <ZoomFile>`
  * For the "time report", outputted to the Terminal:\
`awk -f AttendanceTool.awk -v mode=time -v classListFile=<ClassFile> <ZoomFile> | sort -n`
* Windows: 
  * For the "absence report":\
`bin\gawk -f AttendanceTool.awk -v mode="abs" participants_782414819.csv`
  * For the "time report":\
`bin\gawk -f AttendanceTool.awk -v mode="time" participants_782414819.csv | bin\sort -n`

Thus, endeth the homily.

<div style="break-after:page"></div>
## Version History
| Version | Date       | Description |
| :-----: | :--:       | :---------- |
| 1.1     | 12/09/2020 | Added full report |
| 1.0     | 10/09/2020 | Initial release |
