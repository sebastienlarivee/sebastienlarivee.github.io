---
layout: default
---

# MusicGen Finetunes

[Github](https://github.com/sebastienlarivee/musicgen-finetune)

[Model Download](https://drive.google.com/drive/folders/1gqRlnr2UOxBVQATlOY5JC-qpa51RRISu?usp=drive_link)

Facebook Research’s [MusicGen](https://ai.honu.io/papers/musicgen/) is a great open source AI model for generating music and is pretty straightforward to finetune thanks to the [AudioCraft](https://github.com/facebookresearch/audiocraft) library that it’s part of. As this would be my first finetune I wanted to make it as forgiving as possible to hopefully get a decent result even with  sub-optimal amounts of data and training time. So I decided to make a model optimized for lo-fi beats as 1) imperfections are already a stylistic quality of the genre, and 2) [I’ve produced this kind of music before](https://www.youtube.com/@hoennbeats/releases).

### Process:

[Google Colab](https://colab.research.google.com/) with a T4 16GB GPU)

1. Scraped a YouTube playlist with roughly 1200 lo-fi songs.
2. Chunked each track into 30 second .wav files. This is the maximum audio length that MusicGen will accept for data.
3. Use the [Essentia](https://github.com/MTG/essentia) library to automatically label each file’s BPM, moods, instrumentation, etc.
4. Split the dataset to have a training set and an evaluation set.
5. Compress the dataset and send it to Google Cloud storage.

[RunPod](https://www.runpod.io/) with 2 RTX A6000 48GB GPUs)

1. Load the data from Google Cloud.
2. Set training parameters. I selected the large 3.3B parameter version of MusicGen and set training time to 5 epochs.
3. Train for around 8 hours.
4. Export the model to compression_state_dict.bin and state_dict.bin for use in audio generation.

And here’s what the model sounds like with the prompt “relaxing piano calm jazzy hip hop”:

[relaxing piano calm jazzy hip hop 3.mp3](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/fcc886ab-cb85-48a4-b907-f3f94afd5e86/relaxing_piano_calm_jazzy_hip_hop_3.mp3)

[chill piano jazzy hip hop.wav](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/0884e502-839d-4982-8716-67c718025e29/chill_piano_jazzy_hip_hop.wav)

[relaxing piano calm jazzy hip hop 2.mp3](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/524e11df-b8e9-4c9b-b1ed-a091472c1c91/relaxing_piano_calm_jazzy_hip_hop_2.mp3)

[relaxing piano calm jazzy hip hop 4.mp3](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/4d5c5a64-8c89-4c48-a786-6fee821edacf/relaxing_piano_calm_jazzy_hip_hop_4.mp3)

The loss function during training:

![Screen Shot 2023-09-07 at 3.41.52 AM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/8b86ee91-f33f-4ba8-a71f-7fe1640d37ee/Screen_Shot_2023-09-07_at_3.41.52_AM.png)

Notes:

- As you may have heard, the samples tend to cut off around the 27 second mark, even though I had set MusicGen to generate 30 seconds. I’m not sure exactly what caused this, my best guess is it’s because I didn’t concatenate my dataset into a single audio file before splitting. This means that the last audio file split from every song would usually be less than 30 seconds long (ex: a song that’s 1m40s would be split into three 30 second chunks, and one 10 second chunk). Maybe this caused the model to only generate audio of the average length of the dataset? I’m not confident in this explanation but it’s something I’ll try modifying for my next finetunes.
- Usually I’d be able to get a [checkpoint.th](http://checkpoint.th) file at the end of the training process which would allow me to continue from the last trained epoch at a later date. Unfortunately my RunPod instance crashed before I could download it. Not exactly sure what caused the crash, but in the future I’ll try setups that automatically download the checkpoint at the end of each epoch.
- I originally saved my ~75GB dataset on Google Drive. This was sub-optimal as it was too large of a file to download onto RunPod through the command line due to Drive download quotas. This forced me to move to Google Cloud storage, which was a bit finnicky to set up, but good to learn in the long term as it’s much more suited for moving large files around.