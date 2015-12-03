---
layout: post
title: Colorização HSV
description: ""
modified: 2015-10-01
tags: [processamento de imagem digital]
---

Carrega as bibliotecas necessárias e a imagem da Lena.


    %matplotlib inline
    
    from skimage import color
    import urllib, cStringIO
    import numpy as np
    import matplotlib.pyplot as plt
    from PIL import Image as ImagePIL
    
    lenaUrl = 'https://upload.wikimedia.org/wikipedia/en/2/24/Lenna.png'
    f = cStringIO.StringIO(urllib.urlopen(lenaUrl).read())
    
    img = ImagePIL.open(f)
    img = np.array(img.getdata(), np.uint8).reshape(img.size[1], img.size[0], 3)
    img = color.rgb2gray(img)
    
    plt.imshow(img, cmap=plt.cm.gray)
    plt.show()


![png](output_0_0.png)

Cria uma paleta HSV e aplica ela a imagem

    def paleta_hsv(img, h_in, h_out):
        h = np.linspace(h_in, h_out, 256).reshape(256) / 360
        s = np.full(256, 0.8)
        v = np.full(256, 0.8)
        
        pallet = np.dstack((np.dstack((h, s)), v))[0]
        
        imgout = np.zeros((img.shape[0], img.shape[1], 3))
        imgout[::, ::] = pallet[np.uint8(img[::, ::]*255)]
        return imgout
    
    ret = paleta_hsv(img, 20, 100)
    plt.imshow(color.hsv2rgb(ret))


Resultado:
![png](output_1_1.png)
