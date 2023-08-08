---
layout: default
title: BABIES MRI Data Analysis
parent: Babies
grand_parent: MRI Protocol
nav_order: 9
---

# BABIES MRI Data Analysis

## Setting up Design Trackpad for Editing

1. Plug the trackpad into the computer. A window will pop up asking you to identify your keyboard. Just close out this window, it is irrelevant.
2. Go to xp-pen.com. Find “Support” in the menu bar and then navigate to driver download.
3. Locate the “Star G430S” product and click on it. Download the newest version of the driver for your OS.
4. Double click on the .zip file once the download is complete. Once the file opens, open the .dmg file within. This is the installer.
5. Follow the prompts and agree to the licenses.
6. Drag the XP-PenPenTabletPro folder over to the Applications folder once the window pops up.
7. Close out the window, open the Applications folder, and then open the PenTablet app.
8. Once the app is open, navigate to pen settings.
9. Set the bottom button to custom -> left click. Set the top button to custom -> right click if you zoom in and out more. Set the top button to custom -> middle click if you like to move the brain around more. If you’re not sure yet, just set it to right click.
10. Your pen tablet should be ready to use.

## Editing Nibabies

1. To open up a preprocessed file to edit, navigate to the subject files using this file path:

        [Insert file path once Henry is done with processing data]

2. Open a new terminal and type “Freeview”
3. Drag the T1, T2, and aseg files into the terminal, hit “enter”
    * This will open the file you need to edit in freesurfer

4. To open up a preprocessed file to edit, open a terminal window at the nibabies_code folder
5. Type into the terminal 

        ./edit_surf <session> <subID>

For example, 

        
        ./edit_surf newborn sub-1098

6. This will open the file you need to edit in freesurfer
    * If you are prompted to co-register a T2, follow the steps above to do so.

## Editing Process

1. Once the files are done loading and you are ready to begin editing, click the aseg file to open the details of the file.
    1. If working on a previously saved aseg file, go to File > Load Volume > and click on the file folder icon to select the desired file. Once selected, click Open, then Ok to load the file on freesurfer.
    2. Once loaded, you can unselect the original aseg by clicking the box next to its name. As with step a, click the aseg file you just loaded to access its details.
    3. Make sure the aseg file is at the top of the loaded files. You may have to change the Color Map from Grayscale to Lookup Table to be able to see the different colored regions.
2. Adjust opacity to desired level using the slider on the left (~13 depending on preference). You can also adjust the contrast by holding shift and right click at the same time, then moving the mouse up and down.
3. Depending on which hemisphere you are wanting to edit, you will need to uncheck the regions on the side you are NOT working on as well as the white and pial files.
    1. ex: If you are working on the right hemisphere, you would want to unselect lh.white, lh.pial, and all of the regions starting with “left” in the lookup table.

    2. It’s likely a good idea to keep the other regions selected even if you aren’t editing them, just as a guide so that you don’t accidentally mark a region as grey or white matter when it is actually something else.
4. Center the brain in the editing frame by holding shift and clicking and dragging the brain image. You can also zoom in or out by scrolling up or down with the mouse or trackpad.
5. Once you are ready to begin editing, go to the top left of the screen and select the head with the green pencil . Upon selecting, the voxel edit window should appear. Feel free to drag the window to any desired spot on the screen.

    1. REMEMBER: Should you need to go back to any of the previous steps to unselect/select a region, adjust opacity, move the image, etc., you have to reselect the head with the blue arrows on the far left. Otherwise you may accidentally mark the file which would affect the data. A general rule of thumb: if you’re doing anything other than marking the brain, you should not be in editing mode. However, you are still able to (un)select the aseg file and coregistered_T2 as well as change views of the brain in editing mode.
6. Make sure the brush size value is at 1, as it is the most precise. Aside from that, you only need to focus on the Brush value and Eraser value. The regions in the lookup table each have a corresponding number, and those are the values you will enter into the Brush and Eraser value boxes. Click to apply what is entered in the Brush value and click while holding shift to apply what is entered in the Eraser value.
    1. Ex: If working on the right hemisphere, enter 42 (Right Cerebral Cortex) into the Brush value and 41 (Right Cerebral White Matter) into the Eraser value. When clicking and/or dragging, you’ll apply the right cerebral cortex RCC to voxels. Doing so while holding shift will apply right cerebral white matter RCWM.
7. If you have the coregistered_T2 selected, the regions that are darker grey around the border of the brain should be marked as cortex while the lighter grey areas that are more medial should be marked as white matter (image on the left). However if the coregistered_T2 is unselected, the white matter will be a darker gray while the cortex will be lighter (image on the right). Toggle the views to help guide your edits in areas you feel uncertain about.

8. Toggle the aseg file on and off to see the regions that are erroneously marked, and mark the cortex and white matter regions accordingly. In general, there should not be islands of cortex surrounded by white matter as the cortex exists in folds and extends into the brain in branches. Depending on the slice there may be some instances where this may not be the case every time, but be sure to check neighboring slices to see how the cortex region develops.
9. If you are confused on where the cortex ends and white matter begins, it might be helpful to look at the neighboring slices to see how the region you are editing changes across the slices. Seeing how the area develops will help you to get a grasp of where to mark the border between the two. Additionally, looking at the neighboring slices would be helpful to tell you what edits are unnecessary. If there is a minor error that fixes itself within the next 2 or 3 slices, then correcting the error on that one slice would be insignificant in the grand scheme. 
10. If you make a mark that you would like to undo, click the blue arrow at the top left of the screen that points left. If you want to re-place that mark, simply click the arrow pointing right.
11. Sometimes, it may be easier to edit certain parts of the brain from different views. You can select from sagittal, coronal, and axial views of the brain by clicking the respective options in the top bar. The best way to keep track of the spot you are editing is to exit editing mode (see e.i) and click in the region. The red crosshairs should appear where you click, and will remain in that spot when you change views (see below).
    1. Complete the majority of edits in the coronal view. To check your edits, switch to the axial view to edit the occipital pole. Switch to the sagittal view to edit the frontal pole

12. The bright white spots seen in the center and along the edges of the brain in the two above images are cerebrospinal fluid (CSF). Those spots, if marked as grey or white matter, should instead be marked with the Brush Value as 0, though it is not listed in the lookup table.
13. REMEMBER: Be sure to include a minimum 2 voxel thick region of cortex at the edge of the brain. Since the cortex completely covers the brain, no white matter should be poking out. There must always be a layer of cortex around the border. Some scan images may show white matter extending all the way to the edge, but know that this is not the case and that cortex voxels must be present.
14. Once ready to save the edits made, click File > Save Volume As > and name the file with your initials (ex: aseg_XX) before clicking save. It should automatically save the aseg into that subject’s mri file folder. 
        - If saving changes made on an already edited aseg file, select Save Volume instead.
