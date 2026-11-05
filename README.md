# YOLO ONNX C++ Defect Inspection

산업용 표면 결함 데이터로 YOLO 모델을 학습하고, ONNX로 변환해 **C++(ONNX Runtime + OpenCV)로 추론**하는 프로젝트입니다.
Python 학습 결과와 C++ 추론 결과가 같은지 확인하고, 처리 시간을 측정합니다.

## 목표
- Python에서 학습한 모델을 C++ 검사 프로그램에 붙이는 과정을 직접 구현
- 전처리, 추론, 후처리(박스 디코딩, NMS)를 C++로 작성
- Python 대비 결과 일치 여부와 이미지당 처리 시간 비교

## 파이프라인
```
[Python] 데이터셋 변환 → YOLO 학습 → ONNX 변환
[C++]    ONNX 로드 → 전처리 → 추론 → 박스 디코딩·NMS → 결과 저장·시각화
```

## 데이터셋
- NEU-DET (강판 표면 결함 6종: crazing, inclusion, patches, pitted_surface, rolled-in_scale, scratches)
- 학습/검증/테스트 분할: 측정 후 기재

## 폴더 구조
```
├── python/
│   ├── convert_dataset.py   # VOC XML → YOLO txt
│   ├── train.py             # YOLO 학습
│   └── export_onnx.py       # ONNX 변환, Python 기준 결과 저장
├── cpp/
│   ├── CMakeLists.txt
│   ├── include/
│   └── src/
│       └── main.cpp         # C++ 추론
├── models/                  # .onnx (용량 크면 Release에 첨부)
├── samples/                 # 테스트 이미지 몇 장
└── results/                 # 결과 이미지, 비교표
```

## 실행 방법
### 1. 학습 (Python)
```bash
pip install ultralytics
python python/train.py
python python/export_onnx.py
```

### 2. 추론 (C++)
```bash
cd cpp && mkdir build && cd build
cmake .. -DONNXRUNTIME_DIR=/path/to/onnxruntime
cmake --build . --config Release
./yolo_infer ../../models/best.onnx ../../samples/
```

## 결과
| 항목 | Python (Ultralytics) | C++ (ONNX Runtime CPU) |
|---|---|---|
| mAP@0.5 (test) | | |
| 박스 일치율 | 기준 | |
| 이미지당 처리 시간 (ms) | | |

## 환경
- 학습: Linux GPU 서버
- 추론: (OS, CPU) 기재
- ONNX Runtime 버전, OpenCV 버전 기재

## 배운 점
- (진행하며 작성)
