---
layout: default
title: Resources
nav_order: 6
---
# Neuroanatomy
- [MRI with neuroanatomical labels](https://neuroanatomy.ca/mri.html)

# Neuroscience Fundamentals
- [Free online textbook](https://nba.uth.tmc.edu/neuroscience/toc.htm). Good chapters for new students:
  - [Overview of the nervous system](https://nba.uth.tmc.edu/neuroscience/s2/chapter01.html)
  - [Introduction to neurons and neuronal networks](https://nba.uth.tmc.edu/neuroscience/s1/introduction.html)
  - [Organization of cell types](https://nba.uth.tmc.edu/neuroscience/s1/chapter08.html)
  - [Hippocampus](https://nba.uth.tmc.edu/neuroscience/s4/chapter05.html)

# MRI Physics
- C Baron has copies of [Principles of Magnetic Resonance Imaging](https://www.lulu.com/shop/dwight-nishimura/principles-of-magnetic-resonance-imaging/paperback/product-22829760.html?page=1&pageSize=4) by Nishimura that trainees can borrow for short periods of time
  - It is not expensive (see link above), and buying a personal copy is not a bad idea.
- [Basics of MRI physics at accessible level](https://www.mriquestions.com/index.html)
- [Diffusion MRI free online textbook](https://academic.oup.com/book/24921)
- [Diffusion MRI online Module, C Baron](https://uwoca-my.sharepoint.com/:f:/g/personal/cbaron4_uwo_ca/EjlDmLYA78tBmPmDHc8VF5YBA36egbXvNj3m1jX_XTDTsQ?e=CJocCw)

# Image Segmentation and Registration
- The following links introduce the concepts of image segmentation and image registration in general and in the context of MRI for students who may be new to these methods:
- Image Segmentation:
  - [Image Segmentation (MathWorks):](https://www.mathworks.com/discovery/image-segmentation.html) A broad, beginner-friendly explanation of image segmentation
  - [Image Segmentation in Medical Imaging:](https://www.statcan.gc.ca/en/data-science/network/image-segmentation) A technical explanation of image segmentation with underlying math for univariate and multivariate segmentation models
  - [Image Segmentation with SynthSeg+:](https://www.pnas.org/doi/10.1073/pnas.2216399120) A paper written on the topic of SynthSeg+, a tool used for brain MRI segmentation with examples of its uses.
- Image Registration:
  - [Image Registration (MathWorks):](https://www.mathworks.com/help/images/image-registration.html) A short explanation on image registration which also covers MATLAB functions and examples (This is more useful for practical implementation)
  - [Image Registration:](https://3dqlab.stanford.edu/image-registration/) A pipeline/step-by-step overview of a 'typical' image registration process
    
# Tips for thesis exam
- Some things everyone doing diffusion MRI research should know:
  - How things change with magnetic field strength
    - e.g., SNR, main field homogeneity and off-resonance, B1 field inhomogeneity (and impact of this on parallel imaging), relaxation times
  - Typical Values:
    - e.g., relaxation times in various tissues, ADC values in various tissues, typical experimental diffusion times, and how that relates to diffusion-based mean-squared displacement, max gradient for hardware (and why they typically are lower than hardware max during frequency-encoding), max slew rates (and why they typically need to be limited to below hardware max)
  - Units for the parameters you work with
  - How noise propagates in parameter estimation
- You must know the "why" and trade-offs for all parameter choices
  - Review all your experimental methods to refresh your memory
  - Be honest about limitations and/or shortcomings
- Think about how your work could potentially be clinically translated, including barriers to it
  - Don't forget that, aside from clinical translation, usage in basic science investigations to learn more about the brain is also valuable
- Expect high level questions like "where do you think this field is going"
- Have general knowledge about the condition you may be studying
  - e.g., how is it diagnosed, subtypes, prevalence, treatments, typical outcomes
- Know the background and details behind anything you put in your thesis text
- Be ready for statistics questions. 
  - Did you do the appropriate tests? Type 1 error? Multiple comparisons?
- Know the literature! If you've been reading papers throughout your program, this should not be too difficult. But if you haven't been reading throughout, it is very hard to catch up while prepping for your exam.
