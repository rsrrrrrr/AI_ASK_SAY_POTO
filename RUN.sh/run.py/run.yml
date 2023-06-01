import base64
from gtts import gTTS
import speech_recognition as sr
import os

# Convert color to 16-bit binary
def color_to_binary(r, g, b):
    # Check if the provided values are within the 16-bit range
    if not all(0 <= val <= 65535 for val in (r, g, b)):
        raise ValueError("All color values must be in the range 0-65535")

    # Convert each color channel to 16-bit binary
    r_bin = format(r, '016b')
    g_bin = format(g, '016b')
    b_bin = format(b, '016b')

    # Combine the binary values
    color_bin = r_bin + g_bin + b_bin

    return color_bin

# Convert binary to Base64 encoded ASCII text
def binary_to_text(binary):
    bytes_repr = int(binary, 2).to_bytes(len(binary) // 8, byteorder='big')
    encoded = base64.b64encode(bytes_repr).decode('ascii')
    return encoded

# Save the text to a file
def save_to_file(text, filename="color.txt"):
    with open(filename, "w") as f:
        f.write(text)

# Convert text to speech
def text_to_speech(text, lang='en'):
    tts = gTTS(text=text, lang=lang)
    tts.save('output.mp3')

# Recognize speech from mic
def recognize_speech_from_mic(recognizer, microphone):
    with microphone as source:
        print("Listening...")
        audio = recognizer.listen(source)

    try:
        print("Recognizing...")
        return recognizer.recognize_google(audio)
    except sr.UnknownValueError:
        print("Google Speech Recognition could not understand audio")
    except sr.RequestError as e:
        print("Could not request results from Google Speech Recognition service; {0}".format(e))

# Synthesize text to speech
def synthesize_text_to_speech(text, lang='en', slow=False):
    speech = gTTS(text=text, lang=lang, slow=slow)
    speech.save("output.mp3")
    os.system("mpg123 output.mp3")

# Main code
def main():
    recognizer = sr.Recognizer()
    mic = sr.Microphone()
    text = recognize_speech_from_mic(recognizer, mic)
    synthesize_text_to_speech(text)

    # Convert color to binary
    binary_color = color_to_binary(65535, 32767, 16383)

    # Convert binary to text
    text_color = binary_to_text(binary_color)
    print(text_color)

    # Save text to file
    save_to_file(text_color)

if __name__ == "__main__":
    main()
