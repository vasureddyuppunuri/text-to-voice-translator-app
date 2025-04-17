# text-to-voice-translator-app
# 🌐  SAM The AI Voice Translator App

import streamlit as st
from gtts import gTTS
from deep_translator import GoogleTranslator
import os

st.title("🌐 sam the translator")


text = st.text_input("🗣️ Enter text to translate and speak:")


languages = GoogleTranslator().get_supported_languages(as_dict=True)


target_lang_name = st.selectbox("🎯 Choose output language:", list(languages.keys()))
target_lang_code = languages[target_lang_name]


if st.button("Translate and Speak"):
    if text.strip() == "":
        st.warning("Please enter some text.")
    else:
        try:
            # Translate text using Google Translate
            translated_text = GoogleTranslator(target=target_lang_code).translate(text)
            st.success(f"📝 Translated Text: {translated_text}")
            tts = gTTS(text=translated_text, lang=target_lang_code)
            mp3_path = "translated_audio.mp3"
            tts.save(mp3_path)
            audio_bytes = open(mp3_path, "rb").read()
            st.audio(audio_bytes, format="audio/mp3")
        except Exception as e:
            st.error(f"⚠️ Error: {e}")


## Run locally

