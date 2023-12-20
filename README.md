import tkinter as tk
from PIL import Image, ImageTk
import random
import imageio
import os
import pygame
import threading
import pyttsx3
import speech_recognition as sr
import time


class QuizGame():
    pygame.init()
    pygame.mixer.music.load("music_zapsplat_milky_way.mp3")
    pygame.mixer.music.set_volume(0.3)
    if not pygame.mixer.music.get_busy():
        pygame.mixer.music.play(-1)

    def on_enter(self, event):
        self.submit_button.config(bg='green', fg='white')

    def on_leave(self, event):
        self.submit_button.config(bg='black', fg='white')

    def play_sound(self):
        pygame.mixer.init()
        sound = pygame.mixer.Sound("zapsplat_multimedia_button_click_bright_003_92100.mp3")
        channel1 = pygame.mixer.Channel(1)
        channel1.play(sound)

    def load_frames(self):
        video_path = "WhatsApp Video 2023-12-15 at 09.04.36 (1).mp4"
        if os.path.exists(video_path):
            vid = imageio.get_reader(video_path)
            for num, im in enumerate(vid):
                if num % 5 == 0:
                    im = Image.fromarray(im)
                    self.frame_images.append(ImageTk.PhotoImage(im))
        else:
            print("Video file not found!")

    def display_frame(self):
        if self.frame_images:
            self.background_label.config(image=self.frame_images[self.current_frame])
            self.current_frame = (self.current_frame + 1) % len(self.frame_images)
            self.root.after(50, self.display_frame)

    def t2s(self, text):
        pyttsx3.speak(text)
        # self.s2t()
        # self.submit_button.invoke()

    def s2t(self):
        time.sleep(3)
        recording = sr.Recognizer()

        with sr.Microphone() as source:
            recording.adjust_for_ambient_noise(source, duration=3)
            pyttsx3.speak("Aswer the question to get your voice recognized")
            while True:
                audio = recording.record(source, duration=3)
                try:
                    sgtext = recording.recognize_google(audio)
                    break
                except sr.UnknownValueError:
                    pyttsx3.speak("Your Voice is recognized. Please Answer the question again")
            # sgtext = recording.recognize_vosk(audio)
            print(sgtext)
            # print(sgtext)
            if "a" in sgtext.lower():
                self.selected_option.set(self.answers[self.index][0])
            elif "b" in sgtext.lower():
                self.selected_option.set(self.answers[self.index][1])
            elif "c" in sgtext.lower():
                self.selected_option.set(self.answers[self.index][2])
            elif "d" in sgtext.lower():
                self.selected_option.set(self.answers[self.index][3])
        time.sleep(3)
        self.submit_button.invoke()

    def __init__(self, root):
        self.root = root
        self.root.title("Quiz Game")
        self.root.geometry("1200x720")

        self.background_label = tk.Label(root)
        self.background_label.place(x=0, y=0, relwidth=1, relheight=1)

        self.frame_images = []
        self.load_frames()

        self.current_frame = 0
        self.display_frame()

        self.questions = ["What is the name of the planet which is composed mostly of water?",
                          "Which celestial body is known as Earth's natural satellite?",
                          "What is the closest star to Earth?",
                          "What is the force that keeps planets in orbit around the sun?",
                          "Which planet is known as the Red Planet?",
                          "What is the largest planet in our solar system?",
                          "What is the name of the galaxy that contains our solar system?",
                          "Who was the first human to set foot on the moon?",
                          "What is the name of the first artificial satellite launched into space?",
                          "What does NASA stand for?",
                          "What is the speed of light in a vacuum?",
                          "What is the main function of a telescope?",
                          "What is the International Space Station (ISS)?",
                          "What is the name of the rover that NASA sent to Mars in 2021?",
                          "What is the basic unit of electric current?",
                          "What is the main component of Earth's atmosphere?",
                          "What is the process by which plants make their own food using sunlight?",
                          "What is the largest ocean on Earth?",
                          "What is the name of the first human-made object to reach interstellar space?",
                          "What is the term for a sudden burst of energy on the Sun's surface?",
                          "What is the name of the first artificial satellite launched by the Soviet Union?",
                          "Which planet is known as the Morning Star or Evening Star due to its brightness?",
                          "What is the term for the path an object takes around a star?",
                          "Who developed the theory of general relativity?",
                          "What is the name of the most famous space telescope launched by NASA in 1990?",
                          "What is the primary mission of the Kepler Space Telescope?",
                          "Which spacecraft, launched by NASA in 1977, is the farthest human-made object from Earth?",
                          "What is the primary purpose of the Large Hadron Collider (LHC) located at CERN?",
                          "Which mission successfully landed a probe on Saturn's moon Titan in 2005?",
                          "What is the concept behind the Alcubierre warp drive discussed in theoretical physics?",
                          "Which phenomenon occurs when a massive star collapses under its gravity, resulting in a super-dense object?",
                          "What is the primary objective of the Artemis program by NASA?",
                          "Which space agency successfully launched the Chang'e-4 mission, the first to land on the far side of the Moon?",
                          "What is the primary goal of the Breakthrough Starshot initiative?",
                          "Which space telescope is designed to observe the universe in infrared wavelengths and is set to succeed the Hubble Space Telescope?",
                          "What is the name of the first artificial satellite launched by the United States?",
                          "Which planet is often referred to as the Evening Star when visible after sunset?",
                          "What is the term for the point in an orbit where a satellite is closest to Earth?",
                          "Who was the first woman in space?",
                          "What is the name of the mission that successfully landed the Curiosity rover on Mars in 2012?",
                          "What does the acronym ISS stand for in the context of space technology?",
                          "What is the force that pulls objects toward the center of the Earth?",
                          "Which spacecraft was the first to carry humans to the Moon's surface?",
                          "What is the name of the giant storm on Jupiter that has been raging for centuries?",
                          "Which element is most abundant in the Sun?",
                          "What is the term for the imaginary line that runs from the North Pole to the South Pole through the Earth's center?",
                          "What is the name of the process by which a star releases energy, including light and heat?",
                          "Which planet is known as the Blue Planet due to its abundant water?",
                          "What is the name of the largest moon of Saturn?",
                          "Which space agency successfully landed the Perseverance rover on Mars in 2021?",
                          "What is the primary aim of Chandrayaan-3?",
                          "Which space agency is responsible for Chandrayaan-3?",
                          "Which telescope has been instrumental in discovering numerous exoplanets?",
                          "Which component of Chandrayaan-2 experienced a hard landing on the Moon?",
                          "What improvements are expected in Chandrayaan-3 compared to its predecessor missions?",
                          "Which part of the Moon is of particular interest for potential Chandrayaan-3 exploration?",
                          "What's the significance of exploring the lunar poles?",
                          "Which type of lander/rover system is anticipated to be part of Chandrayaan-3?",
                          "What type of scientific instruments might be deployed on Chandrayaan-3 to explore the lunar surface?",
                          "What's the primary advantage of exploring the lunar poles for future human missions?",
                          "What is an exoplanet?",
                          "Which method is commonly used to detect exoplanets?",
                          "What is the habitable zone of an exoplanet?",
                          "Which type of star is most likely to have habitable exoplanets?",
                          "What is the primary indicator of potential habitability on an exoplanet?",
                          "Which telescope has been instrumental in discovering numerous exoplanets?",
                          "What is an 'exomoon'?",
                          "Which of these gases is often considered a potential sign of life on an exoplanet?",
                          "What is the transit method in exoplanet detection?",
                          "What are extremophiles in the context of exoplanet research?",
                          "What is the primary source of power for solar-powered small playing cars?",
                          "Which component in a robotic system is responsible for processing information and making decisions?",
                          "What is the purpose of sensors in robotics?",
                          "Which programming language is commonly used for programming small robotic devices?",
                          "What does the term 'AI' stand for in the context of robotics?",
                          "In a solar-powered small playing car, what converts solar energy into electrical energy?",
                          "Which of the following is a benefit of using solar power in small playing cars?",
                          "What is the role of actuators in robotics?",
                          "What is the primary advantage of using small playing cars with robotic capabilities?",
                          "Which of the following is a common application of robotics in everyday life?",
                          "What is the name of the most recent Mars rover that landed on the red planet in 2023?",
                          "What organization is responsible for the Mars rover missions, including the 2023 Perseverance rover?",
                          "Which country's space agency collaborated with NASA on the ExoMars program, aiming to search for signs of past or present life on Mars?",
                          "What is the purpose of the SuperCam instrument on the Perseverance rover?",
                          "What innovative technology did the Perseverance rover deploy for the first time to attempt to produce oxygen from the Martian atmosphere?",
                          "Which prominent feature of Mars did the Perseverance rover land near in 2023, known for its scientific interest and potential signs of ancient life?",
                          "What is the main objective of the Ingenuity helicopter, which accompanied the Perseverance rover to Mars?",
                          "What is the approximate duration of a Martian day, also known as a 'sol'?",
                          "What role does the Sample Caching System on the Perseverance rover play in future Mars exploration missions?",
                          "In addition to Perseverance, which other NASA Mars rover was designed to operate for a nominal mission duration of 90 Martian sols (days)?",
                          "Which moon of Jupiter is considered one of the best candidates for finding life beyond Earth due to its subsurface ocean of liquid water?",
                          "What is the name of the dwarf planet, located beyond Neptune, that was visited by NASA's New Horizons spacecraft in 2015?",
                          "Which space agency, in collaboration with the Russian space agency Roscosmos, operates the International Space Station (ISS)?",
                          "What is the term for a star that suddenly increases in brightness, often reaching luminosities thousands of times greater than its normal state?",
                          "Which space probe, launched by NASA, became the first spacecraft to fly through the outer atmosphere of the Sun, known as the solar corona?",
                          "What is the name of the phenomenon where two neutron stars spiral into each other, producing gravitational waves and often leading to the formation of a black hole?",
                          "Which space mission, launched by NASA, is designed to study the atmosphere of Jupiter and determine its composition, temperature, and cloud structure?",
                          "What is the name of the phenomenon where a star collapses under its gravity, forming a dense core that emits beams of radiation and is observed as a rapidly rotating celestial object?",
                          "Which space agency, in collaboration with ESA, launched the Rosetta spacecraft, which successfully landed the Philae probe on the comet 67P/Churyumov-Gerasimenko?",
                          "What is the term for the point in an orbit where a celestial body is farthest from the Sun?"]

        self.correct_answers = ['B) Earth', 'B) Moon', 'A) Proxima Centauri',
                                'B) Gravity', 'B) Mars', 'B) Jupiter', 'B) Milky Way', 'A) Neil Armstrong',
                                'B) Sputnik 1',
                                'A) National Aeronautics and Space Administration', 'A) 186,282 miles per second',
                                'D) Observe distant objects',
                                'D) A habitable artificial satellite', 'D) Perseverence', 'B) Ampere', 'A) Nitrogen',
                                'B) Photosynthesis',
                                'B) Pacific Ocean',
                                'B) Voyager 1', 'A) Solar flare', 'A) Sputnik 1', 'B) Venus', 'A) Orbit',
                                'B) Albert Einstein', 'B) Hubble Space Telescope',
                                'A) Study exoplanets', 'A) Voyager 1', 'D) Explore the Higgs boson',
                                'B) Cassini-Huygens', 'A) Faster-than-light travel', 'B) Black hole',
                                'B) Return humans to the Moon and go on to Mars',
                                'D) CNSA (China National Space Administration)',
                                'A) Send small spacecraft to another star system', 'A) James Webb Space Telescope',
                                'A) Explorer 1',
                                'B) Venus', 'B) Perigee', 'A) Valentina Tereshkova', 'B) Mars Science Laboratory',
                                'D) International Space Station', 'A) Gravitational force', 'B) Apollo 11',
                                'A) Great Red Spot',
                                'B) Hydrogen', 'B) Meridian', 'A) Fusion', 'A) Earth', 'A) Titan', 'C) NASA',

                                'B) Explore the lunar poles for water ice',
                                'D) ISRO (Indian Space Research Organisation)', 'B) Chandrayaan-2',
                                'A) Vikram lander', 'A) Enhanced rover capabilities', 'C) Lunar poles',
                                'C) Potential presence of water ice', 'A) Pragyan', 'C) Seismometer',
                                'A) Shelter from solar radiation',
                                'A) A planet located outside our solar system', 'C) Spectroscopy',
                                'B) The area around a star where conditions may support liquid water', 'A) Red dwarfs',
                                'B) The presence of an atmosphere', 'C) Kepler Space Telescope',
                                'B) A moon orbiting an exoplanet',
                                'A) Methane',
                                'C) Observing the dimming of a star\'s light as a planet passes in front of it',
                                'A) Organisms that can survive in extreme temperatures and conditions',

                                'C) Solar energy', 'C) Microcontroller',
                                'C) To detect and gather data from the environment',
                                'C) Python', 'D) Artificial Intelligence', 'A) Solar panels',
                                'A) Unlimited power supply',
                                'C) To provide mechanical movement', 'B) Enhanced safety', 'A) Cooking',
                                'C) Perseverance',
                                'A) NASA', 'B) Russia', 'B) To study the Martian atmosphere',
                                'C) MOXIE (Mars Oxygen In-Situ Resource Utilization Experiment)', 'C) Jezero Crater',
                                'C) To demonstrate powered flight in the thin Martian atmosphere', 'D) 24.6 hours',
                                'A) To collect and analyze rock and soil samples for signs of life', 'D) Curiosity',
                                'C) Europa', 'C) Pluto', 'A) NASA', 'B) Nova', 'A) Parker Solar Probe',
                                'B) Neutron Star Merger',
                                'A) Juno', 'B) Pulsar', 'A) NASA', 'B) Aphelion']

        self.answers = [['A) Mars', 'B) Earth', 'C) Venus', 'D) Jupiter'],
                        ['A) Mars', 'B) Moon', 'C) Sun', 'D) Jupiter'],
                        ['A) Proxima Centauri', 'B) Betelgeuse', 'C) Alpha Centauri', 'D) Sirius'],
                        ['A) Magnetism', 'B) Gravity', 'C) Centrifugal force', 'D) Friction'],
                        ['A) Mars', 'B) Jupiter', 'C) Venus', 'D) Saturn'],
                        ['A) Earth', 'B) Jupiter', 'C) Mars', 'D) Saturn'],
                        ['A) Andromeda', 'B) Milky Way', 'C) Triangulum', 'D) Sombrero'],
                        ['A) Neil Armstrong', 'B) Buzz Aldrin', 'C) Yuri Gagarin', 'D) Michael Collins'],
                        ['A) Hubble', 'B) Sputnik 1', 'C) Voyager 1', 'D) ISS'],
                        ['A) National Aeronautics and Space Administration', 'B) North American Space Agency',
                         'C) New Astronomical and Space Association', 'D) Nuclear Astrophysics and Space Authority'],
                        ['A) 186,282 miles per second', 'B) 150,000 miles per hour', 'C) 300,000 kilometers per second',
                         'D) 500,000 kilometers per hour'],
                        ['A) Measure temperature', 'B) Amplify sound waves', 'C) Generate electricity',
                         'D) Observe distant objects'],
                        ['A) A large telescope', 'B) A space observatory', 'C) A human-made satellite',
                         'D) A habitable artificial satellite'],
                        ['A) Spirit', 'B) Curiosity', 'C) Opportunity', 'D) Perseverance'],
                        ['A) Volt', 'B) Ampere', 'C) Ohm', 'D) Watt'],
                        ['A) Nitrogen', 'B) Oxygen', 'C) Carbon dioxide', 'D) Helium'],
                        ['A) Respiration', 'B) Photosynthesis', 'C) Transpiration', 'D) Germination'],
                        ['A) Atlantic Ocean', 'B) Pacific Ocean', 'C) Indian Ocean', 'D) Southern Ocean'],
                        ['A) Pioneer 10', 'B) Voyager 1', 'C) New Horizons', 'D) Juno'],
                        ['A) Solar flare', 'B) Lunar eclipse', 'C) Stellar wind', 'D) Comet tail'],
                        ['A) Sputnik 1', 'B) Explorer 1', 'C) Vanguard 1', 'D) Telstar 1'],
                        ['A) Mars', 'B) Venus', 'C) Mercury', 'D) Jupiter'],
                        ['A) Orbit', 'B) Trajectory', 'C) Route', 'D) Circuit'],
                        ['A) Isaac Newton', 'B) Albert Einstein', 'C) Galileo Galilei', 'D) Johannes Kepler'],
                        ['A) Kepler Space Telescope', 'B) Hubble Space Telescope', 'C) Chandra X-ray Observatory',
                         'D) Spitzer Space Telescope'],
                        ['A) Study exoplanets', 'B) Search for dark matter', 'C) Monitor solar flares',
                         'D) Explore the Kuiper Belt'],
                        ['A) Voyager 1', 'B) Pioneer 10', 'C) New Horizons', 'D) Juno'],
                        ['A) Study cosmic microwave background radiation', 'B) Investigate dark energy',
                         'C) Search for extraterrestrial intelligence', 'D) Explore the Higgs boson'],
                        ['A) Mars Science Laboratory', 'B) Cassini-Huygens', 'C) Galileo', 'D) Rosetta'],
                        ['A) Faster-than-light travel', 'B) Time dilation', 'C) Wormhole creation',
                         'D) Antimatter propulsion'],
                        ['A) Supernova', 'B) Black hole', 'C) Neutron star', 'D) Pulsar'],
                        ['A) Search for extraterrestrial life on Mars',
                         'B) Return humans to the Moon and go on to Mars',
                         'C) Study the outer planets with robotic missions',
                         'D) Establish a permanent human presence on the Moon'],
                        ['A) NASA', 'B) ESA', 'C) ISRO', 'D) CNSA (China National Space Administration)'],
                        ['A) Send small spacecraft to another star system', 'B) Search for extraterrestrial signals',
                         'C) Study the atmospheres of exoplanets', 'D) Develop advanced space habitats'],
                        ['A) James Webb Space Telescope', 'B) Kepler Space Telescope', 'C) Planck Telescope',
                         'D) WISE (Wide-field Infrared Survey Explorer)'],
                        ['A) Explorer 1', 'B) Vanguard 1', 'C) Sputnik 1', 'D) Telstar 1'],
                        ['A) Mars', 'B) Venus', 'C) Jupiter', 'D) Saturn'],
                        ['A) Apogee', 'B) Perigee', 'C) Zenith', 'D) Nadir'],
                        ['A) Valentina Tereshkova', 'B) Sally Ride', 'C) Yuri Gagarin', 'D) Mae Jemison'],
                        ['A) Mars Express', 'B) Mars Science Laboratory', 'C) Mars Pathfinder',
                         'D) Mars Reconnaissance Orbiter'],
                        ['A) Interstellar Space Station', 'B) International Science Shuttle',
                         'C) In conterplanetary Solar System', 'D) International Space Station'],
                        ['A) Gravitational force', 'B) Magnetic force', 'C) Centrifugal force',
                         'D) Electrostatic force'],
                        ['A) Apollo 8', 'B) Apollo 11', 'C) Apollo 13', 'D) Apollo 17'],
                        ['A) Great Red Spot', 'B) White Oval', 'C) Dark Spot', 'D) Blue Storm'],
                        ['A) Helium', 'B) Hydrogen', 'C) Oxygen', 'D) Carbon'],
                        ['A) Equator', 'B) Meridian', 'C) Tropic', 'D) Latitude'],
                        ['A) Fusion', 'B) Fission', 'C) Convection', 'D) Radiation'],
                        ['A) Mars', 'B) Earth', 'C) Venus', 'D) Neptune'],
                        ['A) Titan', 'B) Europa', 'C) Ganymede', 'D) Io'],
                        ['A) ESA', 'B) ISRO', 'C) NASA', 'D) Roscosmos'],

                        ['A) Search for signs of life on the Moon', 'B) Explore the lunar poles for water ice',
                         'C) Study the geology of lunar highlands', 'D) Investigate the Moon magnetic field'],
                        ['A) NASA', 'B) ESA (European Space Agency)', 'C) CNSA (China National Space Administration)',
                         'D) ISRO (Indian Space Research Organisation)'],
                        ['A) Chandrayaan-1', 'B) Chandrayaan-2', 'C) Mangalyaan', 'D) Astrosat'],
                        ['A) Vikram lander', 'B) Pragyan rover', 'C) Chandrayaan-2 orbiter',
                         'D) All components landed successfully'],
                        ['A) Enhanced rover capabilities', 'B) Ability to study lunar volcanism',
                         'C) Focus on magnetic anomalies', 'D) Study of lunar atmospherics'],
                        ['A) Lunar equator', 'B) Lunar far side', 'C) Lunar poles', 'D) Lunar highlands'],
                        ['A) Strong magnetic fields', 'B) Extreme surface temperatures',
                         'C) Potential presence of water ice', 'D) Higher seismic activity'],
                        ['A) Pragyan', 'B) Yutu', 'C) Sojourner', 'D) Opportunity'],
                        ['A) Pyrometer', 'B) Telecoms', 'C) Seismometer', 'D) Robots'],
                        ['A) Shelter from solar radiation', 'B) Access to abundant helium-3',
                         'C) Ease of communication with Earth', 'D) Presence of ancient artifacts'],
                        ['A) A planet located outside our solar system', 'B) A planet with rings similar to Saturn',
                         'C) A planet within the asteroid belt', 'D) A planet larger than Earth'],
                        ['A) Gravitational waves', 'B) Direct imaging', 'C) Spectroscopy', 'D) All of the above'],
                        ['A) The region where life already exists',
                         'B) The area around a star where conditions may support liquid water',
                         'C) The zone where only microbial life can survive',
                         'D) The outer edge of a planetary system'],
                        ['A) Red dwarfs', 'B) Blue giants', 'C) White dwarfs', 'D) Yellow dwarfs'],
                        ['A) The planets size', 'B) The presence of an atmosphere', 'C) The composition of the surface',
                         'D) The existence of a magnetic field'],
                        ['A) Hubble Space Telescope', 'B) Spitzer Space Telescope', 'C) Kepler Space Telescope',
                         'D) James Webb Space Telescope'],
                        ['A) A moon within our solar system', 'B) A moon orbiting an exoplanet',
                         'C) A moon orbiting a black hole', 'D) A moon with extreme temperatures'],
                        ['A) Methane', 'B) Carbon monoxide', 'C) Oxygen', 'D) Nitrogen'],
                        ['A) Directly photographing the exoplanet',
                         'B) Measuring the stars wobble due to the planets gravitational pull',
                         'C) Observing the dimming of a stars light as a planet passes in front of it',
                         'D) Detecting radio signals from exoplanets'],
                        ['A) Organisms that can survive in extreme temperatures and conditions',
                         'B) Microscopic organisms found on Mars',
                         'C) Organisms that thrive in Earth-like environments',
                         'D) Creatures living in oceanic depths'],
                        ['A) Batteries', 'B) Gasoline', 'C) Solar energy', 'D) Wind power'],
                        ['A) Actuator', 'B) Sensor', 'C) Microcontroller', 'D) Power source'],
                        ['A) To store data', 'B) To process information',
                         'C) To detect and gather data from the environment', 'D) To provide mechanical movement'],
                        ['A) Java', 'B) C++', 'C) Python', 'D) HTML'],
                        ['A) Artificial Invention', 'B) Automated Intelligence', 'C) Advanced Interface',
                         'D) Artificial Intelligence'],
                        ['A) Solar panels', 'B) Wheels', 'C) Microcontroller', 'D) Batteries'],
                        ['A) Unlimited power supply', 'B) Heavy fuel consumption', 'C) High maintenance costs',
                         'D) Limited mobility'],
                        ['A) To process information', 'B) To detect and gather data',
                         'C) To provide mechanical movement', 'D) To store data'],
                        ['A) Increased speed', 'B) Enhanced safety', 'C) Reduced environmental impact',
                         'D) Limited mobility'],
                        ['A) Cooking', 'B) Reading books', 'C) Painting', 'D) Breathing'],
                        ['A) Curiosity', 'B) Opportunity', 'C) Perseverance', 'D) Spirit'],
                        ['A) NASA', 'B) ESA (European Space Agency)', 'C) ISRO (Indian Space Research Organisation)',
                         'D) Roscosmos (Russian Space Agency)'],
                        ['A) China', 'B) Russia', 'C) Japan', 'D) Canada'],
                        ['A) To analyze soil samples', 'B) To study the Martian atmosphere',
                         'C) To search for signs of microbial life', 'D) To capture panoramic images'],
                        ['A) Solar panels', 'B) Nuclear power',
                         'C) MOXIE (Mars Oxygen In-Situ Resource Utilization Experiment)', 'D) Wind turbines'],
                        ['A) Olympus Mons', 'B) Valles Marineris', 'C) Jezero Crater', 'D) Hellas Planitia'],
                        ['A) To search for water', 'B) To study the Martian surface',
                         'C) To demonstrate powered flight in the thin Martian atmosphere',
                         'D) To communicate with Earth'],
                        ['A) 24 hours', 'B) 36 hours', 'C) 48 hours', 'D) 24.6 hours'],
                        ['A) To collect and analyze rock and soil samples for signs of life',
                         'B) To generate electricity', 'C) To communicate with other rovers',
                         'D) To study the Martian climate'],
                        ['A) Spirit', 'B) Opportunity', 'C) Sojourner', 'D) Curiosity'],
                        ['A) Ganymede', 'B) Io', 'C) Europa', 'D) Callisto'],
                        ['A) Haumea', 'B) Eris', 'C) Pluto', 'D) Makemake'],
                        ['A) NASA (National Aeronautics and Space Administration)', 'B) ESA (European Space Agency)',
                         'C) CNSA (China National Space Administration)',
                         'D) ISRO (Indian Space  Research Organisation)'],
                        ['A) Supernova', 'B) Nova', 'C) Ulsar', 'D)Quasar'],
                        ['A) Parker Solar Probe', 'B) Solar Orbiter', 'C) STEREO', 'D) Ulysses'],
                        ['A) Gamma-ray Burst', 'B) Neutron Star Merger', 'C) Pulsar Wind', 'D) Quasar Emission'],
                        ['A) Juno', 'B) Galileo', 'C) Cassini-Huygens', 'D) Voyager'],
                        ['A) Quasar', 'B) Pulsar', 'C)Black Hole', 'D) Neutron Star'],
                        ['A) NASA (National Aeronautics and Space Administration)',
                         'B) CNSA (China National Space Administration)',
                         'C) ISRO (Indian Space Research Organisation)',
                         'D) Roscosmos(Russian Federal Space Agency)'],
                        ['A) Apogee', 'B) Aphelion', 'C) Perigee', 'D) Erihelion']]

        self.index = self.questions.index(random.choice(self.questions)) - 1
        self.current_question = self.index
        self.score = 0
        self.count = 0

        self.question_label = tk.Label(self.root, text=self.questions[self.current_question],
                                       font=("Times New Roman", 30),
                                       bg="black", fg="white", wraplength=1000)
        self.question_label.pack()

        self.selected_option = tk.StringVar()

        self.option_button1 = tk.Radiobutton(self.root, text=self.answers[self.index][0], variable=self.selected_option,
                                             value=self.answers[self.index][0], font=("Bahnschrift Condensed", 25),
                                             bg="black", fg="purple",
                                             command=self.play_sound, wraplength=800)
        self.option_button2 = tk.Radiobutton(self.root, text=self.answers[self.index][1], variable=self.selected_option,
                                             value=self.answers[self.index][1], font=("Bahnschrift Condensed", 25),
                                             bg="black", fg="purple",
                                             command=self.play_sound, wraplength=800)
        self.option_button3 = tk.Radiobutton(self.root, text=self.answers[self.index][2], variable=self.selected_option,
                                             value=self.answers[self.index][2], font=("Bahnschrift Condensed", 25),
                                             bg="black", fg="purple",
                                             command=self.play_sound, wraplength=800)
        self.option_button4 = tk.Radiobutton(self.root, text=self.answers[self.index][3], variable=self.selected_option,
                                             value=self.answers[self.index][3], font=("Bahnschrift Condensed", 25),
                                             bg="black", fg="purple",
                                             command=self.play_sound, wraplength=800)
        self.option_button1.pack()
        self.option_button2.pack()
        self.option_button3.pack()
        self.option_button4.pack()

        self.submit_button = tk.Button(self.root, text="Submit", command=self.check_answer, font=("Helvetica", 26),
                                       bg="black", fg="white", height=1, width=7)
        self.submit_button.pack()

        self.score_label = tk.Label(self.root, text="Score: 0", font=("Impact", 23), bg="black", fg="white")
        self.score_label.pack()

        self.submit_button.bind("<Enter>", self.on_enter)
        self.submit_button.bind("<Leave>", self.on_leave)

        th1 = threading.Thread(target=self.t2s, args=(self.questions[self.current_question],))
        th1.start()

    def check_answer(self):
        user_answer = self.selected_option.get()
        correct_answer = self.correct_answers[self.current_question]

        selected_letter = user_answer.split(')')[0] + ')'
        right = ["Wonderful!", "Amazing!", "Great! That's Correct", "Keep it up", "Excellent!"]
        wrong = ["Keep Trying", "Uh-oh! Incorrect Answer", "Try Again", "Don't Worry, Keep Trying", "You can do it!"]
        if selected_letter == correct_answer.split(')')[0] + ')':
            self.score += 1
            self.score_label.config(text=f"Score: {self.score} {random.choice(right)}")
        else:
            self.score_label.config(text=f"Score: {self.score} {random.choice(wrong)}")
        self.selected_option.set("")

        if self.count < 3:
            pygame.mixer.init()
            sound1 = pygame.mixer.Sound(
                "zapsplat_foley_plastic_button_med_small_single_press_bright_click_003_72185.mp3")
            channel1 = pygame.mixer.Channel(1)
            channel1.play(sound1)
            self.index = self.questions.index(random.choice(self.questions)) - 1
            self.current_question = self.index
            self.question_label.config(text=self.questions[self.current_question])
            self.option_button1.config(text=self.answers[self.index][0], value=self.answers[self.index][0])
            self.option_button2.config(text=self.answers[self.index][1], value=self.answers[self.index][1])
            self.option_button3.config(text=self.answers[self.index][2], value=self.answers[self.index][2])
            self.option_button4.config(text=self.answers[self.index][3], value=self.answers[self.index][3])
            self.option_button1.pack()
            self.option_button2.pack()
            self.option_button3.pack()
            self.option_button4.pack()
            self.count += 1
            th1 = threading.Thread(target=self.t2s, args=(self.questions[self.current_question],))
            th1.start()
            # self.t2s(self.questions[self.current_question])
        else:
            pygame.mixer.init()
            pygame.mixer.music.load("little_robot_sound_factory_Jingle_Win_Synth_03.mp3")
            pygame.mixer.music.play()
            self.question_label.config(text="Quiz Completed!")
            self.show_final_score()
            self.submit_button.pack_forget()
            self.score_label.pack_forget()
            self.submit_button.pack_forget()
            self.option_button1.pack_forget()
            self.option_button2.pack_forget()
            self.option_button3.pack_forget()
            self.option_button4.pack_forget()

    def show_final_score(self):
        final_message = f"Quiz Complete\nFinal Score: {self.score}/4"
        final_label = tk.Label(self.root, text=final_message, font=("Helvetica", 20), pady=20, bg="black", fg="white")
        final_label.pack()

        if self.score == 4:

            congrats = ["You're a Quiz Master!", "Perfect Score!", "You're a Genius"]
            message = random.choice(congrats)
            message="THANKS FOR PLAYING"
            congrats_label = tk.Label(self.root, text=message, font=("Arial", 18, "italic"), fg="green")
            congrats_label.pack()
        elif self.score == 3:

            nice = ["Well Done!", "Great Job!", "Nice Effort!"]
            message = random.choice(nice)
            nice_label = tk.Label(self.root, text=message, font=("Arial", 16), fg="blue")
            nice_label.pack()
        else:
            sorry = ["Keep Going!", "Better Luck Next Time!", "Practice Makes Perfect!"]
            message = random.choice(sorry)
            sorry_label = tk.Label(self.root, text=message, font=("Arial", 14), fg="red")
            sorry_label.pack()

        for widget in self.root.winfo_children():
            if isinstance(widget, (tk.Radiobutton, tk.Entry, tk.Button)):
                widget.pack_forget()


root = tk.Tk()
game = QuizGame(root)
root.mainloop()
