# Image Inpainting for Regular Holes Using Partial Convolutions
## Abstract
>Image inpainting is the process of restorating a missing, undesired or damaged part of an image. Initially, the
technical faults of early cameras (scratches, stains) were corrected by experts while the development of photos. 
Although nowadays the manipulation of images happens digitally, the proper way of inpainting still requires 
skilled hands. Many effort have been made to solve the automatization of this method. In this paper,we 
represent our work on applying a model based on partial convolutions to fill missing parts of images with 
natural looking content.

## Team

### Király Zoltán [G24TCR]:
* adatok előkészítése (méretezés, maszkolás)
* hálózat elkészítése
* cloud environent beüzemelése
* hálózat tanítása
### Sándor Mátyás [YHMJTY]:
* datasetek felkutatása
* hálózat továbbtanítása checkpoint-ból
* hiperparaméter optimalizáció
### Kékesi Dávid [J9PCWO]:
* tudományterület feltérképezése
* adatok beszerzése
* desktop environment beüzemelése
* hálózat továbbtanítása checkpoint-ból

## Requirements
* Python 3.6
* TensorFlow 1.13
* Keras 2.2.4
* OpenCV and NumPy (maszkoláshoz)
* ImageIO (adatelőkészítéshez)

 Részletes specifikáció a *spec-file.txt* fájlban.
 
## How to use
* VGG16 letöltése és bemásolása a Data mappába 
[VGG16 download](https://drive.google.com/file/d/1HOzmKQFljTdKWftEP-kWD7p2paEaeHM0/view)
* a notebooks/*network_and_training.ipynb* celláinak futtatása a **Train model** alcímig
  #### STAGE1 (initial training)
    ```sh
    #Initial weights set from vgg16 pretrained model
    model = PConvUnet(img_rows=256, img_cols=256, vgg_weights='./data/pytorch_vgg16.h5')
    ```
    Futtassuk a fenti kódot a **VGG16** súlyainak betöltéséhez! 
    Majd a ```model.fit_generator``` kezdetű cellát a tanításhoz 60-70 epoch hosszan.
    
    #### STAGE2 (fine tuning)
    ```sh
    #Load trained model
    model = PConvUnet(img_rows=256, img_cols=256)
    model.load("./data/log/weights/weights.95-0.90.h5",train_bn=True,lr=0.00015)
    ```
    Futtassuk a fenti kódot **ModelCheckpoint** betöltéséhez!
    (a fájlnév helyére értelemszerűen az kerüljön amit be szeretnénk tölteni, és másoljuk a path-ban lévő mappába)
    Majd a ```model.fit_generator``` kezdetű cellát a tanításhoz 50-60 epoch hosszan,
    0.00005 LearningRate mellett.
	
## Last Checkpoint
A teljes training elvégzését követően a hálózatunk súlyai az alábbi linken elérhető fájlban találhatóak:
[weights download](https://drive.google.com/file/d/1c4qXxUBBKn4dqwr7T9zLfotWkv5BJWoL/view)