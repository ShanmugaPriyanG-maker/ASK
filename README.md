# ASK
## Aim:
Write a simple Python program for the modulation and demodulation of ASK and FSK.
## Tools required:
Google Colab
## Program:
### ASK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, fc, br, T = 5000, 100, 10, 1
t = np.linspace(0, T, fs, endpoint=False)

bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, fs // br)

car = np.sin(2 * np.pi * fc * t)
ask = msg * car

b, a = butter(5, fc / (0.5 * fs), btype='low')
demod = lfilter(b, a, ask * car)

dec = (demod[::fs // br] > 0.25).astype(int)

signals = [msg, car, ask]
titles = ['Message', 'Carrier', 'ASK Signal']
colors = ['red', 'yellow', 'green']

plt.figure(figsize=(10, 8))

for i in range(3):
    plt.subplot(4, 1, i + 1)
    plt.plot(t, signals[i], color=colors[i])
    plt.title(titles[i])
    plt.grid()

plt.subplot(4, 1, 4)
plt.step(range(len(dec)), dec, where='mid', color='red')
plt.title("Decoded Bits")
plt.grid()

plt.tight_layout()
plt.show()
```
### Output Waveform:
<img width="989" height="790" alt="dc4-1" src="https://github.com/user-attachments/assets/8cb7065f-7889-4cf1-ac3e-cb9f9a5b7635" />

### FSK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs,f1,f2,br,T=1500,50,90,10,1
t=np.linspace(0,T,fs,endpoint=False)

bits=np.random.randint(0,2,br)
bd=fs//br
msg=np.repeat(bits,bd)

c1=np.sin(2*np.pi*f1*t)
c2=np.sin(2*np.pi*f2*t)

fsk=np.concatenate([np.sin(2*np.pi*(f2 if b else f1)*t[i*bd:(i+1)*bd]) for i,b in enumerate(bits)])

b,a=butter(5,f2/(0.5*fs),'low')
x1=lfilter(b,a,fsk*c1)
x2=lfilter(b,a,fsk*c2)

dec=np.array([
    1 if np.sum(x2[i*bd:(i+1)*bd]**2) >
         np.sum(x1[i*bd:(i+1)*bd]**2) else 0
    for i in range(br)
])

demod=np.repeat(dec,bd)

signals=[msg,c1,c2,fsk,demod]
titles=['Message Signal','Carrier f1','Carrier f2',
        'FSK Signal','Demodulated Signal']

colors=['blue','green','red','magenta','blue']

plt.figure(figsize=(12,12))

for i in range(5):
    plt.subplot(5,1,i+1)
    plt.plot(t,signals[i],color=colors[i])
    plt.title(titles[i])
    plt.grid()

plt.tight_layout()
plt.show()
```
### output:
<img width="1189" height="1190" alt="dc4-2" src="https://github.com/user-attachments/assets/04858421-a7f4-4d56-89ca-f5f3dfb87c55" />


## Result:
Thus, the ASK and FSK performed using python in VS Code

