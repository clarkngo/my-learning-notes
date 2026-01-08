---
title: Intro Prompt Design
layout: default
has_toc: false
parent: Prompt Engineering
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


```python
# Copyright 2025 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
```

# Prompt Design - Best Practices

<table align="left">
  <td style="text-align: center">
    <a href="https://colab.research.google.com/github/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb">
      <img width="32px" src="https://www.gstatic.com/pantheon/images/bigquery/welcome_page/colab-logo.svg" alt="Google Colaboratory logo"><br> Open in Colab
    </a>
  </td>
  <td style="text-align: center">
    <a href="https://console.cloud.google.com/vertex-ai/colab/import/https:%2F%2Fraw.githubusercontent.com%2FGoogleCloudPlatform%2Fgenerative-ai%2Fmain%2Fgemini%2Fprompts%2Fintro_prompt_design.ipynb">
      <img width="32px" src="https://lh3.googleusercontent.com/JmcxdQi-qOpctIvWKgPtrzZdJJK-J3sWE1RsfjZNwshCFgE_9fULcNpuXYTilIR2hjwN" alt="Google Cloud Colab Enterprise logo"><br> Open in Colab Enterprise
    </a>
  </td>
  <td style="text-align: center">
    <a href="https://console.cloud.google.com/vertex-ai/workbench/deploy-notebook?download_url=https://raw.githubusercontent.com/GoogleCloudPlatform/generative-ai/main/gemini/prompts/intro_prompt_design.ipynb">
      <img src="https://www.gstatic.com/images/branding/gcpiconscolors/vertexai/v1/32px.svg" alt="Vertex AI logo"><br> Open in Workbench
    </a>
  </td>
  <td style="text-align: center">
    <a href="https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb">
      <img width="32px" src="https://www.svgrepo.com/download/217753/github.svg" alt="GitHub logo"><br> View on GitHub
    </a>
  </td>
  <td style="text-align: center">
    <a href="https://goo.gle/4fWHlze">
      <img width="32px" src="https://cdn.qwiklabs.com/assets/gcp_cloud-e3a77215f0b8bfa9b3f611c0d2208c7e8708ed31.svg" alt="Google Cloud logo"><br> Open in  Cloud Skills Boost
    </a>
  </td>
</table>

<div style="clear: both;"></div>

<b>Share to:</b>

<a href="https://www.linkedin.com/sharing/share-offsite/?url=https%3A//github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb" target="_blank">
  <img width="20px" src="https://upload.wikimedia.org/wikipedia/commons/8/81/LinkedIn_icon.svg" alt="LinkedIn logo">
</a>
<a href="https://bsky.app/intent/compose?text=https%3A//github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb" target="_blank">
  <img width="20px" src="https://upload.wikimedia.org/wikipedia/commons/7/7a/Bluesky_Logo.svg" alt="Bluesky logo">
</a>
<a href="https://twitter.com/intent/tweet?url=https%3A//github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb" target="_blank">
  <img width="20px" src="https://upload.wikimedia.org/wikipedia/commons/5/5a/X_icon_2.svg" alt="X logo">
</a>
<a href="https://reddit.com/submit?url=https%3A//github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb" target="_blank">
  <img width="20px" src="https://redditinc.com/hubfs/Reddit%20Inc/Brand/Reddit_Logo.png" alt="Reddit logo">
</a>
<a href="https://www.facebook.com/sharer/sharer.php?u=https%3A//github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/prompts/intro_prompt_design.ipynb" target="_blank">
  <img width="20px" src="https://upload.wikimedia.org/wikipedia/commons/5/51/Facebook_f_logo_%282019%29.svg" alt="Facebook logo">
</a>

| Authors |
| --- |
| [Polong Lin](https://github.com/polong-lin) |
| [Karl Weinmeister](https://github.com/kweinmeister) |

## Overview

This notebook covers the essentials of prompt engineering, including some best practices.

Learn more about prompt design in the [official documentation](https://cloud.google.com/vertex-ai/docs/generative-ai/text/text-overview).

In this notebook, you learn best practices around prompt engineering -- how to design prompts to improve the quality of your responses.

This notebook covers the following best practices for prompt engineering:

- Be concise
- Be specific and well-defined
- Ask one task at a time
- Turn generative tasks into classification tasks
- Improve response quality by including examples

## Getting Started

### Install Google Gen AI SDK



```python
%pip install --upgrade --quiet google-genai
```

### Import libraries



```python
from IPython.display import Markdown, display
from google import genai
from google.genai.types import GenerateContentConfig
```

### Set Google Cloud project information and create client

To get started using Vertex AI, you must have an existing Google Cloud project and [enable the Vertex AI API](https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com).

Learn more about [setting up a project and a development environment](https://cloud.google.com/vertex-ai/docs/start/cloud-environment).


```python
import os

PROJECT_ID = "qwiklabs-gcp-02-e5495ee5184b"
LOCATION = os.environ.get("GOOGLE_CLOUD_REGION", "global")

client = genai.Client(vertexai=True, project=PROJECT_ID, location=LOCATION)

```

### Load model

Learn more about all [Gemini models on Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/models#gemini-models).


```python
MODEL_ID = "gemini-2.5-flash"  # @param {type: "string"}
```

## Prompt engineering best practices

Prompt engineering is all about how to design your prompts so that the response is what you were indeed hoping to see.

The idea of using "unfancy" prompts is to minimize the noise in your prompt to reduce the possibility of the LLM misinterpreting the intent of the prompt. Below are a few guidelines on how to engineer "unfancy" prompts.

In this section, you'll cover the following best practices when engineering prompts:

* Be concise
* Be specific, and well-defined
* Ask one task at a time
* Improve response quality by including examples
* Turn generative tasks to classification tasks to improve safety

### Be concise

🛑 Not recommended. The prompt below is unnecessarily verbose.


```python
prompt = "What do you think could be a good name for a flower shop that specializes in selling bouquets of dried flowers more than fresh flowers?"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


That's a lovely niche! Dried flowers evoke a sense of timelessness, rustic charm, and enduring beauty. Here are some name ideas, categorized by their vibe:

---

**Direct & Descriptive (Clear & Concise):**

1.  **The Dried Bouquet Co.**
2.  **Everlasting Blooms & Bouquets**
3.  **Preserved Petals**
4.  **Timeless Blooms Studio**
5.  **The Cured Blossom**
6.  **Dried Flower Atelier**
7.  **Forever Flora**
8.  **The Enduring Stem**

---

**Evocative & Poetic (Focus on Mood & Beauty):**

9.  **Whispering Petals** (Suggests delicate, quiet beauty)
10. **Ember Bloom** (Like the lasting glow of embers)
11. **Sun-Kissed Stems** (Highlights the drying process)
12. **Air & Earth Botanicals** (Emphasizes natural elements)
13. **Memory Bloom** (Connects to nostalgia and lasting moments)
14. **Heirloom Blooms** (Suggests vintage, cherished beauty)
15. **Aged Petals & Pine** (Adds a rustic, woody element)
16. **Perpetual Petal Co.**
17. **Autumnal Artistry** (If your style leans towards fall colors/textures)

---

**Modern & Chic (Sleek & Stylish):**

18. **The Dried Co.**
19. **Bloom & Dry**
20. **Ever Bloom Studio**
21. **Stem & Stone** (Stone for lasting, natural elements)
22. **Aridus Botanicals** (Aridus is Latin for dry)
23. **Muted Bloom Co.** (Highlights the softened colors)
24. **The Preservationist** (Emphasizes the craft)
25. **Terra Flora Dried** (Terra = Earth)

---

**Rustic & Earthy (Natural & Wholesome):**

26. **Harvest & Hearth Blooms**
27. **Rustic Bloom Collective**
28. **The Dried Garden**
29. **Earthen Petal**
30. **Homestead Blooms**
31. **The Curing House Botanicals**

---

**Tips for Choosing:**

*   **Say it aloud:** Does it roll off the tongue?
*   **Check availability:** Is the name available as a domain name, on social media, and legally for business registration?
*   **Consider your target audience:** Are they looking for rustic, elegant, modern, or whimsical?
*   **Reflect your aesthetic:** Does the name match the look and feel of your bouquets and branding?
*   **Keep it memorable:** Short and distinct names are often easier to recall.

Good luck with your flower shop!


✅ Recommended. The prompt below is to the point and concise.


```python
prompt = "Suggest a name for a flower shop that sells bouquets of dried flowers"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


Here are some name suggestions for a flower shop specializing in dried flower bouquets, categorized by their vibe:

**Evocative & Poetic:**

1.  **Everbloom Botanicals:** Suggests lasting beauty.
2.  **Timeless Petals:** Emphasizes enduring charm.
3.  **The Memory Bloom:** Connects to keepsakes and sentiments.
4.  **Whispering Stems:** Gentle, ethereal, and a bit magical.
5.  **Faded Flora Co.:** Embraces the unique beauty of dried flowers.
6.  **Eternal Roots:** Combines longevity with nature.
7.  **Sun-Drenched Stems:** Evokes warmth and the drying process.
8.  **Muted Florals:** Highlights the soft, natural palette.
9.  **The Enduring Arrangement:** Focuses on the lasting quality of the bouquets.
10. **Heirloom Bloom:** Suggests cherished, passed-down beauty.

**Chic & Modern:**

11. **Bloom & Preserve:** Direct and stylish.
12. **Dried. (or D R I E D.):** Minimalist and bold.
13. **The Floral Archivist:** Positions the shop as curating beauty.
14. **Botanical Keepsakes:** Elegant and descriptive.
15. **Form & Flora Dried:** Highlights design and nature.
16. **The Preserved Collective:** Suggests curated collections.
17. **Stems & Stone:** Implies longevity and natural elements.
18. **Curated Drieds:** Emphasizes careful selection.

**Rustic & Whimsical:**

19. **The Dusty Bloom:** Charming, unique, and a nod to their nature.
20. **Wild Dried Petals:** Evokes a natural, untamed feel.
21. **Rustic Rose & Thistle:** Classic rustic elements.
22. **Grandma's Garden Dried:** A nostalgic, comforting feel.
23. **The Petal Pantry:** Cute and suggests a storehouse of treasures.
24. **Terra Bloom:** Combines earthiness with florals.
25. **The Dried Herbarium:** A sophisticated, natural history vibe.

**Direct & Descriptive:**

26. **The Dried Flower Shop:** Clear and straightforward.
27. **Everlasting Bouquets:** Exactly what you offer.
28. **Preserved Petals Co.**
29. **Dried Flower Delights:** Adds a positive spin.
30. **Botanical Everlastings:** Professional and descriptive.

**Tips for Choosing:**

*   **Say it out loud:** Does it roll off the tongue?
*   **Check availability:** Is the name (and domain name/social media handles) available?
*   **Consider your target audience:** Are they looking for rustic, modern, or whimsical?
*   **Keep it memorable:** Easy to spell and recall.
*   **Reflect your brand:** What feeling do you want customers to have?

Good luck with your new venture!


### Be specific, and well-defined

Suppose that you want to brainstorm creative ways to describe Earth.

🛑 The prompt below might be a bit too generic (which is certainly OK if you'd like to ask a generic question!)


```python
prompt = "Tell me about Earth"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


Earth is a truly remarkable and unique planet, often called "the Blue Marble" due to its abundance of water. It's the only known celestial body to harbor life, making it a subject of endless scientific fascination and profound cultural significance.

Here's a detailed look at our home planet:

---

### **1. Our Cosmic Home**

*   **Third Planet from the Sun:** Earth orbits the Sun at an average distance of about 150 million kilometers (93 million miles), a distance that places it in the "Goldilocks Zone" – not too hot, not too cold – perfect for liquid water.
*   **Terrestrial Planet:** Like Mercury, Venus, and Mars, Earth is a rocky, inner planet with a solid surface.
*   **Fifth Largest Planet:** Among the eight planets in our Solar System, Earth is the fifth largest.
*   **Name Origin:** "Earth" is derived from both English and German words, "eorþe" and "erde," respectively, meaning "ground" or "soil." It's the only planet not named after a Greek or Roman deity.

---

### **2. Physical Characteristics**

*   **Size and Shape:** Earth is an **oblate spheroid**, meaning it bulges slightly at the equator and is flattened at the poles due to its rotation. Its equatorial diameter is about 12,756 km (7,926 miles), while its polar diameter is 12,714 km (7,900 miles).
*   **Atmosphere:** A relatively thin, gaseous envelope composed primarily of:
    *   **Nitrogen (N2):** ~78%
    *   **Oxygen (O2):** ~21%
    *   **Argon (Ar):** ~0.93%
    *   **Carbon Dioxide (CO2):** ~0.04%
    *   Other trace gases, including water vapor.
    The atmosphere is crucial for life, providing breathable air, protecting us from harmful solar radiation, regulating temperature, and creating weather patterns.
*   **Internal Structure:** Earth is layered, like an onion:
    *   **Crust:** The thin, outermost solid layer (5-70 km thick). Composed of lighter silicates.
    *   **Mantle:** A thick, viscous layer (2,900 km thick) mostly of solid rock, but capable of flowing slowly over geological time due to convection currents.
    *   **Outer Core:** A liquid layer (2,200 km thick) primarily of iron and nickel. Its convection creates Earth's magnetic field.
    *   **Inner Core:** A solid ball (1,220 km radius) of iron and nickel, incredibly hot (similar to the Sun's surface) but solid due to immense pressure.
*   **Surface Features:** Approximately 71% of Earth's surface is covered by **liquid water** (oceans, lakes, rivers), and 29% is **land** (continents and islands). Its surface is dynamic, featuring mountains, valleys, plains, deserts, forests, and ice caps, all shaped by geological processes like **plate tectonics**, erosion, and volcanic activity.

---

### **3. The Uniqueness of Earth**

*   **Liquid Water:** Earth is the only known planet to have stable bodies of liquid water on its surface. Water is essential for all known life forms and drives the planet's hydrological cycle, influencing climate and weather.
*   **Life:** This is Earth's most defining characteristic. From microscopic bacteria to towering trees and massive whales, Earth hosts a breathtaking **biodiversity** across countless ecosystems. The interactions between living organisms and their environment have profoundly shaped the planet's history and continue to do so.
*   **Magnetic Field (Magnetosphere):** Generated by the churning liquid iron in the outer core, this powerful magnetic field extends far into space, deflecting harmful charged particles from the Sun (solar wind) and cosmic rays. Without it, our atmosphere would likely be stripped away, making life impossible. The beautiful **aurora borealis and australis** are visible manifestations of this interaction.
*   **Its Moon:** Earth has one relatively large natural satellite, the Moon. It stabilizes Earth's axial tilt, preventing drastic climate shifts, and creates tides in our oceans, which are vital for many coastal ecosystems.

---

### **4. Motions and Seasons**

*   **Rotation:** Earth spins on its axis, completing one rotation approximately every 24 hours, giving us day and night.
*   **Revolution:** Earth orbits the Sun, completing one revolution approximately every 365.25 days, which defines a year.
*   **Axial Tilt:** Earth's axis is tilted at about 23.5 degrees relative to its orbital plane. This tilt is responsible for the **seasons**, as different parts of the planet receive more direct sunlight at different times of the year.

---

### **5. Composition**

Overall, Earth is primarily composed of **iron (32%), oxygen (30%), silicon (15%), magnesium (14%), sulfur (3%), nickel (2%), calcium (1.5%), and aluminum (1.4%)**, with trace amounts of other elements.

---

### **6. Humanity's Impact and Role**

As the dominant intelligent species, humans have profoundly impacted Earth. Our activities, including industrialization, deforestation, agriculture, and burning fossil fuels, have led to significant challenges such as **climate change, pollution, deforestation, habitat loss, and biodiversity extinction**. Understanding and mitigating these impacts is crucial for the continued health and habitability of our planet.

---

In essence, Earth is a vibrant, dynamic, and incredibly complex system, perfectly tuned to sustain a dazzling array of life forms. It is our shared home, a precious oasis in the vastness of space, demanding our respect, understanding, and careful stewardship.


✅ Recommended. The prompt below is specific and well-defined.


```python
prompt = "Generate a list of ways that makes Earth unique compared to other planets"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


Earth stands out in our solar system, and as far as we know, in the observable universe, due to a unique combination of factors that have allowed for the emergence and sustenance of complex life. Here are some key ways Earth is unique compared to other planets:

1.  **Abundance of Surface Liquid Water:** Earth is the only planet known to harbor vast, stable bodies of liquid water on its surface. While other bodies have ice (Mars, Europa) or subsurface oceans (Europa, Enceladus), Earth's oceans, rivers, and lakes are crucial for life and regulate climate.

2.  **Oxygen-Rich Atmosphere:** Unlike the CO2-dominated atmospheres of Venus and Mars, or the hydrogen/helium atmospheres of the gas giants, Earth's atmosphere is ~21% oxygen. This is a direct result of biological activity (photosynthesis) and is essential for most complex life forms.

3.  **Active Plate Tectonics:** Earth is the only planet in our solar system with active, large-scale plate tectonics. This process recycles crust, creates continents and mountain ranges, drives volcanism, and plays a vital role in regulating the planet's climate over geological timescales through the carbon-silicate cycle.

4.  **Global Magnetic Field (Magnetosphere):** Earth has a strong, global magnetic field generated by its molten iron outer core. This magnetosphere deflects harmful solar wind and cosmic radiation, preventing atmospheric stripping and protecting life on the surface. No other rocky planet in our solar system has such a robust and active global magnetic shield.

5.  **Presence of Complex, Diverse, and Abundant Life:** While the search for extraterrestrial life continues, Earth is the only planet definitively known to host life, from microscopic organisms to intelligent beings. This life has profoundly altered the planet's geology and atmosphere.

6.  **"Goldilocks Zone" Location and Stable Climate:** Earth orbits within the Sun's habitable zone, where temperatures are "just right" for liquid water to exist on the surface. Furthermore, its orbit is relatively stable and its axial tilt contributes to moderate, predictable seasons, avoiding extreme temperature swings.

7.  **Large, Stabilizing Moon:** Earth's Moon is unusually large relative to its planet. It exerts significant tidal forces, which may have played a role in early life, but more importantly, it stabilizes Earth's axial tilt, preventing dramatic shifts in climate that could otherwise occur over millions of years.

8.  **Water Cycle:** The continuous process of evaporation, condensation, precipitation, and runoff involving liquid, solid, and gaseous water is unique to Earth and fundamental to its climate and ecosystems.

9.  **Stratospheric Ozone Layer:** While part of the oxygen-rich atmosphere, the ozone layer deserves special mention. It effectively blocks most of the Sun's harmful ultraviolet (UV) radiation from reaching the surface, which was crucial for life to colonize land.

These unique characteristics are interconnected, forming a complex system that has made Earth a cradle for life as we know it.


### Ask one task at a time

🛑 Not recommended. The prompt below has two parts to the question that could be asked separately.


```python
prompt = "What's the best method of boiling water and why is the sky blue?"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


That's a fun combination of practical and scientific! Let's break them down.

---

### What's the best method of boiling water?

The "best" method depends on your priorities: **speed, efficiency, volume, convenience, and energy source.**

1.  **For Speed & Efficiency (Small to Medium Volumes - 1 to 2 Liters): Electric Kettle**
    *   **Why it's often best:** Electric kettles are designed specifically for boiling water. They have a heating element directly immersed in the water, which transfers heat very efficiently. They also often have automatic shut-off features, making them convenient and safe.
    *   **Pros:** Very fast, energy-efficient for their task, safe (auto-shutoff), convenient.
    *   **Cons:** Requires electricity, not suitable for very large volumes.

2.  **For Larger Volumes or Cooking: Stovetop (Induction or Gas)**
    *   **Induction Cooktop:**
        *   **Why it's great:** Induction heating is very efficient because it directly heats the pot itself (using electromagnetism), minimizing heat loss to the surroundings. It's often very fast.
        *   **Pros:** Fast, highly energy-efficient (if you have induction-compatible cookware), precise temperature control.
        *   **Cons:** Requires special ferromagnetic cookware, generally more expensive initial investment.
    *   **Gas Stovetop:**
        *   **Why it's good:** Gas flames provide strong, direct heat and allow for easy visual control. They are responsive and reliable.
        *   **Pros:** Fast, responsive, works during power outages, common.
        *   **Cons:** Less energy-efficient than induction (some heat escapes around the pot), produces combustion byproducts (needs ventilation).

3.  **For Portability / Outdoors: Camp Stove or Campfire**
    *   **Why it's essential:** When off-grid, these are your primary options. Propane/butane camp stoves are surprisingly efficient and fast for small volumes.
    *   **Pros:** Portable, self-contained energy source (fuel canisters), robust.
    *   **Cons:** Requires fuel, slower and less efficient than electric methods, open flame risks.

**General Tips for Efficient Boiling (regardless of method):**

*   **Use a lid:** This traps heat and steam, significantly speeding up the boiling process and reducing energy consumption.
*   **Only boil what you need:** Boiling excess water wastes energy and time.
*   **Use the right pot size:** A pot that's too large will take longer to heat up, while one that's too small might boil over.
*   **Start with hot water (if available):** If your tap water is already hot, it will boil faster. (Though for drinking, cold tap water is usually recommended as it's less likely to contain dissolved minerals or contaminants from the hot water heater).

**In summary:** For typical home use and convenience, an **electric kettle** is hard to beat for speed and efficiency for small to medium amounts. For serious cooking or larger volumes, an **induction stovetop** or a good **gas stovetop** is excellent.

---

### Why is the sky blue?

The sky is blue primarily due to a phenomenon called **Rayleigh Scattering**. Here's how it works:

1.  **Sunlight is White:** Sunlight is made up of all the colors of the rainbow (red, orange, yellow, green, blue, indigo, violet – ROYGBIV). When combined, these colors appear white.

2.  **Earth's Atmosphere:** Our atmosphere is composed of tiny molecules of gases, primarily nitrogen (N2) and oxygen (O2), along with other trace gases and particles.

3.  **Scattering of Light:** As sunlight enters the atmosphere, it collides with these tiny gas molecules. When light hits these molecules, it gets scattered in all directions.

4.  **Wavelength Matters (Rayleigh Scattering):** The key is that the amount of scattering depends on the **wavelength** of the light and the **size of the particles** it hits. For particles much smaller than the wavelength of light (like our atmospheric gas molecules and visible light), shorter wavelengths (bluer colors) are scattered much more effectively than longer wavelengths (redder colors).
    *   **Blue and Violet light** have shorter wavelengths, so they are scattered about 10 times more efficiently than red light.
    *   **Red and Orange light** have longer wavelengths, so they mostly pass straight through the atmosphere without much scattering.

5.  **Why We See Blue:**
    *   When sunlight reaches our atmosphere, the blue and violet light is scattered in every direction. This scattered blue light reaches our eyes from all parts of the sky, making the sky appear blue.
    *   While violet light is scattered even more than blue, our eyes are more sensitive to blue light, and there's also slightly less violet light in the sun's spectrum to begin with. The combination of these factors makes the sky appear distinctly blue to us.

**What about other colors?**

*   **Sunrise and Sunset:** When the sun is low on the horizon, its light has to travel through a much thicker slice of atmosphere to reach our eyes. By the time it gets to us, most of the blue and violet light has been scattered away, leaving mostly the longer wavelengths (red, orange, yellow) to pass through directly. This is why we see beautiful reds and oranges during sunrise and sunset.
*   **Clouds:** Clouds are made of water droplets or ice crystals that are much larger than the wavelengths of visible light. These larger particles scatter *all* wavelengths of light equally (a process called Mie scattering), which is why clouds often appear white or gray.
*   **Space:** In space, there's no atmosphere to scatter light, so the "sky" (or rather, the void) appears black.


✅ Recommended. The prompts below asks one task a time.


```python
prompt = "What's the best method of boiling water?"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


There isn't a single "best" method for boiling water, as the ideal choice depends on your priorities: **speed, energy efficiency, convenience, volume, and available equipment.**

Here's a breakdown of common methods, highlighting their pros and cons:

### Top Contenders for Most Situations:

1.  **Electric Kettle**
    *   **Pros:**
        *   **Speed:** Often the fastest method for small to medium volumes (1-2 liters), especially models with higher wattage.
        *   **Energy Efficiency:** Very efficient as the heating element is directly in contact with the water, and most have auto-shutoff.
        *   **Convenience:** Simple to use, portable, and doesn't tie up a stovetop burner.
        *   **Safety:** Auto-shutoff prevents boiling dry.
    *   **Cons:**
        *   Requires electricity.
        *   Capacity is limited.
        *   Initial purchase cost.
    *   **Best for:** Everyday use, tea, coffee, instant noodles, quick cooking prep.

2.  **Induction Stovetop**
    *   **Pros:**
        *   **Speed:** Extremely fast, often comparable to or even faster than electric kettles for larger volumes, as heat is directly generated in the pot.
        *   **Energy Efficiency:** Highly efficient due to direct magnetic heating, minimal heat loss to the surroundings.
        *   **Control:** Precise temperature control.
        *   **Safety:** The cooktop itself doesn't get as hot as gas or electric, reducing burn risk.
    *   **Cons:**
        *   Requires induction-compatible cookware (magnetic bottom).
        *   Higher upfront cost for the cooktop itself.
    *   **Best for:** Boiling larger volumes quickly and efficiently, home chefs, modern kitchens.

### Other Common Methods:

3.  **Gas Stovetop**
    *   **Pros:**
        *   **Speed:** Good speed, especially with a powerful burner and a good pot.
        *   **Control:** Excellent visual control over flame intensity.
        *   **Versatility:** Works with any type of cookware.
        *   **Availability:** Very common in many homes.
    *   **Cons:**
        *   **Energy Efficiency:** Less efficient than electric kettles or induction due to heat loss around the pot.
        *   **Safety:** Open flame, potential for gas leaks.
    *   **Best for:** Boiling larger volumes, general cooking, kitchens with gas hookups.

4.  **Electric Coil/Radiant Stovetop**
    *   **Pros:**
        *   **Versatility:** Works with any type of cookware.
        *   **Availability:** Very common.
    *   **Cons:**
        *   **Speed:** Generally the slowest stovetop method.
        *   **Energy Efficiency:** Least efficient of the stovetop types, as the element heats, then the air, then the pot.
        *   **Safety:** The element stays hot for a long time after being turned off.
    *   **Best for:** If it's your only stovetop option, or you're not in a hurry.

5.  **Microwave**
    *   **Pros:**
        *   **Convenience:** No dedicated appliance needed beyond the microwave itself.
        *   **Small Volumes:** Quick for a single cup of water.
    *   **Cons:**
        *   **Safety Risk (Superheating):** Water can heat beyond its boiling point without bubbling, then suddenly boil violently when disturbed (e.g., adding a tea bag). Use caution and stir before removing.
        *   **Energy Efficiency:** Not very efficient for boiling, as the energy heats the entire oven cavity.
        *   **No Rolling Boil:** Doesn't produce a vigorous, uniform boil.
        *   **Slow for Larger Volumes:** Very inefficient for more than a cup or two.
    *   **Best for:** A single cup of water in a pinch, in a microwave-safe container with caution.

### Situational Methods:

*   **Immersion Heater:** Great for travel or camping where an electric kettle isn't practical. Be extremely careful with safety, ensuring the element is fully submerged and avoiding contact with the cord/handle.
*   **Boiling Water Dispenser (Instant Hot Water Tap):** Offers immediate hot water, great for convenience, but the tank constantly uses energy to keep water hot, making it less energy-efficient if not used frequently.
*   **Campfire/Outdoor Stove:** Essential for off-grid situations, but slow, less efficient, and requires fuel management.

### Tips for Faster & More Efficient Boiling (Regardless of Method):

*   **Use a Lid:** This is the #1 tip! A lid traps steam and heat, drastically reducing boiling time and energy consumption.
*   **Heat Only What You Need:** Don't boil a full kettle if you only need one cup.
*   **Start with Hot Tap Water (if safe):** If your tap water gets hot quickly and is safe for consumption, starting with hot water can shave off some time.
*   **Use the Right Size Pot/Kettle:** A wider base allows for more contact with the heat source.
*   **Clean Your Kettle:** Mineral buildup (limescale) on electric kettle elements can reduce efficiency.

### Conclusion:

For most households, an **electric kettle** offers the best balance of **speed, energy efficiency, and convenience** for small to medium volumes. For larger volumes or a premium kitchen experience, an **induction stovetop** is hard to beat for its speed and efficiency.



```python
prompt = "Why is the sky blue?"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


The sky is blue because of a phenomenon called **Rayleigh scattering**. Here's a breakdown:

1.  **Sunlight is White Light:** Sunlight, when it reaches Earth, appears white. However, it's actually made up of all the colors of the rainbow, each with a different wavelength (red has the longest wavelength, violet the shortest).

2.  **Earth's Atmosphere:** Our atmosphere is composed of various gases, primarily nitrogen (about 78%) and oxygen (about 21%), along with tiny particles.

3.  **Scattering of Light:** When sunlight enters the atmosphere, it collides with these tiny gas molecules and particles. This collision causes the light to be scattered (redirected) in all directions.

4.  **Wavelength Matters (Rayleigh Scattering):** The key is that the amount of scattering depends on the wavelength of the light.
    *   **Shorter wavelengths** (like blue and violet light) are scattered much *more effectively* than longer wavelengths (like red and yellow light). In fact, blue light is scattered about 10 times more than red light.
    *   This is because the gas molecules in the atmosphere are much smaller than the wavelength of visible light, making them more efficient at scattering the shorter, "bluer" wavelengths.

5.  **Why We See Blue:**
    *   When we look up at the sky *away from the sun*, we are seeing the sunlight that has been scattered by the atmosphere.
    *   Because blue and violet light are scattered the most, these colors are spread across the sky, making it appear blue to our eyes.
    *   While violet light is scattered even more than blue, there are two reasons we perceive blue:
        *   The sun emits less violet light than blue light.
        *   Our eyes are more sensitive to blue light than to violet light.

**What about other colors?**

*   The longer wavelengths (red, orange, yellow) are not scattered as much and tend to travel more directly through the atmosphere. This is why, when the sun is low on the horizon (sunrise or sunset), its light has to travel through much more atmosphere. Most of the blue light has been scattered away, leaving the reds, oranges, and yellows to reach our eyes directly, giving us those beautiful colorful sunsets.

In summary, the sky is blue because the Earth's atmosphere is very good at scattering blue light in all directions, making it the dominant color we see.


### Watch out for hallucinations

Although LLMs have been trained on a large amount of data, they can generate text containing statements not grounded in truth or reality; these responses from the LLM are often referred to as "hallucinations" due to their limited memorization capabilities. Note that simply prompting the LLM to provide a citation isn't a fix to this problem, as there are instances of LLMs providing false or inaccurate citations. Dealing with hallucinations is a fundamental challenge of LLMs and an ongoing research area, so it is important to be cognizant that LLMs may seem to give you confident, correct-sounding statements that are in fact incorrect.

Note that if you intend to use LLMs for the creative use cases, hallucinating could actually be quite useful.

Try the prompt like the one below repeatedly. We set the temperature to `1.0` so that it takes more risks in its choices. It's possible that it may provide an inaccurate, but confident answer.


```python
generation_config = GenerateContentConfig(temperature=1.0)

prompt = "What day is it today?"

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


Today is **Wednesday**.


Since LLMs do not have access to real-time information without further integrations, you may have noticed it hallucinates what day it is today in some of the outputs.

### Using system instructions to guardrail the model from irrelevant responses

How can we attempt to reduce the chances of irrelevant responses and hallucinations?

One way is to provide the LLM with [system instructions](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/prompts/system-instruction-introduction).

Let's see how system instructions works and how you can use them to reduce hallucinations or irrelevant questions for a travel chatbot.

Suppose we ask a simple question about one of Italy's most famous tourist spots.


```python
generation_config = GenerateContentConfig(temperature=1.0)

chat = client.chats.create(
    model=MODEL_ID,
    config=GenerateContentConfig(
        system_instruction=[
            "Hello! You are an AI chatbot for a travel web site.",
            "Your mission is to provide helpful queries for travelers.",
            "Remember that before you answer a question, you must check to see if it complies with your mission.",
            "If not, you can say, Sorry I can't answer that question.",
        ]
    ),
)

prompt = "What is the best place for sightseeing in Milan, Italy?"

response = chat.send_message(prompt)
display(Markdown(response.text))
```


Milan offers a wealth of incredible sightseeing opportunities! While "best" can be subjective and depend on your interests, a very popular and iconic starting point that many travelers consider a must-see is the **Duomo di Milano (Milan Cathedral)**.

Here's why it's highly recommended for sightseeing:
*   **Architectural Marvel:** It's one of the largest and most magnificent Gothic cathedrals in the world, with intricate details, countless statues, and stunning stained glass windows.
*   **Rooftop Views:** You can take an elevator or stairs to the Duomo terraces for breathtaking panoramic views of Milan, including the Galleria Vittorio Emanuele II and even the Alps on a clear day.
*   **Central Location:** It's situated in Piazza del Duomo, a vibrant central square from which you can easily access other key attractions like the Galleria Vittorio Emanuele II and the La Scala Opera House.
*   **Historical Significance:** It has a rich history spanning over six centuries of construction.

I highly recommend dedicating a good portion of your day to exploring the Duomo, both inside and on its rooftop!


Now let us pretend to be a user asks the chatbot a question that is unrelated to travel.


```python
prompt = "How do I make pizza dough at home?"

response = chat.send_message(prompt)
display(Markdown(response.text))
```


Sorry, I can't answer that question. My purpose is to provide helpful queries for travelers.


You can see that this way, a guardrail in the prompt prevented the chatbot from veering off course.

### Turn generative tasks into classification tasks to reduce output variability

#### Generative tasks lead to higher output variability

The prompt below results in an open-ended response, useful for brainstorming, but response is highly variable.


```python
prompt = "I'm a high school student. Recommend me a programming activity to improve my skills."

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```


That's fantastic! The best way to improve is by *building* things. Here are a few programming activity recommendations, ranging in complexity, with an emphasis on tangible results and practical skills:

---

### Activity 1: Build a Text-Based Adventure Game (or Classic Game Clone)

This is a classic for a reason – it teaches core programming concepts in a fun, engaging way.

*   **What you'll learn:**
    *   **Variables & Data Structures:** Storing player inventory, room descriptions, character stats (lists, dictionaries).
    *   **Conditionals (`if`/`else`):** Handling player choices ("go north," "pick up item").
    *   **Loops (`while`):** The main game loop that keeps the game running until an end condition.
    *   **Functions:** Organizing your code (e.g., `display_room()`, `handle_input()`, `check_win_condition()`).
    *   **User Input/Output:** Getting commands from the player and printing descriptive text.
    *   **Basic Logic & Problem Solving:** Designing game flow, puzzles, and consequences.
*   **Suggested Tools/Languages:** Python is excellent for this due to its readability and ease of use.
*   **Project Ideas:**
    *   **Text Adventure:** A simple "escape the room" or "find the treasure" game.
    *   **Hangman:** Randomly pick a word, let the user guess letters.
    *   **Tic-Tac-Toe:** Two players take turns, check for win conditions.
    *   **Guess the Number:** Computer picks a number, user tries to guess it with hints.
*   **Stretch Goals:** Add more complex inventory systems, combat, multiple paths, saving/loading game state.

---

### Activity 2: Create a Personal Website/Portfolio

This is incredibly practical and a great way to showcase your projects.

*   **What you'll learn:**
    *   **HTML:** The structure of web pages.
    *   **CSS:** Styling and making your website look good (colors, fonts, layout).
    *   **Basic JavaScript:** Adding interactivity (e.g., a "dark mode" toggle, a simple image carousel, form validation).
    *   **Web Hosting/Deployment:** Getting your website online for others to see (GitHub Pages is a great free option for static sites).
    *   **User Interface (UI) / User Experience (UX) Basics:** Thinking about how people interact with your site.
*   **Suggested Tools/Languages:** HTML, CSS, JavaScript. No special software needed, just a text editor and a browser.
*   **Project Ideas:**
    *   A simple "About Me" page with your interests and contact info.
    *   A portfolio page to display your other coding projects (even just screenshots and descriptions).
    *   A blog where you write about your learning journey.
*   **Stretch Goals:** Learn a JavaScript framework (like React or Vue.js for more complex interactions), explore responsive design (making it look good on phones), add a simple backend with Python (Flask/Django) or Node.js (Express) to handle forms or dynamic content.

---

### Activity 3: Automate a Repetitive Task

This activity shows the power of programming to make your life easier!

*   **What you'll learn:**
    *   **File I/O:** Reading from and writing to files.
    *   **Operating System Interaction:** Moving, renaming, deleting files/folders.
    *   **Web Scraping (Optional):** Extracting data from websites.
    *   **APIs (Optional):** Interacting with online services (e.g., weather, social media).
    *   **Error Handling:** What to do when things go wrong.
    *   **Scheduling:** Running your script automatically.
*   **Suggested Tools/Languages:** Python is king here, with libraries like `os`, `shutil`, `requests`, `BeautifulSoup` (for web scraping), `smtplib` (for email).
*   **Project Ideas:**
    *   **File Organizer:** A script that sorts files in your Downloads folder into subfolders based on file type (e.g., all `.pdf` files go into a "PDFs" folder).
    *   **Web Scraper:** Grab the latest headlines from a news site, or track the price of an item you want to buy.
    *   **Automated Email Sender:** Send a personalized email reminder to yourself or a friend (use this responsibly!).
    *   **Daily Digest:** A script that pulls data from various sources (weather, news, your calendar) and compiles it into a single text file or email.
*   **Stretch Goals:** Build a simple GUI for your automation script, integrate with more complex APIs, create a scheduled task to run your script daily.

---

### Activity 4: Participate in Competitive Programming / Algorithmic Challenges

This is excellent for sharpening your problem-solving skills and understanding efficient code.

*   **What you'll learn:**
    *   **Algorithms:** Sorting, searching, recursion, dynamic programming, graph traversal.
    *   **Data Structures:** Arrays, linked lists, stacks, queues, trees, hash maps.
    *   **Problem-Solving Strategies:** Breaking down complex problems, recognizing patterns.
    *   **Time & Space Complexity:** Understanding how efficient your code is.
    *   **Debugging:** Identifying and fixing errors in complex logic.
*   **Suggested Tools/Languages:** Python, C++, or Java are common choices. You'll use online judges.
*   **Platforms:**
    *   **Codeforces / AtCoder:** For competitive programming contests. Start with Div2A/B problems.
    *   **LeetCode / HackerRank:** Huge libraries of problems, good for practicing specific algorithms or data structures.
    *   **USACO (USA Computing Olympiad):** If you're in the US and serious about competitive programming, this is a great structured path.
*   **Project Ideas:** Start with "easy" problems and gradually work your way up. Focus on understanding the underlying algorithm rather than just memorizing solutions.
*   **Stretch Goals:** Participate in a virtual contest, aim to solve a medium-difficulty problem without hints, learn about advanced data structures (segment trees, Fenwick trees).

---

### General Tips for Success:

1.  **Start Small:** Don't try to build the next Facebook on your first try. Break down big ideas into tiny, manageable steps.
2.  **Use Version Control (Git/GitHub):** Learn how to use Git and create a GitHub account. Upload all your projects there. It's essential for collaboration, showing off your work, and tracking your progress.
3.  **Learn to Debug:** Your code *will* have errors. Learning to effectively use print statements, a debugger, or just systematically thinking through your code is a crucial skill.
4.  **Read Code:** Look at how other people solve similar problems. Open-source projects on GitHub are a treasure trove.
5.  **Ask for Help:** Don't get stuck for hours. Use resources like Stack Overflow, online forums, or even ask a teacher/mentor. Learning how to ask good questions is a skill in itself.
6.  **Stay Curious:** If you encounter a concept you don't understand, look it up! Google is your best friend.
7.  **Have Fun:** Programming should be enjoyable. Pick projects that genuinely interest you.

Good luck! You're on a great path to improving your skills.


#### Classification tasks reduces output variability

The prompt below results in a choice and may be useful if you want the output to be easier to control.


```python
prompt = """I'm a high school student. Which of these activities do you suggest and why:
a) learn Python
b) learn JavaScript
c) learn Fortran
"""

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```

### Improve response quality by including examples

Another way to improve response quality is to add examples in your prompt. The LLM learns in-context from the examples on how to respond. Typically, one to five examples (shots) are enough to improve the quality of responses. Including too many examples can cause the model to over-fit the data and reduce the quality of responses.

Similar to classical model training, the quality and distribution of the examples is very important. Pick examples that are representative of the scenarios that you need the model to learn, and keep the distribution of the examples (e.g. number of examples per class in the case of classification) aligned with your actual distribution.

#### Zero-shot prompt

Below is an example of zero-shot prompting, where you don't provide any examples to the LLM within the prompt itself.


```python
prompt = """Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment:
"""

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```

#### One-shot prompt

Below is an example of one-shot prompting, where you provide one example to the LLM within the prompt to give some guidance on what type of response you want.


```python
prompt = """Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment: positive

Tweet: That was awful. Super boring 😠
Sentiment:
"""

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```

#### Few-shot prompt

Below is an example of few-shot prompting, where you provide a few examples to the LLM within the prompt to give some guidance on what type of response you want.


```python
prompt = """Decide whether a Tweet's sentiment is positive, neutral, or negative.

Tweet: I loved the new YouTube video you made!
Sentiment: positive

Tweet: That was awful. Super boring 😠
Sentiment: negative

Tweet: Something surprised me about this video - it was actually original. It was not the same old recycled stuff that I always see. Watch it - you will not regret it.
Sentiment:
"""

response = client.models.generate_content(model=MODEL_ID, contents=prompt)
display(Markdown(response.text))
```

#### Choosing between zero-shot, one-shot, few-shot prompting methods

Which prompt technique to use will solely depends on your goal. The zero-shot prompts are more open-ended and can give you creative answers, while one-shot and few-shot prompts teach the model how to behave so you can get more predictable answers that are consistent with the examples provided.
