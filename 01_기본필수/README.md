# 📌 OpenCV 이미지 및 영상 전처리 파이프라인

본 프로젝트는 OpenCV를 활용하여 이미지(`png`)와 동영상(`avi`) 데이터에 대한 일련의 전처리 및 특징 추출(Edge 검출) 작업을 자동화하고, 그 결과를 정량적 요약 지표로 산출하는 파이프라인입니다.

---

## 🚀 주요 기능 및 처리 흐름 (Pipeline)

### 1. 이미지 데이터 전처리 파이프라인
* **대상 파일**: `scene.png`, `noisy.png`, `objects.png`
* **주요 처리 단계**:
  1. **Color Conversion**: 원본 BGR 이미지를 Grayscale(`cv2.COLOR_BGR2GRAY`)로 변환하여 연산량 최적화
  2. **Resizing**: 모델 입력 표준화를 위해 너비 160, 높이 120 크기로 축소 (`cv2.INTER_AREA` 보간법 사용)
  3. **Canny Edge Detection**: 축소된 이미지에 Canny 알고리즘(Low Threshold: 80, High Threshold: 160)을 적용하여 윤곽선(Edge) 추출
* **산출물**: 
  * 색변환 결과 이미지 (`p07_{stem}_gray.png`)
  * 리사이즈 결과 이미지 (`p07_{stem}_small.png`)
  * 엣지 검출 결과 이미지 (`p07_{stem}_edge.png`)
  * 메타데이터 및 엣지 픽셀 수를 담은 요약 CSV (`p07_summary.csv`)

### 2. 영상 데이터 전처리 파이프라인
* **대상 파일**: `sample.avi`
* **주요 처리 단계**:
  1. 원본 비디오 스트림을 프레임 단위로 읽기 (`cv2.VideoCapture`)
  2. 각 프레임마다 이미지 전처리 과정과 동일한 파이프라인 적용 (Grayscale → 160x120 Resize → Canny Edge)
  3. 처리된 엣지 프레임을 다시 3채널(BGR)로 변환하여 새로운 AVI 비디오 파일로 압축 및 저장 (`MJPG` 코덱 적용)
* **산출물**: 
  * 전처리된 엣지 영상 (`p07_video.avi`)

---

## 📂 프로젝트 산출물 구조 (Outputs)

코드가 실행되면 작업 디렉토리 내에 다음과 같은 접두어(`p07_`)가 붙은 결과물들이 생성됩니다. 원본 파일의 덮어쓰기를 방지하도록 설계되었습니다.

```text
📦 01_기본필수/
 ┣ 📜 p07_scene_gray.png
 ┣ 📜 p07_scene_small.png
 ┣ 📜 p07_scene_edge.png
 ┣ 📜 p07_noisy_gray.png
 ┣ 📜 p07_noisy_small.png
 ┣ 📜 p07_noisy_edge.png
 ┣ 📜 p07_objects_gray.png
 ┣ 📜 p07_objects_small.png
 ┣ 📜 p07_objects_edge.png
 ┣ 🎥 p07_video.avi
 ┗ 📊 p07_summary.csv