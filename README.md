# Toonification

## 1. Steps

1. Train Stylegan generator with custom cartoon dataset
   
   ```
   a. python dataset_tool.py create_from_images path/to/custom_cartoon path/to/custom_cartoon_tf
   b. python run_training.py --num-gpus=1 --data-dir=/path/to/cartoon_parent_folder/ --config=config-f --dataset=custom_cartoon_tf --mirror-augment=true --total-kimg=32
   ```
   
2. Blend 2 Stylegan generators, One trained on Cartoon dataset, other pretrained model trained on ffhq dataset.
   Lower resolution layers would be from ffhq, As we want to maintain the basic structure of the face. 
   Toon model on higher resolution layers would add cartoonize the input image by toonifying the low level features.
   
   Blend model takes 3 input, lower resolution model, higher resolution model, And the layer at which to swap between these 2 models.

   ```
   a. Download pretrained stylegan-2 config-f model
   b. cd stylegan2
   c. python blend_models.py stylegan2-ffhq.pkl cartoon_model.pkl 16 --output-pkl="cartoon_blended.pkl"

3. Convert .pkl(The blended model) file to .pth file. Used [Rosinality's](https://github.com/rosinality/stylegan2-pytorch) repository for this
4. Creating dataset to train pixel2style2pixel model.
   I used the blended stylegan generator to create paired dataset.
   
   ```
   a. cd stylegan2
   b. Add training input images to folder raw
   c. python align_images.py raw aligned
   d. python project_images.py --num-steps 500 aligned projected
   e. python create_data.py
   ```
5. Once paired dataset is created(Around 3k pairs are enough), Train pixel2style2pixel
   ```
   a. Set correct data paths in configs/paths_config.py
   b. python scripts/train.py --dataset_type=toonify --exp_dir=path/to/experiment --batch_size=4 --val_interval=2500 --save_interval=5000 --encoder_type=GradualStyleEncoder --start_from_latent_avg --lpips_lambda=0.8 --l2_lambda=1 --id_lambda=1 --w_norm_lambda=0.025 --stylegan_weights pretrained_models/cartoon_blended.pt
   
   
   
6. Test
   ```
   python scripts/inference.py --exp_dir=path/to/experiment --checkpoint_path=toonification/experiment_4/checkpoints/iteration_55000.pt --         data_path=test_data --couple_outputs --test_batch_size=1
   ```
   
Images need to be aligned before training or testing.



## 2. Links

1. Custom cartoon dataset I created
2. Stylegan model trained on Cartoon dataset
3. Blended Stylegan model
4. Blended model converted to Pytorch


## 3. Results

<img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/test_6.png" height="360" width="360"> <img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/out_8.jpeg" height="360" width="360">
<img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/test_5.png" height="360" width="360"> <img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/out_5.jpeg" height="360" width="360">
<img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/test_4.png" height="360" width="360"> <img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/out_4.jpeg" height="360" width="360">
<img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/test_7.jpeg" height="360" width="360"> <img src="https://github.com/Nerdyvedi/Toonification/blob/main/assets/out_7.jpeg" height="360" width="360">


