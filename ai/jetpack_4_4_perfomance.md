# jetpack 4.4 성능 이슈

링크참조 1: https://forums.developer.nvidia.com/t/face-recognition-running-slow-on-jetpack-4-4/124200/4  
링크참조 2: https://forums.developer.nvidia.com/t/darknet-slower-using-jetpack-4-4-cudnn-8-0-0-cuda-10-2-than-jetpack-4-3-cudnn-7-6-3-cuda-10-0/121579/12
  
## 문제
Nvidia copr.의 Jetson Xavier NX 보드를 테스트 하는 상황입니다. 
기존 Jetson Nano(이하 Nano)와 JetsonTX2(이하 TX2)를 보유하고 있는 상황이고, 이 두 디바이스에서
작동하는 **Object Detector** 프로그램을 올려 NX 디바이스에서 얼마나 성능이 나오는지 테스트하려 했습니다.  


같은 Jetpack 버전이 달라 완벽하게 같은 셋팅을 할 수는 없지만 프로그램이 돌아가는 pip 모듈버전과
네트워크 모델은 같은 것으로 준비하였습니다. 다른점은 아래와 같습니다. [4.2, 4.3버전 정보](https://developer.nvidia.com/jetpack-4_3_DP) 와 
[4.4 버전 정보](https://forums.developer.nvidia.com/t/jetpack-4-4-developer-preview-l4t-r32-4-2-released/120594)

| Jetpack Ver. |TensorRT ver.|CUDA ver. |cuDNN ver.|
|:------------:|:-----------:|:--------:|:--------:|
|Jetpack 4.4   |7.1.0        |10.2      |8.0.0     |
|Jetpack 4.3   |6.0.1        |10.0      |7.6.3     |
|Jetpack 4.2   |5.1.6        |10.0      |7.5.0     |

저의 TX2에는 Jetpack 4.2가 설치되어 있고, NX는 Jetpack을 4.4 부터만 지원합니다.
[Jetpack Archive](https://developer.nvidia.com/embedded/jetpack-archive) 를 참조하면 
각 버전에 맞는 호환 디바이스들을 확인할 수 있습니다. 

문제는 같은 프로그램의 성능이 TX2보다 NX에서 비슷하거나 더 느리다는 것 입니다.  

현재 Object Detection 환경은 3가지를 준비하고 있는데 `TensorRT`, `TF-TRT`, `Tensorflow` 입니다.
성능은 기술한 순서대로 같은 모델 기준 TensorRT > TF-TRT > Tensorflow 입니다. 

이어서 TX2는 TensorRT와 TF-TRT를 이용하여 Inference time 체크를 하였고 NX는 TensorRT 변환에 대해
이슈가 있어서 TF-TRT와 Tensorflow에 대해서만 체크하였습니다. 이슈에 관해서는 추후 포스팅하겠습니다.
Inference 결과를 보면 아래와 같습니다.

>TX2 / **TensorRT**    
>detection inference time: 평균 0.01454 sec  
>FPS: 68~69 fps  

>TX2 / **TF-TRT**    
>detection inference time: 평균 0.07068 sec  
>FPS: 14.1 fps

>NX / **TF-TRT**    
>detection inference time: 평균 0.06887 sec  
>FPS: 14.5 fps

>NX / **Tensorflow**    
>detection inference time: 평균 0.14963 sec  
>FPS: 6.6 fps

 이상과 같습니다. NX는 TX2에 비해 월등한 사양을 가지고 있습니다. [TX2]() 스펙과 [NX]() 스펙을
 비교해볼 수 있습니다. 하지만 결론적으로 위의 Inference 결과를 보았을 때 NX의 결과가 그리 좋지 않음을 알 수 있습니다.
 
 
 ## 참조링크 내용

