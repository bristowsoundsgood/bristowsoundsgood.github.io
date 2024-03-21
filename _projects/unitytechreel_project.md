---
layout: page
title: unity x wwise
description: short spatial audio demo
img: assets/img/thumbnail_unity.png
importance: 3
category: work
---

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
    <iframe width="750" height="400" src="https://www.youtube.com/embed/ZnSvjqGVgK4?si=eu6svnvFTeIFU4Tw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen style="border-radius: 5px;"></iframe>
    </div>
</div>

This scene was a few days' work in the Unity & Wwise Demo Scene downloaded from the Audiokinetic Launcher. With minimal gameplay mechancis and basic graphics, intricate ambience beds and suitable spatial audio implementation had to pull the weight for immersion.

<strong>1. <u>WWISE TECHNIQUES</u></strong>

<strong>// granulated ambience</strong>

Long ambience assets were made less static through heavy granulation. The two most prominent ambience loops and their variations were granulated into 2-5 second chunks inside random containers - configured to play continuously, then loop through each grain with a slightly randomised cross-fade time.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/granulation_wwise_01.png" title="Wwise Granulation: Forest Ambience" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/granulation_wwise_02.png" title="Wwise Granulation: Oasis Ambience" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/granulation_wwise_03.png" title="Wwise Random Container Granulation Settings" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

This worked notably well for the oasis ambience loop, as the original file had subtle musical tones. Granulation smeared the tones into something more indistinct and fleeting.

Voices were controlled appropriately as a whole on the ambience actor-mixer.

<strong>// muted footsteps</strong> 

Footstep SFX were controlled by switches, though this became an issue when I added a lake into the scene. Footsteps would sound even if the player was no longer moving on land. I navigated this by adding a 'mute' switch, triggered upon entering the lake's boundaries. This could have also been achieved by raytracing in C#, though I opted for a purely middleware-based solution.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/footsteps_switches_01.png" title="Footstep Switch Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/footsteps_switches_02.png" title="Footstep SFX Switch Container" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div> 

<strong>2. <u>SPATIAL AUDIO TECHNIQUES</u></strong>

<strong>// reflect plugin</strong>

AkSurfaceReflector components were added to surfaces (e.g. cave walls) and acoustic texture parameters were set for appropriate transmission loss qualities. 

Passing the cabin mesh through as geometry to Reflect was found to cause crippling CPU spikes as it is made up of too many triangles. This issue caused bother for a while. Thanks to the Wwise Profilers and systematically removing & adding objects back into the scene, I was able to find the cause.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_reflect_fpscontroller.png" title="Spatial Audio Listener Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_reflect_aksurfacereflector.png" title="AkSurfaceReflector Setup on Cave Wall" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div> 

<strong>// states</strong>

To simulate realistic attenuation, ambience assets were positioned within the scene rather than sounded at the player character. Some ambience assets had diffraction and transmission enabled. 

States were subsequently created to affect the volume and filtering of the ambience actor-mixer as the player moves through different environments (indoors, in-cave, underwater). This unified the ambiences and their specific attenuation properties into a cohesive whole.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_states.png" title="States and their effects" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

<strong>// rooms and portals</strong>

I opted to use AkRooms instead of the AkEnvironments native to the Demo Scene. This way, I could have room coupling occur between an exterior reverb auxiliary bus (priority = 0 in corresponding AkRoom component) and the various interior reverb buses (priority = 1). 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_auxbuses.png" title="All Reverb and ER buses" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

<strong>// attenuation, obstruction, occlusion</strong>

Custom attenuation sharesets were made for certain emitters to fit their nature (e.g. campfire, caveglow, ambience). The project obstruction / occlusion settings were also adapted for a more realistic approach to the default.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_sharesets.png" title="Attenuation ShareSets" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_obstruction_volume.png" title="Project Obstruction Volume Settings" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_obstruction_lpf.png" title="Project Obstruction LPF Settings" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_occlusion_volume.png" title="Project Occlusion Volume Settings" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/screenshots/unity/spatialaudio_occlusion_lpf.png" title="Project Occlusion LPF Settings" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
