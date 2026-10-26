# 04. 영상 스티칭

인접한 두 카메라의 화면을 하나로 이어 붙이는 실험입니다.

| 파일 | 설명 |
|---|---|
| `stitched.ipynb` | ① `02061/02062.png`: SIFT + BFMatcher → Affine/Homography 정합 ② `02101/02102.png`: `cv2.Stitcher`와 SIFT + FLANN + RANSAC Homography 비교 ③ RTSP 2채널을 매 프레임 스티칭해 `stitched_video.mp4`로 저장 ④ Homography를 한 번만 구해 재사용하고 프레임 시간차를 보정하는 버전 → `test.mp4` ⑤ 딥러닝 Homography 추정(`HomographyNet`) 골격 |
| `msop.ipynb` | OpenCV 특징점 함수를 쓰지 않고 직접 구현: Harris 코너 응답 → 피라미드 → ANMS(비최대 억제) → 방향 정규화 8×8 MSOP 디스크립터 → 원통 투영(`meta.txt`의 초점거리) → NearestNeighbors + Lowe ratio test 매칭 |

## 데이터
- `02061.png`, `02062.png`, `02063.png`, `02101.png`, `02102.png`: 카메라 캡처 이미지
- `test1.png`, `test2.png` + `meta.txt`: MSOP 실습 이미지와 메타데이터(파일명, 크기, 내부 파라미터 K, 회전 R, 초점거리 f)

## 참고
- `stitched.ipynb`의 첫 Affine 셀은 `numpy.float32`를 쓰는데 `numpy`가 `np`로만 import되어 있어 `NameError`가 납니다.
- `HomographyNet`은 학습되지 않은(무작위 초기화) 모델이라 결과에 의미가 없습니다. 구조 확인용 골격입니다.
- RTSP 셀은 `<USER>:<PASSWORD>@<CAMERA_IP>`를 실제 주소로 바꿔야 동작합니다.
