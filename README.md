you must use a Fiji macro to calculate the coordinates of each cell first, based on DAPI colocalization 
Here is FIJI macro
// 1. Split Channels
run("Split Channels");

// 2. Segment DAPI
selectImage("C3-edit_1_OS806_2-6_AR_CTIP2-488_SATB2-555.tif");
setAutoThreshold("Otsu");
setThreshold(14, 255, "raw");
setOption("BlackBackground", true);
run("Convert to Mask");
run("Open");
run("Watershed");
run("Analyze Particles...", "size=1-Infinity circularity=0-1.00 show=Nothing display clear add");
roiManager("Show All");
roiManager("Deselect");
roiManager("Select All");
roiManager("Save", "/Users/emmanoel/Desktop/Processed_Images_Structure/Analysis-Slices/Centroids/1_806 2-6/DAPI.zip");

// 3. CTIP2: Mask with DAPI nuclei ROIs
selectImage("C2-edit_1_OS806_2-6_AR_CTIP2-488_SATB2-555.tif");
setAutoThreshold("Otsu dark no-reset");
setThreshold(24, 255, "raw");
setOption("BlackBackground", true);
run("Convert to Mask");

// Mask: Clear everything outside nuclei
roiManager("Open", "/Users/emmanoel/Desktop/Processed_Images_Structure/Analysis-Slices/Centroids/1_806 2-6/DAPI.zip");
roiManager("Deselect");
roiManager("Select All");
roiManager("AND"); // Keep only areas inside DAPI ROIs
run("Analyze Particles...", "size=30-10000 circularity=0.10-1.00 display clear add composite");
saveAs("Results", "/Users/emmanoel/Desktop/Processed_Images_Structure/Analysis-Slices/Centroids/1_806 2-6/CTIP2_centroids_DAPIconstrained.csv");

// 4. SATB2: Mask with DAPI nuclei ROIs
selectImage("C1-edit_1_OS806_2-6_AR_CTIP2-488_SATB2-555.tif");
setAutoThreshold("Otsu dark no-reset");
setThreshold(25, 255, "raw");
setOption("BlackBackground", true);
run("Convert to Mask");
run("Watershed");

roiManager("Open", "/Users/emmanoel/Desktop/Processed_Images_Structure/Analysis-Slices/Centroids/1_806 2-6/DAPI.zip");
roiManager("Deselect");
roiManager("Select All");
roiManager("AND"); // Mask with DAPI nuclei

run("Analyze Particles...", "size=30-10000 circularity=0.10-1.00 display clear add composite");
saveAs("Results", "/Users/emmanoel/Desktop/Processed_Images_Structure/Analysis-Slices/Centroids/1_806 2-6/SATB2_centroids_DAPIconstrained.csv");
