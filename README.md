# PEAL:  Prior-embedded Explicit Attention Learning for Low-overlap Point Cloud Registration [CVPR-2023]
This is the official repo of CVPR 2023 paper :  '' PEAL: Prior-embedded Explicit Attention Learning for Low-overlap Point Cloud Registration ''


<div  align="center">  
<img src="https://github.com/Gardlin/PEAL/blob/main/assets/iter_sample.gif" alt="show" align=center  />
</div>  
import numpy as np

def is_point_in_frustum(P_c, K, z_near, z_far, fov_x, fov_y):
    # 获取相机坐标系中的坐标
    X_c, Y_c, Z_c = P_c

    # 视锥裁剪
    if Z_c < z_near or Z_c > z_far:
        return False
    
    # 计算图像坐标
    u, v = K @ np.array([X_c, Y_c, Z_c])
    u /= Z_c
    v /= Z_c

    # 检查是否在视锥的宽高比范围内
    aspect_ratio_x = np.tan(fov_x / 2) * Z_c
    aspect_ratio_y = np.tan(fov_y / 2) * Z_c
    
    if abs(u) > aspect_ratio_x or abs(v) > aspect_ratio_y:
        return False

    return True

# 内参矩阵
f_x = 1000  # 焦距
f_y = 1000  # 焦距
c_x = 640   # 主点 x 坐标
c_y = 480   # 主点 y 坐标
K = np.array([[f_x, 0, c_x],
              [0, f_y, c_y],
              [0, 0, 1]])

# 外参矩阵（旋转和平移）
R = np.eye(3)  # 假设没有旋转
t = np.array([0, 0, 0])  # 假设没有平移

# 视锥参数
z_near = 0.1
z_far = 1000
fov_x = np.radians(90)  # 水平视场角
fov_y = np.radians(60)  # 垂直视场角

# 假设一个点云
point_cloud = np.array([[1, 2, 10],
                        [3, 4, 20],
                        [5, 6, 50]])

# 将点云转换到相机坐标系
point_cloud_camera = np.dot(R, point_cloud.T).T + t

# 视锥切割
filtered_points = []
for point in point_cloud_camera:
    if is_point_in_frustum(point, K, z_near, z_far, fov_x, fov_y):
        filtered_points.append(point)

filtered_points = np.array(filtered_points)
print(filtered_points)


