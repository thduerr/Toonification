# Toonification

## 1. Steps

1. Train Stylegan generator with cartoon dataset(Link for Custom dataset on Section 2)
2. Blend 2 Stylegan generators, One trained on Cartoon dataset, other pretrained model trained on ffhq dataset.
   Lower resolution layers would be from ffhq, As we want to maintain the basic structure of the face. 
   Toon model on higher resolution layers would add cartoonize the input image by toonifying the low level featurs.
3. Convert .pkl(The blended model) file to .pth file. Used Rosinality's repository for this
4. Creating dataset to train pixel2style2pixel model.
   I used the blended stylegan generator to create paired dataset.
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


