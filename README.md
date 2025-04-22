<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->




<!-- PROJECT LOGO -->
<br />
<div align="center">
  </a>

  <h3 align="center">Segmented Generation & Trait-Based Systems</h3>

  <p align="center">
    This project visualizes identity by transforming a user's MBTI, moral alignment, and favorite color into layered Voronoi diagrams. Through recursive subdivision and trait-driven visual rules, the system encodes psychological attributes into a unique generative portrait.
<!--     <br />
    <a href="https://github.com/manguyen0017/4-comma-Assignment_01/blob/main/pdf/Yonkoma__The_Headaches_of_Iterative_AI_Generation.pdf"><strong>Link to PDF Report»</strong></a>
    <br /> -->
    <br />
    <a href="https://editor.p5js.org/manguyen0017/sketches/mO1DKlAn-"><strong>Link to DEMO prototype></a>
    <br />
    <br />
    <a href="https://manguyen.myportfolio.com/">Michael Nguyen</a>
    &middot;
    <a href="https://sites.google.com/view/viza626/home">VIZA 626</a>
  </p>
</div>

[![Voronoi][images-fig1]](https://github.com/manguyen0017/Segmented-Generative-Pipeline-Assignment_02/blob/main/images/fig1.png)

Figure 1. Full output visual of one complete generative portrait progressing from favorite color to deepest sublayer.

<!-- Abstract -->
## Abstract
This project generates an animated “personality portrait” by combining nested Voronoi diagrams with a custom cellular‑automata overlay. Users specify a base hue (HSV), an MBTI code, and a moral alignment; these inputs drive how cells are seeded, how their colors shift per generation, and how vibrancy and spatial irregularity respond to Good/Evil and Lawful/Chaotic settings. Cells then evolve under a B34/S234 life rule (birth on 3 or 4 neighbors; survival on 2–4), with smooth lerp‑based interpolation to ensure “bounce‑back” oscillations instead of collapse. The result is a perpetual, trait‑reflective animation.

<!-- Introduction and Related Works -->
## Introduction and Related Works
Generative art that encodes personal data into abstract visuals has grown rapidly, leveraging psychology to inform form. Early work by Holtzman (2016) demonstrated how user inputs can map to geometric parameters in interactive pieces [3]. More recent studies in Explainable AI (Guckelsberger et al. 2017; Abdul et al. 2018) emphasize transparency and user trust in procedural systems [1][2], suggesting that visual metaphors for internal logic deepen engagement.

MBTI‐based coloration has been explored for static portraits [Electronics 2024][4], but few systems animate personality over time. Conway’s Game of Life (1970) pioneered emergent cellular animation, yet its classic rules often lead to stasis or extinction [5]. By blending trait inheritance (70% persistence, 20% neutral, 10% flip) with modified B34/S234 rules and smooth interpolation, MBTI Voronoi Life maintains perpetual motion while preserving trait coherence. This fusion of layered Voronoi segmentation and balanced life rules offers a novel avenue for living data portraiture.


### Built With
* ChatGPT
* P5.js

## Methodology

The generator combines p5.js and d3.voronoi to build three nested Voronoi layers from user inputs, and adds a smooth cell‑automaton that continuously animates each cell’s “alive” state. Users still enter:

- **Color (HSV):** Base hue, saturation, brightness  
- **MBTI Code:** Four letters that seed cell positions and hue shifts  
- **Moral Alignment:** Good/Neutral/Evil (vertical bias) and Lawful/Neutral/Chaotic (positional noise)

[![Voronoi][images-fig2]](images/fig2.png)

**Figure 2.** How HSV, MBTI, and alignment feed into cell color, position, and behavior.

[![Voronoi][images-fig3]](images/fig3.png)

**Figure 3.** Pipeline from raw inputs through three nested Voronoi layers.

### Layer Structure & Pipeline

* **Base Layer**: Four large cells arranged on a circle—one per MBTI letter. Each letter shifts hue by ±5–20°.  
* **Sublayer 2**: Each base cell spawns **3** smaller cells inside it. Moral traits (Good = +1, Neutral = 0, Evil = –1) are assigned with a **1:2:1**, **2:1:1**, or **1:1:2** bias depending on vertical alignment.  
* **Sublayer 3**: Each sub‑cell generates **2** further cells. Traits inherit from parent with **70%** chance, neutralize **20%**, or flip **10%**.

### Life Simulation

![Figure 7][images-fig7]

**Figure 7.** Cell “alive” value pulses over time under the birth/survival rules.

* Each cell maintains a continuous `alive` value in [–1, 1].  
* **Birth**: A dead cell (`alive ≤ 0`) becomes alive if it has **3 or 4** alive neighbors.  
* **Survival**: A live cell (`alive > 0`) remains alive if it has **2, 3, or 4** alive neighbors.  
* On every frame (60 fps), `alive` values interpolate toward ±1 using a **regen** factor (e.g. 0.02) for smooth pulsing.  
* The play/pause toggle controls whether the simulation steps; UI sliders and visibility toggles remain responsive even when paused.

### Color Logic

* **Hue**: Starts from the user’s chosen hue, then shifts by MBTI and layer depth (clamped to [0,360]).  
* **Saturation/Brightness**: Modified by moral alignment (Good brightens +15%, Evil darkens –15%) and further modulated by each cell’s current `alive` value (±20%).  
* **Determinism**: A user‑defined random seed ensures identical results on reload.

Randomness is locked to the seed. Users can toggle layers, trait‐number labels, and the life animation at any time.

### Technical Highlights

* **Deterministic Generation**: Fixed seed → reproducible portraits.  
* **Trait Inheritance**: Probabilistic moral trait passing (70/20/10).  
* **Smooth Animation**: Life states ease via linear interpolation, preventing abrupt flicker.  
* **Interactive UI**: Play/pause, layer opacity sliders, and trait‐overlay toggles work continuously.

Libraries used: **p5.js**, **p5.sound**, **d3.v5**

[![Voronoi][images-fig8]](images/fig8.png)

Figure 8. Randomized exploration examples.

## Result and Future Work

The enhanced system now produces living, breathing portraits that continuously evolve rather than remaining static images. As the cellular automaton pulses each cell’s “alive” value between –1 and +1, you see smooth color and shape transitions driven by MBTI‑informed hue shifts and moral‑alignment brightness changes. Lawful alignments yield rhythmic, symmetric pulses; chaotic alignments introduce irregular flicker and clustering. Users can adjust layer opacity and visibility at any time—even when the animation is paused—allowing on‑the‑fly exploration of structural depth and trait interplay. The result is a layered, interactive portrait that not only encodes identity into form and color but also into time‑based behavior, inviting deeper engagement and interpretation.

### Future Work
* Expose life‑rule parameters (birth/survival counts, regen rate) in the UI for user‑defined animations.  
* Add export functionality to record animated sequences as GIFs or videos.  
* Experiment with alternative automata (e.g., Brian’s Brain, Lenia) for richer cell behaviors.  
* Incorporate additional personality models (Big Five, Enneagram) to diversify trait mappings.  
* Optimize rendering performance for higher resolutions and mobile devices.

## Conclusion

By combining trait‑driven Voronoi subdivision with a smooth, continuous life simulation, this project transforms static personality portraits into dynamic, interactive narratives. It demonstrates how psychological inputs—MBTI letters, moral alignment, favorite color—can shape not only form and hue but also temporal behavior. The result is a compelling framework for personal data portraiture, where generative art becomes a living reflection of user identity.


<!-- Bibliography -->
## Bibliography 
[1] Guckelsberger, C., Salge, C., & Colton, S. (2017). Addressing the “Why?” in Computational Creativity: A Non-Anthropocentric, Minimal Model of Intentional Creative Agency. Proceedings of the Eighth International Conference on Computational Creativity.

[2] Abdul, A., Vermeulen, J., Wang, D., Lim, B. Y., & Kankanhalli, M. (2018). Trends and Trajectories for Explainable, Accountable and Intelligible Systems: An HCI Research Agenda. Proceedings of the 2018 CHI Conference on Human Factors in Computing Systems.

[3] Holtzman, H. (2016). The Art of Interaction: What HCI Can Learn from Interactive Art. Interactions, 23(2), 44–49.

[4] Electronics. (2024). Integrating Generative AI with Personality Models. Electronics, 13(23), 4751. https://www.mdpi.com/2079-9292/13/23/4751

<!-- CONTACT -->
## Contact

Michael Nguyen - manguyen@tamu.edu

Personal Website: [manguyen.myportfolio.com](https://manguyen.myportfolio.com)




<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

This work is submitted as part of Assignment 1 for the VIZA 626 course at Texas A&M University, under the instruction of Professor You-Jin Kim, during the Spring 2025 semester.

VIZA 626 Class Website: [https://sites.google.com/view/viza626/](https://sites.google.com/view/viza626/home)

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[product-screenshot]: images/screenshot.png
[images-fig1]: images/fig1.png
[images-fig2]: images/fig2.png
[images-fig3]: images/fig3.png
[images-fig4]: images/fig4.png
[images-fig5]: images/fig5.png
[images-fig6]: images/fig6.png
[images-fig7]: images/fig7.png
[images-fig8]: images/fig8.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 
