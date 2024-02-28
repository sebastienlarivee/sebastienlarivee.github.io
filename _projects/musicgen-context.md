---
layout: default
---

# MusicGen Sliding Context Window

idk if this is a big enough project to include. maybe ill make a misc section

Github: https://github.com/sebastienlarivee/musicgen-long-generation

In June 2023 Facebook Research released [MusicGen](https://ai.honu.io/papers/musicgen/), a really great open source AI model for generative music. The model itself is trained on 30 seconds chunks of music and as such can natively only generate audio of that length. But the release blogpost does feature some longer examples and mentions:

> It is possible to generate longer sequence using a fixed 30 seconds windows. We then slide the window by chunks of 10 seconds, keeping the last 20 seconds that were generated as context.
> 

However on release none of Facebook’s Gradio-based demos or example code included this feature. I was very excited about the model and wanted to try longer generations right away, so I made a simple Jupyter notebook which uses PyTorch to implement the sliding context window described in the blogpost.

Here’s some lo-fi beats style music I generated with it:

[drained ext1.wav](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/e6c58442-e438-447f-a52c-d3c77ec5308e/drained_ext1.wav)

[im in it.wav](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/8bed96cb-2db7-4629-bbdd-54277d0ca721/im_in_it.wav)

[im in it.wav](https://prod-files-secure.s3.us-west-2.amazonaws.com/660769e3-8df5-4a98-a153-35c58b80abbc/6d979fa9-70cd-4e94-a108-ef925431999b/im_in_it.wav)

As you’d expect these samples start to lose coherence past the 30 second mark. Even with a 20 second context window the model’s continuations drift substantially from the source audio.

I mainly used this during the first few days of release and it was quite quickly obsoleted by more refined tools, but it was a great way to start learning Pytorch’s audio features. For an up to date, feature rich, MusicGen dashboard I like [Audiocraft Plus](https://github.com/GrandaddyShmax/audiocraft_plus/tree/plus).