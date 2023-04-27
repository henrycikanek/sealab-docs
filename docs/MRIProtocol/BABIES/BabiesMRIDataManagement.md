---
layout: default
title: BABIES MRI Data Management
parent: Babies
grand_parent: MRI Protocol
nav_order: 8
---

# BABIES MRI Data Managment

## Setting up Freesurfer on your personal computer 

1. Download FreeSurfer on personal Mac using this link – make sure to click the PKG installer at the bottom. https://surfer.nmr.mgh.harvard.edu/fswiki/rel7downloads. Within FreeSurfer you will be able to use FreeView for editing the segmentations (see NIBABIES Protocol above)

2. After FreeSurfer is installed. We want to open up an application called Terminal. Find Terminal under Applications/Utilities/Terminal or Press “Command” & “SpaceBar” at the same time to search for it. 

3. Once opened, find Preferences, under Terminal. Look at shells open with. Click command, enter “/bin/bash”. Now press Command + Q to Quit Terminal

4. Reopen terminal. It should say “Bash” at the top of the window. (You won’t see the Freesurfer information yet at the top - Step 5 will add this)

5. Next, type in “pico .bash_profile” and push enter. This command will open up a profile where we can make it run the correct Freesurfer code automatically every time you open terminal. Type in the following commands.

6. NOTE: We want to verify the file path of Freesurfer (this will change the code above). Go to your Applications folder. There should be a Freesurfer folder if all was installed correctly. Open that folder. If you see a single folder with the Freesurfer version number, make sure to include it in the code for step 5. For me, my version number is 7.1.1 so I include that at the end of the path. If you open Applications/freesurfer and find a variety of different files and folders, do NOT include the version number in your profile. 

7. Below is for you to copy and edit as needed:

        export FREESURFER_HOME=/Applications/freesurfer/7.1.1
        source $FREESURFER_HOME/SetUpFreeSurfer.sh

        export PATH=$PATH:/Applications/freesurfer/7.1.1/Freeview.app/Contents/MacOS

8. Press Control + X then Y to exit. Close out of freesurfer again using Command + Q.

9. Reopen. You will know FreeSurfer is set up correctly if this is what your terminal looks like. (Must say bash and must be have a freesurfer environment” You should be free to use Freesurfer scripts for NIBABIES.

10. Lastly, you will need a “license file” from the website where you downloaded freesurfer. https://surfer.nmr.mgh.harvard.edu/registration.html Go to this site and enter your information. They will send you an email with a license.txt file and instructions on where to put it. For me, I dragged and dropped into the FREESURFER_HOME folder. This would be /Applications/freesurfer/7.7.7/ and everything should be good to go!

## Transferring data from gstudy to lab server

1. ALL OF THIS MUST BE DONE ON WHALE
2. Log in to gstudy (https://theta.vuiis.org/gstudy/) with personal VUnetID and password.
3. Type in HUMPHREYS* under Study Name, select both MRI and MRI (Classic) under Data Type, and search for data by scan date (get the scan date from the SEA Lab Google calendar). Click search.

4. On the next screen, make sure that all scans are checked, and check only the DICOM box. 
5. Select Keep Original Types, and make sure XMLPARREC, NIFTI, and DICOM viewer tool, are all unchecked.
6. Select prepare package


7. Once the package is prepared, click download.

## Nibabies organization

### ALL OF THIS ALSO MUST BE DONE ON WHALE

1. Nibabies tracker: 

        https://docs.google.com/spreadsheets/d/1fqyfMgPkQH1fCaL-EPORacOI2icr9Hs3iybLUOSJdv8/edit#gid=0

2. Once you have downloaded the data from gstudy, take the DICOM folder for that subject and move it to Whale’s desktop.


        Rename it to sub-XXXX-nb or sub-XXXX-6mo where XXXX is the subject ID
3. All nibabies processing takes place locally on whale, so move the subject to Documents > nibabies_newborn/nibabies_sixmonth > sourcedata
4. Once in the sourcedata folder, the subject can be organized.
    1. Open a terminal at the nibabies_code folder (which is in Documents on Whale).
    2. Type in this command, where you are choosing nb or 6mo based on the subject's timepoint

            python3 org_sub.py sub-XXXX-nb XXXX newborn or python3 org_sub.py sub-XXXX-6mo XXXX sixmonth

    3. Hit enter, and you should see the organization script begin to run.
    4. You’ll know the script is complete when the terminal resets itself and allows you to type again. 
5. Once the organization script is complete, you need to go back and check to make sure it ran correctly.
    1. To do this, scroll up in the terminal to where you see the files being routed to a certain folder (some of the files will have “Not paired” next to them).
    2. Scroll through the files that were not paired and make sure that none of them are files that we need
        - Some files that we do not need are files that have SHORT, SURVEY, pCASL, subtraction, and perfusion in the name.
    3. If any files other than the ones listed above do not get paired, that is a problem because we probably need those scans.
6. If files that we need did not get paired, open up the config.json file in Documents > nibabies_code. This file shows what files are getting mapped to where. Note: you only need to do this if the organization does not run correctly.
    1. For example, 



    would indicate that files with “3D_T2” in the name will be put into the anat folder and labeled as T2w.
7. If you have a file that you need not get paired, look to see if any of the series descriptions in the config file match the name of the file you need to pair. 
    1. If there is not one, that is the reason your file is not pairing and you need to add it to the config file. 
    2. Do this by simply copying and pasting exactly what is shown in the image above and adding part of your file name into the series description
        - Make sure that you are copying and pasting the correct file type (i.e., if the problem file is B1 map, make sure you scroll down the config file until you find the B1 map descriptions and copy that one).
    3. Once you add a command to the config file, save it.
8. Before running the organization script again, go back to Downloads > nibabies_newborn/nibabies_sixmonth > tmp_dcm2bids and delete the folder that was created for this subject. Also navigate to the bids folder and delete the folder for the subject.
    1. Once those are deleted, run the organization script again and you should see that the file pairs correctly.
9. Another common type of error is the Re-ordered slice out of volume, which looks like this. On the nibabies tracker there is a second tab labeled error log. Note in this log what subject ID got the error, but other than that you don’t have to do anything. 

## Quality Checking

1. Once the data is organized, go into the Whale’s documents > nibabies_newborn/sixmonth > bids to find the organized data
2. Use Freeview quality-check the data.
    1. Open Freeview, then select File, Load Volume, and click the little folder icon to navigate to the structural scan in the subject’s bids folder.
3. For structural data, delete any scan that is very bad (doesn’t even look like a brain):

4. If there are several runs of a scan, look at each one. If any of them is clearly better than the others, delete the inferior scans. If there are several runs but all of them look pretty equal, keep them all. 
5. Some examples:
    1. A good T1w: 

    2. A  good T2w: 

    3. A borderline T2w – if there was another T2w that is better, you’d delete this one. If this is your best option, it is okay to keep.
    4. A good Blackford T2w: To quality check a blackford in Freeview you need to go to the 4-panel view. One of the panels will have the full scan and 3 will just have fragments.
        - A poor-quality Blackford will jump side-to-side as you scroll through, and you’ll see some stripes/rings as well.

## Sending participant souvenir scan images

1. Open Freeview.
2. Select File > Load Volume.
3. Load best quality T2 file in greyscale
    1. You can find these on Whale under Documents > nibabies_newborn/sixmonth > bids > sub-XXXX > ses-newborn > anat
4. Scroll through slices in all three views (i.e., sagittal, coronal, and axial) until you find a good slice, and then screenshots of all three.
5. Combine them into one picture file named as ID_timepoint_scan.png.
    1. The easiest way to do this is to open all three screenshots, line them up next to each other on the desktop, and take another screenshot with all three brains in it.



5. Under colormap (on the left column), change to PET.
6. Adjust min and max contrast to optimal color.
    1. Max should be significantly higher than min so you get a nice yellow/blue color with a black background.
7. Similar to greyscale images, take screenshots in each view and combine them into one picture file named as ID_timepoint_scan_color.png.

8. Return to greyscale and select the sagittal view only
9. Scroll to a point where the brain is very small/disappears
10. Screen record yourself scrolling through the entire brain
    1. Screen record is shift command 5, and to stop recording do shift command 5 again and click the square icon
11. Use the video thumbnail that populates to trim the video to only include the footage of you scrolling through the brain
12. Name the video ID_timepoint_video.
13. On the whale desktop create a folder called “souvenir_images” and save both images and the video here. If you have several souvenir images folders on the desktop at the same time make sure you add the subject ID in the folder name to keep track of each.
    1. This is a bit unclear in the tutorial video. This souvenir images folder will eventually get moved to the server once the subject has been processed (in processed_nb). You cannot, however, save the souvenir_images folder in the bids folder before processing–the pipeline does not recognize the files and will error out. 
14. Log in to SEA Lab gmail and send participant the souvenir images and video using the following email template:

        "Hello [Participant Name],

        Thank you so much for participating in the newborn phase of our study! Your baby was amazing and we appreciate your patience throughout the process. As a small thank you, we have attached some photos from [his/her] scan to commemorate the experience. We look forward to seeing you and your child again when [he/she] is 6-months!

        Kind Regards,
        [Your Name, Your Title]

15. Make sure to attach the two image files and the video file!

## Nibabies command

1. Once your subjects are organized, you can run the nibabies command.
2. First, ensure that Docker is open on Whale.
    1. Just click this icon 
3. Then, open a terminal at the nibabies_code folder and - for newborn - type in 

        python3 nibabies_wrapper.py docker /Users/sealab/Documents/nibabies_newborn/bids /Users/sealab/Documents/nibabies_newborn/derivatives participant --age-months 1 --fs-license-file /Applications/freesurfer/7.1.1/license.txt --cifti-output --nprocs 4 -w /Users/sealab/Documents/nibabies_newborn/work --participant-label sub-1008

and replace the participant label with whatever participant you are working on.
4. As of 2/28/22 we are getting an error because of the cifti output, so we are using a different command to exclude cifti:

        python3 nibabies_wrapper.py docker /Users/sealab/Documents/nibabies_newborn/bids /Users/sealab/Documents/nibabies_newborn/derivatives participant --age-months 1 --fs-license-file /Applications/freesurfer/7.1.1/license.txt --nprocs 4 -w /Users/sealab/Documents/nibabies_newborn/work --participant-label sub-1008
5. Watch for a few minutes to make sure the script gets up and running, and then you can let it be
6. The script will take several hours to run
7. For six month data, use the same command but replace newborn with sixmonth every time it shows up.
8. If you want to batch process (meaning process multiple subjects at once), you can use the above commands, but just take out the –participant-label sub-XXXX at the end. Doing this will prompt the script to process all subjects that are in the bids folder. If there are files in the bids folder that you don’t want to be processed, move them to the in_queue folder before running the command.


9. If you get an error like this, it means that some files are in the wrong place or are named incorrectly. Read what the error message says.
    1. For this specific error, there were bval and bvec files in the fmap folder, which is incorrect. 
    2. To correct it, we just deleted those files from the fmap folder and the script ran correctly.
10. If a subject only has structural data, you will get an error message saying “No BOLD images found for participant XXXX and task <all>. All workflows require BOLD images.” 
    1. Just move this subject out of the BIDS folder – subjects with only structural data can be processed directly on infant freesurfer and don’t have to go through nibabies
    2. make note of this subject on the nibabies tracker so that you know it hasn’t been processed.
11. You may also get an error that says that a json sidecar is not accompanied by a data folder. This usually happens when a resting-state scan is so short that a nifti file doesn’t get made. To resolve this, just navigate to the unaccompanied json file in the bids folder and delete it.

## Post-processing

1. There are a couple of things you need to do once the subject has been successfully processed to set up for editing the surfaces as well as 3D printing.
2. First, use the edit_surf command explained below to open the processed file
    1. When you enter this command into the terminal and hit return, you will likely see a message prompting you to co-register a T2 image. 
    2. To do this, open a terminal window at the nibabies_code folder and enter ./co_reg_T2 <session> <subID> <file path to T2w file>
        * For example, ./co_reg_T2 newborn sub-1034 /Users/sealab/Documents/nibabies_newborn/bids/sub-1034/ses-newborn/anat/sub-1034_ses-newborn_run-04_T2w.nii.gz
        * NOTE: an easy way to get a file path without having to type it all in is to open a new terminal window and drag into it the folder/file to which you want to map. The file path will show up which you can then copy/paste into the terminal that you’re working in
    3. Once you run the co_reg_T2 command, it will create a coregistered_T2 file in the derivatives folder (nibabies_newborn/sixmonth > derivatives > sourcedata > infant-freesurfer > sub-XXXX)
        *Even if you are not the person editing the subject, still co-register the T2 image once processing is done so the editor does not have to do it.
3. Next, in order to 3D print the brain you need to make a .stl file from the pial file that nibabies spits out.
    1. To do this, navigate to and open a terminal at the anat folder inside the derivatives folder of the subject you just processed (nibabies_newborn/sixmonth > derivatives > sub-XXXX > ses-newborn > anat)
    2. In that folder you should find files titled sub-XXXX_ses-newborn_hemi-L_pial.surf.gii and sub-XXXX_ses-newborn_hemi-R_pial.surf.gii
    3. Duplicate both of these files and rename the copies lh_pial.gii (for the left hemisphere) and rh_pial.gii (for the right hemisphere). 
4. Then, in your terminal that is opened at the anat folder, enter mris_convert lh_pial.gii XXXX-lh-nb.stl and hit return. This will generate a .stl file for the left hemisphere inside that anat folder. 
5. Do the same for the right hemisphere by entering mris_convert rh_pial.gii XXXX-rh-nb.stl
    1. In this command, the first file name is your input pial file, so it must match exactly whatever you have named the pial file inside the anat folder. The second file name is what the script will name the output stl file.
6. Once you’re done with everything, the data can be moved to the server so we don’t run out of storage on whale
    1. Navigate to the SEALab_Projects > BABIES > MRI > Nibabies and you’ll find the same folders as there are on whale. 
    2. Move all files from the documents folder on whale to the server
        * bids → processed_nb
        * derivatives → derivatives
            1. inside the derivatives folder there is a sourcedata folder. Move the subject’s folder in here onto the same folder on the server
       * sourcedata → sourcedata
        * You can delete the folder that gets created in tmp_dcm2bids and work. These do not need to be moved to the server.



