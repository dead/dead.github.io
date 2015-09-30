---
layout: post
title: Amostragem e Quantização
description: ""
modified: 2015-09-30
tags: [processamento de imagem digital]
---

O mundo real é contínuo, ou seja, entre dois pontos quaisquer existem infinitos pontos.
No computador não é possível representar esses infintos pontos, para isso é feita uma amostragem.
Fazer a amostragem quer dizer que serão selecionados um número finito de pontos.


    %matplotlib inline
    
    import urllib, cStringIO
    import numpy as np
    import matplotlib.pyplot as plt
    from PIL import Image as ImagePIL
    
    lenaUrl = 'https://upload.wikimedia.org/wikipedia/en/2/24/Lenna.png'
    f = cStringIO.StringIO(urllib.urlopen(lenaUrl).read())
    
    img = ImagePIL.open(f)
    img = np.array(img.getdata(), np.uint8).reshape(img.size[1], img.size[0], 3)
    print img.shape
    
    plt.imshow(img)

    (512L, 512L, 3L)
    




    <matplotlib.image.AxesImage at 0xa37dac8>




![png](images/posts/amostragem/output_1_2.png)


Considerando uma imagem da Lena de resolução 512 pixels de altura e largura, ao aplicar uma amostragem que seleciona metade dos pixels em relação a altura e relação ao largura. Depois de aplicar a amostragem, é obtida uma imagem com altura e largura de 256 pixels.


    def amostragem(img, n = 2):
        return img[::n, ::n] #corta o array img pelo passo n, tanto na altura quanto pra largura.
    
    plt.imshow(amostragem(img, 2))




    <matplotlib.image.AxesImage at 0xb0c6f60>




![png](images/posts/amostragem/output_3_1.png)


Para fazer a quantização da imagem


    def quantizacao(img, n = 2):
        m = np.amax(img)+1
        a = np.uint8(img/(m/float(n))) #
        b = np.uint8((a/(n-1.))*255) #transforma de volta pra 0-255 (para exibir a imagem)
        return b
    
    plt.imshow(quantizacao(img, 5))




    <matplotlib.image.AxesImage at 0xa54b828>




![png](images/posts/amostragem/output_5_1.png)



    
