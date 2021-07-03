# Toonification

## 1. Steps

1. Train Stylegan generator with custom cartoon dataset
   
   ```
   a. python dataset_tool.py create_from_images path/to/custom_cartoon path/to/custom_cartoon_tf
   b. python run_training.py --num-gpus=1 --data-dir=/path/to/cartoon_parent_folder/ --config=config-f --dataset=custom_cartoon_tf --mirror-augment=true --total-kimg=32
   
   

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
   
   
6. Test

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


