---
layout: post
title: DeepPose, Human Pose Estimation via Deep Neural Networks
comments: true
categories : [paper]
published: true

---

Human Pose 에 관해서 딥러닝을 최초로 적용한 논문이라고 한다.

https://arxiv.org/abs/1312.4659

원글은 위와 같다.

![????](https://ifh.cc/g/sWZw9Q.png)

말그대로 딥러닝을 최초로 적용한 논문이고, 이게 기존에 사용하던 방법이다.

![enter image description here](https://ifh.cc/g/dc02HT.jpg)

x, y 좌표 값을 얻어오고, detected 된 물체의 bounding 된 박스를 그려서 bounding된 박스의 높이와 너비로 나누어서 정규화를 해준다, 그러면 0~1사이의값이 나오게 되고, 이를 통해 연산을 한다는 것.

이 과정을 통해서 skeleton을 추출하게 되면 부정확한 부분들이 많다고 한다.

![enter image description here](https://ifh.cc/g/KK6Vg1.png)

이에, 저 과정을 여러번, 사용자가 지정한 만큼(s) 반복하는 것이 이 논문에서 제시하는 방법이다.

![enter image description here](https://ifh.cc/g/DVfglp.jpg)

연산과정은 위와 같다.

![enter image description here](https://ifh.cc/g/oMko4s.png)

이런 결과를 얻었다고 논문에 적혀있다.

초록색이 원래 사람의 자세이며, 빨간색이 모델이 예측한 사람의 자세이다.
