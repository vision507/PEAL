# PEAL:  Prior-embedded Explicit Attention Learning for Low-overlap Point Cloud Registration [CVPR-2023]
This is the official repo of CVPR 2023 paper :  '' PEAL: Prior-embedded Explicit Attention Learning for Low-overlap Point Cloud Registration ''


<div  align="center">  
<img src="https://github.com/Gardlin/PEAL/blob/main/assets/iter_sample.gif" alt="show" align=center  />
</div>  
# 创建渲染图像
image = torch.zeros((S[0], S[1], 3))  # 初始化图像（高度 x 宽度 x 3）
for i in range(S[0]):
    for j in range(S[1]):
        # 如果该像素被点覆盖，且该点的深度小于当前深度缓冲区中的值
        if idx[i, j] != -1:
            point_idx = idx[i, j]
            image[i, j] = features[point_idx]  # 使用点的颜色特征填充图像

# 使用 Matplotlib 展示渲染的图像
plt.imshow(image.numpy())
plt.axis('off')
plt.show()

