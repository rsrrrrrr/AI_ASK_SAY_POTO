from kivy.app import App
from kivy.uix.camera import Camera
from kivy.uix.button import Button
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from google.cloud import vision
from google.cloud.vision import types
import os
import io
import random
from gtts import gTTS

class MainApp(App):
    def build(self):
        layout = BoxLayout(orientation='vertical')

        # Create a camera object
        self.camera_object = Camera(play=True)
        layout.add_widget(self.camera_object)

        # Create a button to take picture
        self.capture_button = Button(text="Take Picture")
        self.capture_button.size_hint=(.5, .2)
        self.capture_button.pos_hint={'x': .25, 'y': .75}
        self.capture_button.bind(on_press=self.on_capture)
        layout.add_widget(self.capture_button)

        # Create a label to display recognized text
        self.result_label = Label()
        layout.add_widget(self.result_label)

        return layout

    def on_capture(self, *args):
        # Capture the image from the camera
        self.camera_object.export_to_png('captured_image.png')

        # After capturing the image, process it with Google Cloud Vision API
        client = vision.ImageAnnotatorClient()

        with io.open('captured_image.png', 'rb') as image_file:
            content = image_file.read()

        image = types.Image(content=content)
        response = client.text_detection(image=image)
        texts = response.text_annotations

        result_text = 'Texts:\n'
        for text in texts:
            result_text += '"{}"\n'.format(text.description)
            vertices = (['({},{})'.format(vertex.x, vertex.y)
                        for vertex in text.bounding_poly.vertices])
            result_text += 'bounds: {}\n'.format(','.join(vertices))

        # Update the result label
        self.result_label.text = result_text

        # Save the text to a file
        with open("extracted_text.txt", "w") as f:
            for text in texts:
                f.write("{}\n".format(text.description))

        # Analyze the recognized text and provide information
        self.analyze_and_respond(texts)

    def analyze_and_respond(self, texts):
        # This is a simple example that provides a random answer if the recognized text contains a question mark
        for text in texts:
            if '?' in text.description:
                answers = ["Yes", "No", "Maybe", "I don't know"]
                answer = random.choice(answers)
                self.result_label.text += "\nAnswer: {}".format(answer)
                text_to_speech(answer)
                break

def text_to_speech(text):
    # Convert text to speech
    tts = gTTS(text=text, lang='en')
    tts.save('output.mp3')
    os.system("mpg123 output.mp3")

if __name__ == "__main__":
    MainApp().run()
