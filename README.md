# TextRecognizationML
The "Text Reorganization System" uses machine learning and Raspberry Pi to recognize and reorganize text from images. It leverages Tesseract OCR for text extraction and Python libraries for processing, providing a user-friendly interface for real-time text management.
import smbus2 as smbus
from RPLCD.i2c import CharLCD
from PIL import Image
import pytesseract
import time

i2c_address = 0 * 27
lcd_columns = 16
lcd_rows = 2

lcd = CharLCD(i2c_expander='PCF8574', address=i2c_address, port=1, cols=lcd_columns, rows=lcd_rows, charmap='A00')


def recoginze_text_from_image(image_path):
    try:
        img = Image.open(image_path)
        text = pytesseract.image_to_string(img)
        return text.strip()
    except Exception as e:
        print(f"Error recognizing text from image: {e}")
        return ""


def display_on_lcd(text):
    try:
        lcd.clear()
        time.sleep(0.1)

        if len(text) <= lcd_columns:
            lcd.write_string(text)
        else:
            lcd.write_string(text[:lcd_columns])
            lcd.cursor_pos = (1, 0)
            lcd.write_string(text[lcd_columns:lcd_columns * 2])

        print("Text displayed on LCD successfully")
    except Exception as e:
        print(f"Error displaying text on LCD: {e}")


if __name__ == "__main__":
    image_path = 'D:\\final year project\\screenshot\\indexpage.jpg'

    recognized_text = recognize_text_from_image(image_path)

    print("Recognized_text:")
    print(recognized_text)

    display_on_lcd(recognized_text)
