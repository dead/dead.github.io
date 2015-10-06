---
layout: post
title: Colorização
description: ""
modified: 2015-10-01
tags: [processamento de imagem digital]
---

Colorização artificial


    %matplotlib inline
    
    import urllib, cStringIO
    import numpy as np
    import matplotlib.pyplot as plt
    from PIL import Image as ImagePIL
    
    lenaUrl = 'https://upload.wikimedia.org/wikipedia/en/2/24/Lenna.png'
    f = cStringIO.StringIO(urllib.urlopen(lenaUrl).read())
    
    img = ImagePIL.open(f)
    img = np.array(img.getdata(), np.uint8).reshape(img.size[1], img.size[0], 3)
    img = img[::, ::, 2]
    
    plt.imshow(img, cmap=plt.cm.gray)
    plt.show()


![png](/images/posts/colorizacao/output_1_0.png)


Utilizando a imagem da Lena acima, vamos colorir ela utilizando uma peleta que varia de R: 0 G: 0 B: 255 (azul) até R: 255 G: 255 B: 255 (branco). Para isto foi implentada a função paleta que recebe de entrada uma imagem e o intervalo utilizado para a criação da paleta.


    def paleta(img, rgb_in=(0,0,0), rgb_out=(0,0,0)):
        r = np.linspace(rgb_in[0], rgb_out[0], 256, dtype='uint8').reshape(256)
        g = np.linspace(rgb_in[1], rgb_out[1], 256, dtype='uint8').reshape(256)
        b = np.linspace(rgb_in[2], rgb_out[2], 256, dtype='uint8').reshape(256)
        
        pallet = np.dstack((r, g))
        pallet = np.dstack((pallet, b))
        pallet = pallet[0]
        
        # Mostra a paleta gerada
        tmp = np.tile(pallet, (100, 1, 1)).transpose((1, 0, 2))
        plt.imshow(tmp)
        plt.show()
        
        # Aplica a paleta na imagem
        imgout = np.zeros((img.shape[0], img.shape[1], 3), dtype='uint8')
        imgout[::, ::] = pallet[img[::, ::]]
        
        return imgout
    
    print 'Paleta resultante'
    out = paleta(img, (0,0,255), (255,255,255))
    
    print 'Imagem com a paleta aplicada'
    plt.imshow(out)
    plt.show()
    

Paleta resultante

![png](/images/posts/colorizacao/output_3_1.png)

Imagem com a paleta aplicada

![png](/images/posts/colorizacao/output_3_3.png)

Então é isso, até o próximo post!