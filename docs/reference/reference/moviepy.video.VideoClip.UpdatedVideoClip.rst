from moviepy.editor import TextClip, concatenate_videoclips, AudioFileClip

# Параметры видео
WIDTH, HEIGHT = 1280, 720
FONT_SIZE = 40
FONT = "Arial-Bold"
TEXT_COLOR = "black"
BG_COLOR = (255, 255, 255)  # белый фон

# Фоновая музыка (положи mp3 рядом с этим скриптом, или оставь пустым '')
MUSIC_FILE = "background_music.mp3"

# Тексты титров и длительность каждой сцены (сек)
scenes = [
    ("Свадьба Костаревых\nФильм по реальным событиям\n(но местами сценаристы немного увлеклись)", 4),
    ("👰 Екатерина Костарева — НЕВЕСТА\nПлатье — 10/10. Настроение — вечеринка века.", 5),
    ("🤵 Глеб Костарев — ЖЕНИХ\nПомнил кольца. Забыл носки. Главное — любовь.", 5),
    ("🎞 Конец фильма...\nСемья Костаревых — только начинается ❤️", 6),
]

clips = []

for text, duration in scenes:
    txt_clip = TextClip(
        text,
        fontsize=FONT_SIZE,
        font=FONT,
        color=TEXT_COLOR,
        size=(WIDTH - 200, None),
        method='caption',
        align='center'
    ).set_duration(duration).set_position('center').on_color(
        color=BG_COLOR, col_opacity=1
    )
    clips.append(txt_clip)

video = concatenate_videoclips(clips, method="compose")

# Добавляем музыку, если файл существует
import os
if os.path.isfile(MUSIC_FILE):
    audio = AudioFileClip(MUSIC_FILE).volumex(0.1)
    audio = audio.set_duration(video.duration)
    video = video.set_audio(audio)
else:
    print("Музыкальный файл не найден, создаём видео без музыки.")

video.write_videofile("wedding_credits.mp4", fps=24)
