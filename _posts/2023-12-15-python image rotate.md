---
layout: post
title: python image rotate 이미지 회전 좌표회전
categories: python image rotate 이미지 회전 좌표
description: python image rotate 이미지 회전 좌표
keywords: python image rotate 이미지 회전 cv2 cv2.ROTATE point
---

# 이미지 회전
> 이미지를 90,180,270 회전은 cv2를 이용하면 된다.

~~~
# 90도
cv2.ROTATE_CLOCKWISE
# 270도
cv2.ROTATE_COUNTERCLOCKWISE
# 180도
cv2.ROTATE_180
~~~

# 좌표 회전

> 어떤 좌표를 회전 시키는 것은 다음과 같다

~~~
import math

- 본 좌표 x, y

# 본래 좌표에서 본래 이미지의 센터 값 빼주기
x = x - center_origin_x
y = y - center_origin_y

# 각도 
if units == "DEGREES":
    angle = math.radians(angle)


# 좌표회전하고 회전한 이미지 중앙에 좌표 배치
xr = (x * math.cos(angle)) - (y * math.sin(angle)) + center_rotated_x
yr = (x * math.sin(angle)) + (y * math.cos(angle)) + center_rotated_y

~~~

# 그 외 방법들

> 아래와 같은 방법도 있지만 검정 배경이 생긴다.
~~~
# 방법1
 # grab the dimensions of the image and calculate the center of the image
 (h, w) = image.shape[:2]
 (cX, cY) = (w // 2, h // 2)
 #rotate our image by 45 degrees around the center of the image
 M = cv2.getRotationMatrix2D((cX, cY), 45, 1.0)
 rotated = cv2.warpAffine(image, M, (w, h))
~~~

~~~
# 방법2
rotated = imutils.rotate_bound(image, -33)
~~~