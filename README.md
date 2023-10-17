#code
import time
import requests
import sounddevice as sd
import numpy as np

# Replace with your actual Noise Pollution Information Platform API endpoint
platform_url = "https://your-platform-url.com/api/noise-data"

# Function to measure noise level
def measure_noise_level():
    duration = 5  # Duration for recording (in seconds)
    sample_rate = 44100  # Sample rate (samples per second)
    
    # Record audio data
    recording = sd.rec(int(duration * sample_rate), samplerate=sample_rate, channels=1, dtype='int16')
    sd.wait()

    # Calculate the root mean square (RMS) of the audio data
    rms = np.sqrt(np.mean(recording**2))
    
    return rms

# Main loop to send data
while True:
    noise_level = measure_noise_level()
    
    data = {
        "noise_level": noise_level,
        "timestamp": int(time.time()),
        "location": "Sensor Location"  # Replace with actual location data
    }
    
    try:
        response = requests.post(platform_url, json=data)
        
        if response.status_code == 200:
            print("Data sent successfully")
        else:
            print("Failed to send data. Status code:", response.status_code)
    
    except Exception as e:
        print("Error:", str(e))
    
  
