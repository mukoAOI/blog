Torchaudio 处理音频数据的 PyTorch 库，提供了对音频数据的加载、处理、转换等功能，并且与 PyTorch 深度学习框架紧密集成，可以很方便地将音频数据与神经网络模型结合使用。

### 安装 Torchaudio

```cmd
//需要先安装 PyTorch
pip install torch
pip install torchaudio
//当出现Couldn’t find appropriate backend to handle uri and format None 时
pip install pysoundfile
```

### 1. 加载音频数据

Torchaudio 提供了多种加载音频文件的方法，最常用的是 `torchaudio.load`，它可以读取多种格式的音频文件，如 WAV、MP3 等。

```python
import torchaudio

# 加载音频文件
waveform, sample_rate = torchaudio.load("path/to/your/audio/file.wav")
#两个属性
print(f"Waveform: {waveform.shape}")
print(f"Sample rate: {sample_rate}")
```

### 2. 播放音频

可以使用 Python 的 `IPython.display` 模块来播放音频：

```python
from IPython.display import Audio

# 播放音频
Audio(waveform.numpy(), rate=sample_rate)
```

### 3. 音频数据的转换

Torchaudio 提供了丰富的音频转换功能，如变换频率、裁剪、添加噪声等。例如，可以将音频文件的采样率从 44.1kHz 变为 16kHz：

```python
import torchaudio.transforms as T

# 转换采样率
resampler = T.Resample(orig_freq=44100, new_freq=16000)
waveform_resampled = resampler(waveform)

print(f"Resampled waveform: {waveform_resampled.shape}")
```

### 4. 特征提取

在处理音频时，通常会将音频数据转换为特征向量，如梅尔频谱、MFCC 等。Torchaudio 提供了相应的变换方法。

```python
# 提取梅尔频谱
mel_spectrogram = T.MelSpectrogram(sample_rate=sample_rate, n_mels=128)(waveform)

print(f"Mel Spectrogram: {mel_spectrogram.shape}")
```

### 5. 构建音频分类模型

结合 PyTorch，你可以用 Torchaudio 构建一个简单的音频分类模型。例如，下面是一个简单的模型框架：

```python
import torch
import torch.nn as nn
import torch.optim as optim

class AudioClassifier(nn.Module):
    def __init__(self):
        super(AudioClassifier, self).__init__()
        self.conv1 = nn.Conv2d(1, 16, kernel_size=3, stride=1, padding=1)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc1 = nn.Linear(16 * 64 * 64, 10)  # 假设梅尔频谱为 128x128

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = x.view(-1, 16 * 64 * 64)
        x = self.fc1(x)
        return x

# 初始化模型、损失函数和优化器
model = AudioClassifier()
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

# 假设输入为梅尔频谱数据
input_data = torch.randn(1, 1, 128, 128)
output = model(input_data)

print(f"Model output: {output}")
```

### 6. 保存和加载模型

你可以像保存 PyTorch 模型一样保存和加载 Torchaudio 模型：

```python
# 保存模型
torch.save(model.state_dict(), "audio_model.pth")

# 加载模型
model.load_state_dict(torch.load("audio_model.pth"))
```

### 7. 使用预训练模型

Torchaudio 还提供了一些预训练的音频模型，如 Wav2Vec2，可以直接加载并用于任务：

```python
import torchaudio.models as models

# 加载预训练的 Wav2Vec2 模型
wav2vec2 = models.wav2vec2_base(pretrained=True)

# 使用模型进行推理
output = wav2vec2(waveform)
```
