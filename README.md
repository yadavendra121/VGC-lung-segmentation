# testing part for lung segmentation on model VSMTrans that is trained on VGC data (140 subjects)
1. Make VSmTrans environment in Python 3.11
2. Install the VSmTrans package in that environment. The installation guide for VSmTrans can be found here- https://github.com/QZBai/VSmTrans.
	
3. Create three folders nnUNet_raw, nnUNet_preprocessed, nnUNet_results. Then define three variables by using the following commands on the terminal:
	export nnUNet_raw=< absolute address of nnUNet_raw folder>
	export nnUNet_preprocessed=< absolute address of nnUNet_preprocessed folder>
	export nnUNet_results=< absolute address of nnUNet_results folder>

4. Download the model weight and place it in the nnUNet_results folder. I have given the id 011 and named Dataset011_VGC, it can be found at the given link below:
	https://www.dropbox.com/scl/fo/tkaowcgtvstsazcfhsvyj/ACoaHVhMSMXoM-LtVvsdicw?rlkey=3k17tolzi9o4t0bq0yrl3zpes&st=a913budh&dl=0

5. Put your data, which you want to segment, in as <Name_of_image>_0000.nii.gz format in the folder nnUNet_raw/imagesTs
6. Edit the file "nnUNet/nnunetv2/training/nnUNetTrainer/nnUNetTrainer.py" parameter: out_channels = 3 (Because this model is for three classes: 0 for background, 1 and 2 for LLg and RLg, respectively). For example:
			return VSmixTUnet(in_channels=1,
						  out_channels=3,
						  feature_size=48,
						  split_size=[1, 2, 3, 4],
						  window_size=6,
						  num_heads=[3, 6, 12, 24],
						  img_size=[96, 96, 96],
						  depths=[2, 2, 2, 2],
						  patch_size=(2, 2, 2),
						  do_ds=enable_deep_supervision)


7. Run the test command in the main VSmTrans folder on the terminal:
 	nnUNetv2_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d 011 -c 3d_fullres -f 4 --save_probabilities
 	here -i INPUT_FOLDER <absolute address of nnUNet_raw/imagesTs folder>
 	-o OUTPUT_FOLDER <absolute address of output folder, if you want output in "nnUNet_raw/results" then give the address of this folder>
 	-d 011 as id (that I've used in training) 
 	-f fold is 4
 		
8. Now, in the output folder, you will get your output in the following 3 formats: ".nii.gz", ".npz", and ".pkl" for each image. The format with ".nii.gz" is the final predicted image mask in which 0: background, 1: LLg (left lung), 2: RLg (Right lung). Convert the ".nii.gz" file according to your choice.

9. Please cite the papers:
	Tong, Y., Udupa, J. K., McDonough, J. M., Wileyto, E. P., Capraro, A., Wu, C., ... & Campbell, R. M. (2019). Quantitative dynamic thoracic MRI: application to thoracic insufficiency syndrome in pediatric patients. Radiology, 292(1), 206-213.
	
	Tong, Y., Udupa, J. K., McDonough, J. M., Wu, C., Sun, C., Xie, L., ... & Cahill, P. J. (2023). Assessment of regional functional effects of surgical treatment in thoracic insufficiency syndrome via dynamic magnetic resonance imaging. JBJS, 105(1), 53-62.
	
	Liu, T., Bai, Q., Torigian, D. A., Tong, Y., & Udupa, J. K. (2024). VSmTrans: A hybrid paradigm integrating self-attention and convolution for 3D medical image segmentation. Medical image analysis, 98, 103295.

10. For any query, please feel free to contact at the email id: yadavendra.nln@pennmedicine.upenn.edu
