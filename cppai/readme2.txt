root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# ./export ../retinanet-9.onnx chipeak.engine
Building engine...
Building FP16 core model...
Parameter check failed at: optimizationProfile.cpp::setDimensions::119, condition: std::all_of(dims.d, dims.d + dims.nbDims, [](int x) { return x > 0; })
Parameter check failed at: optimizationProfile.cpp::setDimensions::119, condition: std::all_of(dims.d, dims.d + dims.nbDims, [](int x) { return x > 0; })
Parameter check failed at: optimizationProfile.cpp::setDimensions::119, condition: std::all_of(dims.d, dims.d + dims.nbDims, [](int x) { return x > 0; })
chipeak: not int8 ...
Building accelerated plugins...
Parameter check failed at: ../builder/Network.cpp::addConcatenation::308, condition: nbInputs > 0 && nbInputs < MAX_CONCAT_INPUTS
Segmentation fault (core dumped)
root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# ./infer
Usage: ./infer engine.plan image.jpg [<OUTPUT>.png]


root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# ./infer ../dog_bike_car.jpg ../dog_bike_car.png
Loading engine...
../rtSafe/coreReadArchive.cpp (31) - Serialization Error in verifyHeader: 0 (Magic tag does not match)
INVALID_STATE: std::exception
INVALID_CONFIG: Deserialize the cuda engine failed.
Segmentation fault (core dumped)


root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# ./infer
Usage: ./infer engine.plan image.jpg [<OUTPUT>.png]

版本不符合：
root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# ./infer ../retinanet-9.engine  ./dog_bike_car.jpg ../dog_bike_car.png
Loading engine...
../rtSafe/coreReadArchive.cpp (38) - Serialization Error in verifyHeader: 0 (Version tag does not match)
INVALID_STATE: std::exception
INVALID_CONFIG: Deserialize the cuda engine failed.
Segmentation fault (core dumped)



TX2 JetPack4.3 + deepstream4:

root@nvidia:/usr/src/tensorrt/bin# ./trtexec --onnx=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.onnx --saveEngine=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.engine
&&&& RUNNING TensorRT.trtexec # ./trtexec --onnx=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.onnx --saveEngine=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.engine
[09/21/2020-15:56:00] [I] === Model Options ===
[09/21/2020-15:56:00] [I] Format: ONNX
[09/21/2020-15:56:00] [I] Model: /media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.onnx
[09/21/2020-15:56:00] [I] Output:
[09/21/2020-15:56:00] [I] === Build Options ===
[09/21/2020-15:56:00] [I] Max batch: 1
[09/21/2020-15:56:00] [I] Workspace: 16 MB
[09/21/2020-15:56:00] [I] minTiming: 1
[09/21/2020-15:56:00] [I] avgTiming: 8
[09/21/2020-15:56:00] [I] Precision: FP32
[09/21/2020-15:56:00] [I] Calibration: 
[09/21/2020-15:56:00] [I] Safe mode: Disabled
[09/21/2020-15:56:00] [I] Save engine: /media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.engine
[09/21/2020-15:56:00] [I] Load engine: 
[09/21/2020-15:56:00] [I] Inputs format: fp32:CHW
[09/21/2020-15:56:00] [I] Outputs format: fp32:CHW
[09/21/2020-15:56:00] [I] Input build shapes: model
[09/21/2020-15:56:00] [I] === System Options ===
[09/21/2020-15:56:00] [I] Device: 0
[09/21/2020-15:56:00] [I] DLACore: 
[09/21/2020-15:56:00] [I] Plugins:
[09/21/2020-15:56:00] [I] === Inference Options ===
[09/21/2020-15:56:00] [I] Batch: 1
[09/21/2020-15:56:00] [I] Iterations: 10 (200 ms warm up)
[09/21/2020-15:56:00] [I] Duration: 10s
[09/21/2020-15:56:00] [I] Sleep time: 0ms
[09/21/2020-15:56:00] [I] Streams: 1
[09/21/2020-15:56:00] [I] Spin-wait: Disabled
[09/21/2020-15:56:00] [I] Multithreading: Enabled
[09/21/2020-15:56:00] [I] CUDA Graph: Disabled
[09/21/2020-15:56:00] [I] Skip inference: Disabled
[09/21/2020-15:56:00] [I] Input inference shapes: model
[09/21/2020-15:56:00] [I] === Reporting Options ===
[09/21/2020-15:56:00] [I] Verbose: Disabled
[09/21/2020-15:56:00] [I] Averages: 10 inferences
[09/21/2020-15:56:00] [I] Percentile: 99
[09/21/2020-15:56:00] [I] Dump output: Disabled
[09/21/2020-15:56:00] [I] Profile: Disabled
[09/21/2020-15:56:00] [I] Export timing to JSON file: 
[09/21/2020-15:56:00] [I] Export profile to JSON file: 
[09/21/2020-15:56:00] [I] 
----------------------------------------------------------------
Input filename:   /media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.onnx
ONNX IR version:  0.0.6
Opset version:    9
Producer name:    pytorch
Producer version: 1.6
Domain:           
Model version:    0
Doc string:       
----------------------------------------------------------------
WARNING: ONNX model has a newer ir_version (0.0.6) than this parser was built against (0.0.3).
While parsing node number 0 [Conv]:
ERROR: ModelImporter.cpp:296 In function importModel:
[5] Assertion failed: tensors.count(input_name)
[09/21/2020-15:56:02] [E] Failed to parse onnx file
[09/21/2020-15:56:02] [E] Parsing model failed
[09/21/2020-15:56:02] [E] Engine could not be created
&&&& FAILED TensorRT.trtexec # ./trtexec --onnx=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.onnx --saveEngine=/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/retinanet-9.engine


root@nvidia:/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/test/build# cmake --version
cmake version 3.16.2

CMake suite maintained and supported by Kitware (kitware.com/cmake).




