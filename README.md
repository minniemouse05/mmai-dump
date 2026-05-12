# MMAI 2026 - Minnie Liang

Welcome to my site for Modeling Multimodal AI 2026!
This repo contains my homework assignments and random thoughts throughout the class.

## Bio

<img src="./imgs/profile-pic.png" style="width:200px;">

Hi, I'm Minnie Liang! I'm a junior at MIT studying Computer Science and Artificial Intelligence. I love thinking about challenging problems (especially one with lots of data!) using algorithms and machine learning. I'm passionate about creating practical, impactful solutions that bridge theory and real-world applications.

In my free time, you can catch me:

- Running! (ask me about the latest place I've ran to- likely would be Trader Joe's)
- Baking! constantly experimenting with new flavor combos (recent favorite has been strawberry hojicha cheesecake)
- Binging movies & shows! (I love crime & thriller. Recent favs: The Grand Budapest Hotel and Wake Up Dead Man)

## Final Project

For my final project, my team and I took the Multimodal Transformer (MuLT) and gave it a noise-aware upgrade! Instead of blindly fusing audio, video, and text together, we let the model figure out how noisy each signal is on a per-sample basis and adjust its fusion depth accordingly (basically, if a modality is too noisy, don't force it through all the cross-modal layers).

We built a noise estimator + gating network to handle the routing, and ran a bunch of experiments on CMU-MOSEI. The results gave us a ton of insight into what makes variance-based noise detection tricky across different modalities!

Our slide deck:

[![Slides](imgs/thumbnail.png)](https://docs.google.com/presentation/d/1RIiWg-sNTqqaZAbsKy8Lk0pZTBd9UXwZtkv0OzogJC8/edit?usp=sharing)

[View my project code here](./final-project/)

## Homework

- [Homework 1 - Dataset: Extracting Modalities from Cooking Videos to See If t-SNE Thinks Stir-Fry and Sautéing Are the Same Thing](./homework/homework-1/)
- [Homework 2 - Fusion: Four Ways to Combine Someone's Face and Voice to Predict How Positive They're Feeling
   ](./homework/homework-2/)
- [Homework 3 - VLM: Fine-Tuning Qwen to Read Audio and Face Heatmaps and Guess If Someone's Having a Good Day](./homework/homework-3/)
- [Homework 4 - Teaching Qwen to Reason About Heatmaps by Rewarding Itself!](./homework/homework-4/)
- [Homework 5 - Building a Restaurant Recommendation Bot That Browses the Web So You Don't Have To!](./homework/homework-5/)

## Website License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
